# 📘 자바의 신 VOL.2 – 13장: Java 7에 추가된 것들에는?

## Fork/Join은 CPU를 더 쉽게, 효율적으로 사용하기 위해서 만들어진 거에요
- 계산 작업을 여러 조각으로 **나눠(Fork)** 병렬로 처리하고, 끝나면 결과를 **모은다(Join)** 는 방식
- **Work Stealing**: 여러 **덱(Deque)** 을 가진 작업자 중, **한가한 작업자**가 **바쁜 작업자**의 일을 가져와 처리 → 멀티코어에서 계산 위주의 작업 성능 향상
- **관련 클래스**
    - `RecursiveTask<V>`: **결과를 반환**하는 분할 작업
    - `RecursiveAction`: **반환값이 없는** 분할 작업
    - 두 클래스 모두 `ForkJoinTask<V>`를 확장(비동기 제어·결과 대기 등 제공)
    - 실행은 `ForkJoinPool`에서: `execute()`, `invoke()`, `submit()` / 분할·결합: `fork()`, `join()`
- **사용 패턴**: `compute()`를 오버라이드해서 **작으면 직접 처리**, **크면 둘로 쪼개 재귀 호출**로 진행

---

## NIO도 잘 모르는데 NIO 2가 나왔다
- **목적**: `java.io.File`의 한계(심볼릭 링크, 권한/속성, 실패 시 예외, 변경 감시 등) 보완
- **핵심 패키지**: `java.nio.file` 전반(경로/파일/파일시스템 추상화)

---

## Files 클래스는 파일을 다루기 위한 클래스랍니다
- **경로 기반**: `Path`와 함께 사용
- **생성/이동/삭제**: `createFile()`, `createDirectories()`, `copy()`, `move()`, `delete()`, `deleteIfExists()`
- **읽기/쓰기**: `newBufferedReader()`, `newInputStream()`, `write()`(인코딩·`StandardOpenOption` 지정), `readAllBytes()`, `readAllLines()`
  > 대용량에서는 메모리 사용량 주의
- **복사 옵션**: `StandardCopyOption`(`REPLACE_EXISTING`, `ATOMIC_MOVE` 등)
- **임시 리소스**: `createTempDirectory()`, `createTempFile()`
- **Path 얻기/다루기**
    - 얻기: `Paths.get(...)`, `File.toPath()`, `Paths.get(URI)`
    - 유틸: `relativize()`, `toAbsolutePath()`, `normalize()`, `resolve()`

---

## 파일이 변경되었는지 확인하는 WatchService 클래스도 추가되었어요
- **디렉터리 변경 감시**: 생성/수정/삭제 이벤트 수신
- **이벤트 상수**: `ENTRY_CREATE`, `ENTRY_DELETE`, `ENTRY_MODIFY` (in `StandardWatchEventKinds`)
- **흐름**: `Path.register(watchService, …)` → `take()/poll()`로 키 획득 → 이벤트 처리 → 키 `reset()`

---

## 파일과 관련된 다른 새로운 API에는 어떤 것들이 있을까?
- `SeekableByteChannel`: **임의 위치 접근** 가능한 바이트 채널
- `NetworkChannel`: 바인딩, 소켓 옵션, 로컬 주소 조회 등 **네트워크 채널** 공통 기능
- `MulticastChannel`: **IP 멀티캐스트** 지원
- `AsynchronousFileChannel`: 파일 **비동기 I/O**(결과 수신: `Future` 또는 `CompletionHandler`)
- `AsynchronousChannelGroup`: 비동기 채널용 **스레드 풀 그룹**
- **Channel**: 디바이스/파일/네트워크와의 **데이터 통로** 추상화

---

## Fork/Join과 NIO2 외에 추가 및 변경된 것들을 간단히 살펴보죠
- **JDBC 4.1**: `RowSetFactory`, `RowSetProvider`로 `RowSet` **간편 생성**
- **TransferQueue** (`java.util.concurrent`): Producer/Consumer 패턴에 유용한 **전송 중심 큐**(인터페이스), `BlockingQueue` 확장
- **Objects 유틸리티** (`java.util.Objects`): `compare()`, `equals()`, `hash()`, `hashCode()`, `toString()` 등 **널 안전** 정적 메서드 제공
