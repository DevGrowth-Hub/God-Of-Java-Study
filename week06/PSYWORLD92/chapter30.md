# ch.30 Java 7에 추가된 것들

Java 7에서는 병렬 처리, 파일 I/O, 네트워크 채널 등 다양한 기능이 강화되었습니다. 주요 기능은 다음과 같습니다.

---

## 1. Fork/Join Framework
- CPU를 더 효율적으로 활용하기 위해 **Fork/Join 프레임워크**가 도입되었습니다.
- **Work Stealing 알고리즘**을 기반으로 하며, 유휴 상태의 스레드가 다른 작업을 훔쳐와 수행할 수 있습니다.
- 주로 **재귀적(recursive) 연산**에 적합합니다.
- `java.util.concurrent` 패키지에 포함된 두 가지 추상 클래스 사용:
  - `RecursiveAction`: 반환값이 없는 작업
  - `RecursiveTask<V>`: 결과를 반환하는 작업
- 예시: 배열의 합계 계산, 병렬 정렬, 대규모 데이터 처리

---

## 2. NIO.2 (New I/O)
Java 7에서는 기존 NIO의 한계를 보완한 **NIO.2**가 도입되었습니다.

### 기존 NIO의 한계
- 심볼릭 링크, 파일 권한, 속성 제어 기능 부족
- `delete()` 메소드가 실패 시 예외를 던지지 않고 `boolean`만 반환
- 파일 변경 확인 시 `lastModified()`를 이용한 단순 비교 방식 → 성능 문제 존재

### 주요 개선 사항
- **Files 클래스**  
  파일 작업을 쉽게 처리할 수 있도록 제공됨.
  - 파일 쓰기/읽기
  - 파일 복사, 이동, 삭제
  - 파일 속성 읽기/설정
- **WatchService 클래스**  
  파일 또는 디렉토리 변경 사항을 감지할 수 있음 (폴더 모니터링).
- **추가 채널 API**
  - `SeekableByteChannel`: 랜덤 액세스 지원
  - `NetworkChannel`, `MulticastChannel`: 네트워크 I/O 강화
  - `AsynchronousFileChannel`: 비동기 파일 채널

---

## 3. 기타 추가 및 변경 사항

### JDBC 4.1
- `try-with-resources`와 통합되어 자원 해제가 자동화됨.
- `Connection`, `Statement`, `ResultSet` 등이 `AutoCloseable`을 구현.

### TransferQueue
- `BlockingQueue`의 확장 버전.
- **생산자-소비자 패턴**에서 소비자가 준비되지 않은 경우, 데이터를 바로 넘기지 않고 대기 가능.

### Objects 클래스
- `java.util.Objects` 클래스 추가.
- `null` 체크와 관련된 유틸리티 메소드 제공.
  - `Objects.requireNonNull(obj)`
  - `Objects.equals(a, b)`
  - `Objects.hash(values...)`

---

# 요약
- **Fork/Join 프레임워크**: 병렬 처리 효율화, 재귀 연산에 적합
- **NIO.2**: 파일 및 네트워크 I/O 개선, WatchService 도입, 다양한 채널 추가
- **Files 클래스**: 읽기, 쓰기, 복사, 이동, 삭제 등 편리한 파일 작업 지원
- **JDBC 4.1**: try-with-resources 통합
- **TransferQueue**: 확장된 큐 기능 제공
- **Objects 클래스**: null 안전성과 유틸리티 기능 제공
