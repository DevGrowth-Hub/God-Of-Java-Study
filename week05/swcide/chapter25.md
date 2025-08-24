# 📘 자바의 신 VOL.2 – 7장: 쓰레드는 개발자라면 알아두는 것이 좋아요

## 쓰레드가 도대체 뭘까?
- JVM이 시작되면 **자바 프로세스**가 시작되고, 그 안에서 여러 **쓰레드**가 수행됨.
- `java`로 클래스를 실행하면 `main()`이 수행되며 **메인 쓰레드**가 시작됨.
- 더 많은 쓰레드가 필요하면 `main()`에서 직접 생성.
- WAS도 결국 `main()`이 만든 쓰레드들이 돌아가는 형태.
- **프로세스**는 자원 소모가 크고, **쓰레드**는 적은 메모리를 점유(“경량 프로세스”). 여러 쓰레드를 쓰면 공간·처리 효율이 좋아짐.

---

## Runnable 인터페이스와 Thread 클래스
- **Runnable**
    - `run()` 하나만 선언된 인터페이스.
    - 실행: `new Thread(new RunnableImpl()).start()`
- **Thread**
    - Runnable을 구현한 클래스이며 더 많은 기능 제공.
    - 실행: `new MyThread().start()`
- **공통**
    - 시작은 **`start()`** (내부적으로 **`run()`** 호출), 실제 작업은 **`run()`** 안에서 수행.
- **상속 제약 회피**
    - 자바는 단일 상속만 가능 → 다른 클래스를 상속해야 한다면 **Runnable 구현** 방식 사용.
- **스케줄링**
    - `start()` 호출 후 메인 쓰레드는 **대기하지 않고** 다음 줄 실행.
    - 어떤 코드가 먼저 실행될지는 **OS 스케줄러**가 결정(예측 불가).
    - 새 쓰레드는 `run()` 종료 시 끝남.

---

## Thread 클래스의 생성자를 살펴보자
- 기본 이름은 `Thread-n`(n은 0부터). 이름 지정 가능.
- 여러 쓰레드를 **ThreadGroup**으로 묶어 관리할 수 있음.
- 각 쓰레드는 **별도의 스택(Stack)** 을 가짐. `stackSize` 지정 가능(상황에 따라 무시될 수 있음).
- 주요 생성자
    - `Thread()`
    - `Thread(Runnable target)`
    - `Thread(Runnable target, String name)`
    - `Thread(String name)`
    - `Thread(ThreadGroup group, Runnable target)`
    - `Thread(ThreadGroup group, Runnable target, String name)`
    - `Thread(ThreadGroup group, Runnable target, String name, long stackSize)`
    - `Thread(ThreadGroup group, String name)`

---

## 많이 사용되는 sleep( ) 메소드에 대해서 살펴보자
- **static** 메소드로 현재 쓰레드를 **일시 정지**.
- 인자:
    - 1번째: **밀리초(ms)**.
    - 2번째: **나노초(ns)** — 1ms 미만이어야 하며, 아니면 `IllegalArgumentException`.
- 대기 중 **인터럽트**가 오면 `InterruptedException` 발생 → 항상 `try-catch`로 처리.

---

## Thread 클래스의 주요 메소드를 살펴보자
- **프로세스 종료 조건**: 다른 쓰레드가 남아 있으면 자바 프로세스는 종료되지 않음.
- 속성/조회/설정
    - `run()` : 스레드 시작 시 수행.
    - `getId()` : JVM이 부여한 고유 ID.
    - `getName()`, `setName(String)`
    - `getPriority()`, `setPriority(int)` – 기본값 사용 권장. 필요 시 `MAX_PRIORITY(10)`, `NORM_PRIORITY(5)`, `MIN_PRIORITY(1)`.
    - `isDaemon()`, `setDaemon(boolean)` – **`start()` 전에만 데몬 지정 가능**. 일반 쓰레드가 모두 끝나면 데몬은 실행 중이어도 JVM 종료 가능.
    - `getStackTrace()`, `getState()`, `getThreadGroup()`

---

