# Java 7에 추가된 것들에는?

---
### Fork / Join 관련
- Fork => 여러개로 나누는 작업
- Join => 모으는 작업을 의미
- work steal?
  - 작업을 위한 대기열 dequeue 가 있을 경우, 바쁜 dequeue 를 대신해 덜 바쁜 dequeue 에서 작업이 많이 남은 dequeue의 작업을 빼가서 가져가는 행위.
- 관련 클래스:
    - `RecursiveTask<V>`: 결과를 반환하는 작업
    - `RecursiveAction`: 결과가 없는 작업
    - `ForkJoinPool`: 작업 실행을 담당하는 스레드 풀
- 장점: 스레드 생성/관리 코드를 직접 작성할 필요 없이, JVM이 최적의 병렬 실행을 관리.

---
### NIO.2 (New I/O 2)

- 목적: 기존 `java.io.File`의 한계를 보완하고, 더 풍부한 파일 시스템 API 제공.
- 주요 클래스:
    - `Path`: 파일/디렉토리 경로 표현 (`Paths.get()`으로 생성)
    - `Files`: 파일 조작 유틸리티 (복사, 이동, 삭제, 속성 확인 등)
    - `FileSystems`: 파일 시스템 정보 확인
    - `FileStore`: 디바이스/볼륨 관련 정보 제공
- 새로운 기능:
    - 심볼릭 링크, 파일 속성, 권한 처리 가능
    - `WatchService`: 파일 생성/수정/삭제 이벤트 감시 (폴링 없이 이벤트 기반 알림 제공)
    - `AsynchronousFileChannel`: 비동기 파일 읽기/쓰기 지원 (Future, CompletionHandler 활용)
- 장점: 대용량 파일/실시간 파일 변경 감시/심볼릭 링크 지원 등 현대적 파일 처리에 적합.

---

### 새로운 채널 및 네트워크 지원

- `SeekableByteChannel`: 임의 위치에서 파일 읽기/쓰기 (랜덤 액세스 지원)
- `NetworkChannel`: 네트워크 소켓 바인딩, 옵션 설정 가능
- `MulticastChannel`: 멀티캐스트 통신 지원
- `AsynchronousChannelGroup`: 비동기 채널용 스레드 풀 제공
- I/O 작업의 유연성과 비동기 지원 강화 → 고성능 서버 개발에 도움.

---

### JDBC 4.1

- `RowSetFactory`, `RowSetProvider` 추가 → `RowSet` 객체를 더 쉽게 생성 가능.
- `Connection`과 `Statement` 없이도 간단히 SQL 실행 가능.

---

### java.util.concurrent 개선

- TransferQueue 추가: `BlockingQueue` 확장, `SynchronousQueue`의 기능을 일반화.
    - Producer/Consumer 패턴에 유용, 메시지 교환을 더 직관적으로 처리 가능.

---

### java.util.Objects 클래스 추가

- `Objects.equals(a, b)`, `Objects.hash()`, `Objects.toString()` 등 **널 안정성**(Null-safety)을 보장하는 유틸 메서드 제공.
- 장점: `NullPointerException` 방어 코드를 간결하게 작성 가능.