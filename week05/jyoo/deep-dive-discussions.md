## Week05 deep-dive-discussions
1. ConcurrentHashMap이 thread-safe한 이유와 한계는?
   - java 8 이전:
     - segment라는 각각 락을 가진 작은 해시 테이블들로 맵을 나눈 후, 각 키가 어느 세그먼트에 소속되는지 결정하도록함 
       - 이 세그먼트들을 동시 수정하더라도 각각의 세그먼트에 락을 통해 해당 세그먼트만 락이 걸려 수정하는 액션에 대해서는 동시성이 보장이 되었음.
       - 그러나 세그먼트의 수가 객체의 생성시에 고정되며, 동적으로 늘어나지 않는 문제점이 있었어서 이후에 수정됨
   - java 8 이후:
     - 세그먼트는 삭제
     - Compare-And-Swap(CAS) 방식과 synchronized 키워드를 함께 적용한다 -> **atomic 연산을 통해 동시성 문제를 non-blocking 처리를 시도한 후, 불가한 경우에 대해 block/unblock 과정을 걸어 처리한다.**
       - atomic 연산이란?
         - 원자성을 보장하는 연산을 의미함 => 더 이상 쪼개질 수 없는 연산. 실행 도중 다른 스레드가 끼어들 수 없고, 항상 전체가 한번에 수행되거나 아예 수행되지 않는 연산을 의미함.
       - Compare-And-Swap 이란?
         - 특정 메모리 위치에 주어진 값과 동일할 경우, 해당 메모리 주소를 새로운 값으로 교체하는 알고리즘.
           - ConcurrentHashMap의 casTabAt 메서드에서 호출하는 Unsafe.compareAndSetReference에서 해당 알고리즘이 사용됨.
         - 이게 왜 Atomic 연산일까?
           - 일반적으로 특정 값에 접근해 해당 값을 변경하려는 액션의 경우, 하나의 연산을 수행하는 것으로 보이지만, 실상은 여러개의 액션이 혼재한 경우가 많다. (ex: integer++ 의 경우, 하나의 액션만 일어나야 할 것 같지만 1. 값을 읽고 2. +1을 수행하고 3. 값을 쓴다 이렇게 3동작이 수행된다.) 그러나 메모리 주소의 위치를 직접 확인하고 해당 버킷이 비어있으면 직접 넣어버리게 되면 CPU 단일 명령어 수준에서 처리가 되기 떄문이다.
             - 이는 하드웨어 수준에서 원자적인데, CPU의 명령어를 통한 연산이 내부적으로 진행되기 때문이라고 함.
         - CAS 연산이 낳을 수 있는 문제:
           - ABA 문제. CAS가 값이 동일한지만 검사하기 때문에 발생하는 이슈로, 아래와 같은 동시성 문제를 말한다.
             1. 스레드 A가 값 A를 읽음
             2. 다른 스레드 B가 A → B → A로 값을 변경
             3. 스레드 A가 CAS 수행: "현재 값이 A이므로 교체 가능"
             4. 그러나 사실 중간에 값이 바뀌었음 → 논리적으로는 잘못된 상태
           - 이러한 문제가 자바에서도 발생할까?
             - **모든 케이스는 확인하지 못했음 ConcurrentHashMap에서의 경우만 이야기 하는 내용**
             - 삽입 CAS (Unsafe.compareAndSetReference()) 에서는 버킷 단일 참조시에 사용하는 로직이고, 현재 참조가 null 인지만 체크한다 -> 중간에 다른 삽입/삭제가 발생하더라도 명시적으로 null 과의 비교만 하고 있기에 안전하게 처리가 가능.
             - 충돌이 발생하더라도 synchronized 블록에서 동기화 해주기에 ABA문제의 발생 가능성이 낮다고 판단됨.
   - reference:
     - [F-Lab insight](https://f-lab.kr/insight/java-synchronization-and-cas)
     - [코드와 함께 설명하는 블로그](https://curiousjinan.tistory.com/entry/java-concurrent-hash-map-cas)
   - 한계점:
     - 모든 메서드에 synchronized 가 걸려있거나, 모든 메서드에서 CAS 를 통해 접근하지 않기에 정말 완벽한 동시성을 보장해 주지는 못함.
       - 이는 성능을 위한 트레이드 오프로, get, iterator 등의 경우는 수정의 목적이 아니라 조회의 목적이기에 이렇게 만들었다고 생각됨.
     - compound operations (복합 연산)에 대해서는 적용되지 않음
       - ex: `if (map.containsKey(k)) { map.put(k, v); }`
         - containsKey 이후 put 이전에 다른 스레드에서 개입할 가능성이 있음
       - computeIfAbsent(), putIfAbsent(), compute() 등의 메서드로 해결 가능
        
---
2. Serializable의 구현없이도 request body의 값등 JVM외부의 값을 DTO로 매핑할 수 있었는데 무엇이 이를 가능하게 했으며, 어떻게 동작하는 것일까?
   - Spring 서버에서 일반적으로 진행되는 request body 의 매핑과정은 책에서 설명한 JVM 내부 직렬화와는 괘가 다르다.
     - Serializable 이 필요한 경우는 객체를 메모리 그대로 전달할 수 없어 직렬화가 필요한 경우인데, 리퀘스트 바디의 경우, 문자열을 객체로 변환하는 과정에 더 가깝다고 할 수 있다.
     - 이 변환하는 과정에서 DTO 객체는 새로 생성되는 것이기에 필요없다.
     - [자바 챔피언 Adam Bien 블로그](https://www.adam-bien.com/roller/abien/entry/do_jpa_entities_have_to)
   - Tomcat/Netty 등의 서블릿 컨테이너가 HTTP 요청을 받아 reqeust 가 수신되면, MessageConverter 를 선택한 후 (보통은 MappingJackson2HttpMessageConverter) 리플렉션을 이용해 JSON의 키와 DTO 클래스의 필드 이름을 매칭시켜 값을 매핑하는 방시이다.  

---
3. notify/notifyAll을 호출하면 락은 즉시 해제될까? 아니면 메서드코드가 끝날 때까지 유지되고 풀릴까?
   - 락을 즉시 해제하지 않는다. 즉, notify를 호출한 스레드에서는 해제되지 않고, 대기중(Thread.State.WAITING / TIMED_WAITING) 인 **스래드들을 꺠우는 요청만 발생함**
   - 이후 현재의 쓰레드가 synchronized 블록을 완전히 벗어나게 되면 현재 쓰레드의 모니터가 해제되면서 다른 쓰레드가 해당 객체의 모니터를 획득이 가능해지며 이후에 진행이 됨.
   
    - ex:
    ```
    Time →
    Thread A                         Thread B
    ---------                        ---------
    synchronized(obj) {
        // do something
        obj.notify()  ----------------> awakens B (state: WAITING → RUNNABLE)
        // still holding lock
        // do more work
    }                                   <-- exit synchronized → releases lock

                                       <-- B acquires lock → proceeds
    ```
   - Object 의 notify는 어떻게 구현되어 있기에 레퍼런스되는 스레드 마다 별도로 notify가 필요한 걸까?
     - notiy는 JVM 내의 네이티브 코드로 구현되어 있으며, 이는 모니터 락과 스레드 큐의 관리가 JVM 레벨에서 직접 처리함을 의미함
       - 객체의 모니터는 대기 중인 스레드들을 관리하는 큐(wait set)을 내부적으로 유지
         - notify 는 해당 큐의 첫번째를 꺠우는 방식으로 동작.
   - reference:
       - [oracle doc](https://docs.oracle.com/javase/specs/jls/se8/html/jls-17.html#17.2.2)
       - [Object API](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html#notify--)