## 쓰레드와 관련이 많은 synchronized
- 여러 쓰레드가 같은 객체의 상태를 동시에 변경하면 **데이터 경합**으로 값이 꼬일 수 있음(비 Thread-safe).
- 메소드에 `synchronized`를 붙이면 **한 번에 한 쓰레드만** 진입하여 안전성 확보.
- 단, 메소드 전체 잠금은 **불필요한 대기**를 유발할 수 있음.

---

## synchronized 블록은 이렇게 사용한다
- 공유 데이터 처리 **핵심 부분만** 블록으로 잠금.
- 보통 별도의 락 객체 사용: `private final Object lock = new Object();` 후 `synchronized(lock) { ... }`.
- 변수마다 독립 잠금이 필요하면 **변수별로 다른 락** 사용.
- **서로 다른 인스턴스**를 참조하면 `synchronized` 메소드의 상호 배타는 **적용되지 않음**.
- 예: `StringBuffer`(thread-safe) vs `StringBuilder`(thread-unsafe) – 전자는 내부에 동기화로 안전성을 확보.

---

## 쓰레드를 통제하는 메소드들
- **상태 확인**: `getState()` → `Thread.State`
    - `NEW` / `RUNNABLE` / `BLOCKED` / `WAITING` / `TIMED_WAITING` / `TERMINATED`
- **합류 대기**: `join()`
    - 인자 없음: 종료까지 무기한 대기.
    - 인자(밀리/나노초): 해당 시간만 대기(나노초는 1ms 미만, 위반 시 `IllegalArgumentException`). `0`은 무기한과 동일.
- **중지 요청**: `interrupt()`
    - `sleep()`, `join()`, `Object.wait()` 등 **대기 상태**에서 호출 시 `InterruptedException` 발생.
    - 시작 전/종료 후 호출 시 아무 영향 없이 다음 문장으로 진행.
- **기타 상태 메소드**
    - `checkAccess()` : 수정 권한 확인(권한 없으면 `SecurityException`).
    - `isAlive()` : `run()` 종료 여부.
    - `isInterrupted()` : 해당 스레드의 인터럽트 플래그 확인.
    - `interrupted()` : **static**, 현재 스레드의 인터럽트 여부.
- **주요 static 메소드**
    - `activeCount()` : 현재 그룹에서 살아있는 스레드 수.
    - `currentThread()` : 현재 수행 중인 스레드 객체.
    - `dumpStack()` : 현재 스택을 콘솔에 출력.

---

## Object 클래스에 선언된 쓰레드와 관련있는 메소드들
- `wait()` : 쓰레드를 **WAITING**(또는 시간 지정 시 **TIMED_WAITING**) 상태로 전환.
- `notify()` : 해당 객체에서 `wait()` 중인 스레드 **하나**를 깨움(먼저 대기한 스레드부터).
- `notifyAll()` : `wait()` 중인 **모든** 스레드를 깨움.

---

## ThreadGroup에서 제공하는 메소드들
- 하나의 애플리케이션에서 다양한 쓰레드를 **ThreadGroup**으로 묶어 트리 구조로 관리.
- 주요 메소드
    - `activeCount()` : 실행 중인 쓰레드 개수.
    - `activeGroupCount()` : 실행 중인 쓰레드 그룹 개수.
    - `enumerate(Thread[] list)` : 현재 그룹의 스레드를 배열에 담음.
    - `enumerate(Thread[] list, boolean recurse)` : `true`면 하위 그룹까지 포함.
    - `enumerate(ThreadGroup[] list)` / `enumerate(ThreadGroup[] list, boolean recurse)` : 스레드 **그룹**을 나열.
    - `getName()` : 그룹 이름.
    - `getParent()` : 부모 그룹.
    - `list()` : 상세 정보 출력.
    - `setDaemon(boolean daemon)` : `true`면 현재 그룹의 스레드들을 데몬으로 지정.
- `enumerate()`는 **배열에 채워진 개수**를 반환. 모든 스레드를 담으려면 `activeCount()`로 크기를 산정해 배열을 준비.


