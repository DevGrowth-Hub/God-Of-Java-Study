# ch.25 쓰레드는 개발자라면 알아두는 것이 좋아요

---

## 1. 스레드 개념
- **프로세스**: 실행 중인 프로그램(메모리 독립).
- **스레드**: 프로세스 내의 실행 단위, 여러 스레드가 하나의 프로세스 자원을 공유.

---

## 2. Thread 클래스와 Runnable 인터페이스
- **상속 방법**: `class MyThread extends Thread { public void run(){...} }`
- **인터페이스 구현**: `class MyRunnable implements Runnable { public void run(){...} }`
- 스레드 실행: `new Thread(runnable).start()`
- 차이점: `Runnable`은 다중 상속 불가 문제 해결에 유리.

---

## 3. 스레드 실행 메서드
- `run()`: 스레드 실행 코드 정의 (직접 호출 시 단순 메서드 실행).
- `start()`: 새로운 실행 흐름 생성 후 `run()` 자동 호출.

---

## 4. 스레드 상태와 전환
- 상태: NEW → RUNNABLE → RUNNING → WAITING/TIMED_WAITING → TERMINATED
- 주요 메서드
  - `sleep(ms)`: 일정 시간 대기 (InterruptedException 발생 가능)
  - `join()`: 다른 스레드 종료까지 대기
  - `yield()`: 실행 양보
  - `interrupt()`: 대기/슬립 상태의 스레드에 예외 발생시켜 깨움

---

## 5. Thread 클래스 주요 메서드
- `getName()`, `setName()`, `getPriority()`, `setPriority()`
- `isAlive()`: 스레드 실행 여부
- `currentThread()`: 현재 실행 중인 스레드 참조
- `setDaemon(true)`: 데몬 스레드 설정

---

## 6. 스레드 동기화 (synchronized)
- 여러 스레드가 동시에 공유 자원 접근 시 데이터 불일치 발생 가능 → **동기화 필요**
- 방법
  - **메서드 전체 동기화**: `public synchronized void method(){}`
  - **블록 동기화**: `synchronized(this){ ... }`
- `synchronized` 블록은 모니터 락을 사용하여 **한 번에 하나의 스레드만 접근** 가능.

---

## 7. 스레드 상태 제어 메서드
- `wait()`: 다른 스레드가 `notify`/`notifyAll` 호출할 때까지 대기
- `notify()`: 대기 중인 스레드 하나 깨움
- `notifyAll()`: 대기 중인 모든 스레드 깨움
- → Object 클래스에 정의된 메서드이며 `synchronized` 블록 내에서만 호출 가능.

---

## 8. 스레드 우선순위
- 기본값: `5` (범위 1~10)
- 높은 우선순위 스레드가 CPU 점유 가능성 ↑ (하지만 OS 스케줄러에 따라 달라질 수 있음).

---

## 9. 데몬 스레드
- 일반 스레드가 모두 종료되면 데몬 스레드도 자동 종료.
- 예: `GC(가비지 컬렉터)` 스레드.

---

## 10. ThreadGroup 클래스
- 스레드를 그룹 단위로 묶어 관리 가능.
- 생성: `ThreadGroup group = new ThreadGroup("groupName");`
- 주요 메서드: `activeCount()`, `list()`, `interrupt()`

---

## 11. Thread 상태 다이어그램 요약
- **NEW**: 객체 생성 직후 (start() 호출 전)
- **RUNNABLE**: 실행 준비 상태 (스케줄러 대기)
- **RUNNING**: CPU 점유 중
- **WAITING / TIMED_WAITING**: 대기 상태
- **TERMINATED**: 실행 종료

---

# 최종 정리
- 스레드 생성 → `Thread 상속` 또는 `Runnable 구현`
- 실행은 반드시 `start()` 메서드 사용
- 동기화 필요 시 `synchronized` 키워드와 `wait/notify` 메커니즘 활용
- 상태 전환과 제어 메서드(`sleep`, `join`, `yield`, `interrupt`) 숙지
- 관리 필요 시 `ThreadGroup`, 백그라운드 작업은 **데몬 스레드**로 설정
