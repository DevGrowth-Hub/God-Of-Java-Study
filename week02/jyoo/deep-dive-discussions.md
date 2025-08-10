# Week02 deep-dive-discussions
1. hashCode()가 실제 메모리 주소와 연관이 있는지, 그리고 equals()가 false일 때 서로 다른 hashCode() 값이 해시테이블 성능에 어떻게 기여하는가? 
   - hashCode()의 응답값은 실제 메모리 주소는 아니고, 16진수 정수형 값을 리턴한다. 이 값이 실제 메모리의 주소 해시일 수 없는 이유는 API에 자바 개발자들이 남긴 주석의 hashCode() 메서드의 contract에서 요구하는 내용중, 자바 어플리케이션이 실행중일떄 같은 객체에 대해 hashCode()가 호출된다면, 항상 같은 정수값을 리턴해야한다고 한다. 
     - **그러나 JVM은 GC 사이클의 흐름상 객체들을 재배치하는데 이를 어떻게 살아남아 리턴을 하는가?**
       - 그 전에 알아야 할 것은 JVM 의 GC 객체 이동(컴팩션)은 무엇이며, 이는 왜 객체의 메모리 주소를 이동시키는가?
         - GC 는 heap 메모리에서 더 이상 참조되지 않는 객체를 정리하는 역할을 하는데, 이 프로세스의 마지막에 파편화된 메모리를 정리하고, 살아남은 객체들을 밀어붙여 연속적인 공간을 확보하는 작업이 있고 이게 compaction 이다. **이 과정중 객체의 메모리 주소는 변경된다!**
           - 이 프로세스를 통해, 더 효율적으로 메모리를 관리할 수 있기 때문에 시행한다고한다.
       - 최초의 hashCode() 요청이 있을 때, [get_next_hash](https://hg.openjdk.org/jdk8u/jdk8u/hotspot/file/87ee5ee27509/src/share/vm/runtime/synchronizer.cpp#l555) 라는 네이티브 메서드를 이용해 정수값을 생성하는데, OpenJDK8 기준으로는 해당 시점에 정수값을 생성하는 값은 `Thread state combined with xorshift` (스레드 상태에 따른 정수 값) 라고 한다. (위 코드의 해석이 용이하지 않아 자세한 내용은 아직 파해쳐 보지 못했다.) 이 정수값을 객체의 헤더에 담고, 객체의 메모리 주소와 함께 어딘가에 저장
         - 참조: https://varoa.net/jvm/java/openjdk/biased-locking/2017/01/30/hashCode.html
     - 서로다른 두 객체 a와 b에 대해 `a.equals(b) ==  false` 라면, 해시 테이블에서 키를 관리하는 방법은 hashCode를 호출해 버킷의 위치를 결정하는데 이 버킷에 이미 객체가 존재한다면 equals()를 통해 기존 키와 충돌이 있는지를 검사한다, false 일 경우 충돌이 없다는 의미기에 a 와 b는 다른 버킷 해시에 저장되어 탐색과 삽입시 O(1) 성능을 유지하게 된다.
        - equals 메서드의 재정의가 필요한 케이스는 언제일까?
          - 논리적 동치성 (logical equality)를 확인해야 하는데, 상위 클래스의 equals가 논리적 동치성을 비교하도록 정의되지 않은 때. (Effective java Item10 equals는 일반 규약을 지켜 재정의해라 참조) 주로 값 클래스들이 해당한다고 함. ex: Integer, String 
            - 무슨 의미냐 -> 그 값 자체가 키로서의 역할을 수행해야하는 클래스들을 의미한다.
            - ex: String a = "key" ; String b = "key" 일때, Map<String, Object> testMap; 이 있을 때 testMap 에 이미 a 를 키로 값을 등록해놨다면, b 를 키로 값을 추가하면 a 때 put 했던 값이 사라지는 것과 같은 원리다.
        - 책에서는 DTO를 만들 경우에는 객체 비교를 위해 반드시 필요하다고 명시헀는데 그 이유는 무었일까?
          - DTO는 데이터 트랜스퍼를 위해 존재하고, 이러한 객체는 같은 값을 담고 있다면 논리적으로 같은 객체로 취급하는 것이 더 자연스럽다. 
          - 14 이상부터 DTO 패턴의 대표적인 형태인 Record가 기본적으로 equals/hashCode를 생성하고있음.
2. 모든 enum 이 상속받는 다는 java.lang.Enum 클래스를 보면, 이 클래스는 추상 클래스이고 스태틱 메서드는 하나도 없다. 그러나 우리는 enum 을 스태틱한 열거형의 데이터로 사용하는데 이는 어떻게 가능한 걸까?
   - Enum 클래스를 컴파일 할 시, 선안한 열거 데이터들을 전부 상수 필드로 만들어 선언해 버린다. 컴파일러가 .java 파일들을 빌드할 때 만일 해당 파일이 enum 예약어를 가지고 있다면 상수 필드로 선언해 버리는 것이다. 
   - 이 때 생성자를 private 으로 만들어 외부에서의 인스턴스 생성을 방지한다
   - ex:
     ```java
        public enum Color {
             RED, GREEN, BLUE
        }
     ```
     이러한 enum을 javac Color.java 로 컴파일한 후 javap -c -p Color 로 내부 구조를 확인해 보면 아래와 같은 결과를 리턴한다.
   ```java
        Compiled from "Color.java"
            public final class com.virnect.data.domain.Color extends java.lang.Enum<com.virnect.data.domain.Color> {
            public static final com.virnect.data.domain.Color RED;
            public static final com.virnect.data.domain.Color GREEN;
            public static final com.virnect.data.domain.Color BLUE;
            private static final com.virnect.data.domain.Color[] $VALUES;
            public static com.virnect.data.domain.Color[] values();
            public static com.virnect.data.domain.Color valueOf(java.lang.String);
    ```
3. String이 불변(Immutable)으로 설계된 이유와 + 연산 시 발생하는 메모리·GC 부하를 StringBuilder/StringBuffer가 어떻게 해결하는지? 
   - 불변 (immutable) 객체란 무엇인가? 
     - 한번 생성되면 그 상태 (내부 필드 값)이 절대 바뀌지 않는 객체.
     - String 이 불변해야하는 이유는 뭘까?
       - 문자열이 불변하지 않다면, 선언되어 있는 문자열에 대해, 해당 문자열의 필드 중 value(byte[]) 를 악의적으로 조작하여, 매 선언된 문자열의 값이 달라지는 이슈가 생길 수 있다. 이 또한 별도의 키-값 관리를 통해 적용할 수 있을테지만 이는 불필요한 작업을 추가하는 것 같다. 
       - 불변 객체이기에 hashCode는 한번만 계산하고 캐싱해 둘 수 있게되어, 키값으로 사용되는 문자열의 특성상 빠른 키 비교 및 검색에 도움을 줄 수 있음
       - String Constant Pool 을 사용해 heap 메모리에 저장하여 사용이 가능해진다. 절대 변하지 않는 값이기에, 컴파일 시점에 등록된 스트링 값을 풀에 등록되어 있어 계속 효율좋게 사용이 가능해진다.
   - StringBuilder/StringBuffer는 둘 다 AbstractStringBuilder 를 상속받고, 이 부모가 필드로 문자 배열(byte[])을 가지고 있고, 매 추가 (append) 마다 해당 배열에 값들을 추가하고 배열이 부족하면 최소 2배크기로 확장 시킴.
       - 여기서 새로운 배열의 할당이 훨씬 더 메모리 효율이 좋다. 객체가 새로 생성되고 할당되는 작업이 적고, GC 되는 데이터는 결국 하나이기 때문.
   - JDK 5 이상부턴 String 의 + 연산 실행시 내부적으로 StringBuilder를 이용하도록 변경해 준다고 함.
4. @RestControllerAdvice는 어떻게 예외를 감지하여 처리할 수 있도록 동작하는 것 일까?
   - 아주 Spring 에서 예외의 흐름 처리
     1. DispatcherServlet이 Spring Container 로 들어온 요청에 대한 예외를 감지
     2. DispatcherServlet이 ExceptionHandlerExceptionResolver 를 호출 
     3. ExceptionHandlerExceptionResolver가 전역에 등록된 @RestControllerAdvice 빈을 검색
     4. 발견한 빈 안에서 @ExceptionHandler 에너테이션을 기준으로 터진 예외에 대한 핸들러 메서드를 탐색 후 실행
     5. 반환 객체를 HttpMessageConverter 로 직렬화하여 응답 본문(body)에 작성.