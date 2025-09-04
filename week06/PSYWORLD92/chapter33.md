# ch.33 Java 9에서 변경된 것들

Java 9는 **모듈 시스템 도입**과 다양한 성능 및 기능 개선으로 큰 변화를 가져온 버전입니다.

## 1. 추가된 기능

### 1) 모듈화 - 직소 프로젝트 (Project Jigsaw)
- JDK와 애플리케이션을 **모듈 단위**로 관리 가능.
- `module-info.java` 파일을 통해 의존성과 공개 API를 정의.
- 장점:
  - 캡슐화 강화
  - 애플리케이션 규모가 커져도 관리 용이
  - JDK 자체도 모듈화됨 → 불필요한 모듈 제거 가능

### 2) 새로운 HTTP 클라이언트
- 기존 `HttpURLConnection`의 불편함 개선.
- **비동기/동기 요청 지원** (`CompletableFuture` 활용).
- HTTP/2와 WebSocket 지원.

### 3) Publish-Subscribe 프레임워크
- `java.util.concurrent.Flow` API 제공.
- **Reactive Streams** 기반, 비동기 데이터 흐름 처리 지원.
- 주요 인터페이스: `Publisher`, `Subscriber`, `Processor`, `Subscription`

### 4) Immutable Set 제공
- `Set.of()` 메서드로 불변 집합 생성 가능.
- 예시:
  ```java
  Set<String> names = Set.of("Tom", "Jerry", "Kim");
  ```

### 5) Stack-Walker API
- 호출 스택 정보를 효율적으로 탐색할 수 있는 API.
- 성능 개선, 보안 강화.

### 6) `--Xlog` 옵션
- JVM 로그를 **일원화된 방식**으로 관리.
- GC, JIT, 클래스 로딩 등 다양한 로그를 통합 출력.

### 7) JShell 도구 추가
- 대화형 자바 실행 환경 (REPL: Read-Eval-Print Loop).
- 빠른 코드 테스트와 학습에 유용.

---

## 2. 개선된 기능

### 1) Process API 개선
- OS 프로세스 정보에 접근 가능 (`ProcessHandle`).
- PID, 실행 시간, CPU 사용량, 자식 프로세스 등 확인 가능.

### 2) try-with-resources 개선
- 이미 선언된 리소스를 `try` 블록에서 바로 사용할 수 있음.
- 예시 (Java 7 vs 9):
  ```java
  // Java 7
  try (BufferedReader br = new BufferedReader(new FileReader("data.txt"))) { ... }

  // Java 9
  BufferedReader br = new BufferedReader(new FileReader("data.txt"));
  try (br) { ... }
  ```

### 3) 다이아몬드 연산자 확장
- 익명 클래스에서도 다이아몬드(`<>`) 연산자 사용 가능.

### 4) 인터페이스 private 메소드 추가
- 코드 중복을 줄이기 위해 인터페이스 내부에서 `private` 메소드 정의 가능.

### 5) jcmd 개선
- 로딩된 클래스 목록 확인 가능한 옵션 추가.

### 6) MultiResolutionImage API
- 다양한 해상도의 이미지를 관리할 수 있는 API.

### 7) Optional에 stream() 추가
- Optional을 Stream으로 변환 가능.
- 예시:
  ```java
  Optional<String> opt = Optional.of("Hello");
  opt.stream().forEach(System.out::println);
  ```

### 8) String 클래스 변경
- 내부 구현이 **char[]** → **byte[]** 기반으로 변경 (메모리 효율성 향상).
- Compact String 기능으로 메모리 절약.

---

# 요약
- **추가된 기능**: 모듈화(Project Jigsaw), HTTP/2 클라이언트, Publish-Subscribe, Immutable Set, Stack-Walker, JShell, Xlog  
- **개선된 기능**: Process API 확장, try-with-resources 개선, 인터페이스 private 메소드, 다이아몬드 연산자 확장, MultiResolutionImage API, Optional.stream(), String 메모리 구조 개선
