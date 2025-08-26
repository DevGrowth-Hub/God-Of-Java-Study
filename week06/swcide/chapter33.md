# 📘 자바의 신 VOL.2 – 16장: Java 9에서 변경된 것들은?

## Java의 변경된 정책은 꼭 알아야 해요.
- **릴리스 주기**: Java 9부터 **6개월 주기(3·9월)**로 버전 출시.
- **운영 권장**: 장기 지원(LTS) 버전을 **운영 환경에 우선 적용**. 비-LTS는 학습·파일럿에 적합.
- **변경 추적**: 반기마다 변화 요점을 정리해 팀 내 가이드로 누적 관리.

---

## Java 버전별 추가된 사항은 어디서 확인해야 할까?
- **JEP Index**: 각 버전의 개선 제안(번호·상태) 일람.
- **Release Notes / What’s New**: OpenJDK·벤더 배포판의 변경 요약.
- **Javadoc / `--help`**: API·도구 옵션 변화 확인.

---

## Java 9에서 추가되거나 달라진 것들은?

### 추가된 기능
| 항목 | 설명 |
|---|---|
| **모듈화 - 직소프로젝트** | **JPMS** 도입. `module-info.java`로 의존/공개 범위 명시, **강한 캡슐화**와 **경량 런타임(`jlink`)** 구현. |
| **새로운 http 클라이언트** | **`HttpClient` / `HttpRequest` / `HttpResponse`** 중심의 신규 API. **HTTP/2**, 동기/비동기, 빌더 기반 구성 지원. *(JDK 9에선 Incubator)* |
| **publish -subciribe 프레임워크** | **Flow API**(`Publisher/Subscriber/Subscription/Processor`)로 **백프레셔** 포함 표준 펍섭 모델 제공. |
| **immutable 셋 제공** | `Set.of(...)` (및 `List.of(...)`, `Map.of(...)`)로 **불변 컬렉션**을 간결하게 생성. |
| **stack walker api 추가.** | `StackWalker`로 **필터링/지연 접근** 기반의 효율적인 스택 추적. |
| **-Xlog 옵션으로 로그 일원화** | **Unified JVM Logging**: GC·클래스로딩·JIT 등 로그를 **태그/레벨 기반**으로 `-Xlog` 한 가지 옵션으로 제어. |
| **jshell 명령어 추가,.** | 공식 **REPL(JShell)** 제공. 코드 스니펫 실험/학습/프로토타입에 유용. |

### 개선된기능
| 항목 | 설명 |
|---|---|
| **process api** | `ProcessHandle`/`ProcessHandle.Info`로 **PID·부모/자식·시작시각·CPU 시간** 조회, `onExit()`로 종료 이벤트 처리. |
| **try-with resources** | **이미 선언된(효과상 final) 자원 변수**를 `try (varName)`로 재사용해 자동 해제(보일러플레이트 감소). |
| **다이아몬드 연산자 확장** | **익명 클래스**에서도 다이아몬드(`<>`) 사용 허용 → 제네릭 표기 간결화. |
| **interface에 private메소드 추가 기능** | 인터페이스 내부 **공통 로직**을 `private`/`private static` 메서드로 캡슐화하여 `default/static`에서만 재사용. |
| **jcmd에 로딩된 클래스 목록 확인 가능한 옵션 추가** | `jcmd` 진단 기능 보강으로 **로딩된 클래스/클래스로더 통계** 등 확인 용이. |
| **다중 해상도의 이미지를 처리하는 mulitresolutionImage api** | **Multi-Resolution Image API**로 단일 리소스에 다중 해상도 이미지를 캡슐화·선택 표시. |
| **옵셔널에 stream 사용 .** | `Optional<T>.stream()`으로 **0/1개 요소 스트림** 반환 → 스트림 파이프라인과 자연스럽게 결합. |

---

## Java 9부터 변경된 String 클래스
- **Compact Strings**: 내부를 `char[]`에서 **`byte[] + coder`** 구조로 변경.
    - **LATIN1** 가능 문자열은 1바이트/문자로 저장 → 메모리 절감
    - 그 외는 **UTF-16**(2바이트/문자) 사용
- 효과: 문자열 구성에 따라 **메모리 사용 감소** .

---

## Interface의 private 메소드를 추가할 수 있다
- 의도: `default/static` 메서드 간 **공통 로직**을 인터페이스 **내부에서만** 재사용.
- 규칙: `private`/`private static` 허용, **외부 호출 불가**, `abstract` 불가.

---

## 새로운 HTTP 클라이언트를 쓰면 뭐가 좋을까?
- 핵심 타입: **`HttpClient`**, **`HttpRequest`**, **`HttpResponse`**.
- 장점
    - **HTTP/2** 지원
    - **동기/비동기** 처리 및 `CompletableFuture` 기반 비동기 흐름
    - **빌더 패턴**으로 타임아웃·리다이렉트·프록시·인증 등을 선언적으로 구성
    - 표준 API로 **의존성 축소** 및 가독성 향상  
      *(JDK 9에선 Incubator 모듈로 제공, JDK 11에서 표준화)*

---

## Publish-Subscribe 프레임워크
- **풀링(Pulling)** vs **폴링(Polling)**: 풀링은 **필요할 때** 데이터를 요청하는 방식, 폴링은 **주기적으로** 변경 여부를 확인하는 방식이며, **Pub-Sub은 풀링 개념**에 해당.
- **정의**: 발행자(Publisher)와 구독자(Subscriber) 사이의 **직접 의존을 제거**하고 **비동기적으로 메시지를 전달**하는 패턴.
- **구성 요소**
    - **Publisher**: 이벤트를 **발행**하는 주체
    - **Subscriber**: 메시지를 **수신/처리**하는 주체(구독 시 전달되는 **Subscription**을 통해 `onSubscribe()`, `onNext()`, `onError()`, `onComplete()` 사용)
    - **Processor**: Publisher와 Subscriber **사이를 중개**하는 메시지 브로커 역할
- **동작 흐름**
    - (1) **Subscriber**가 중개자에게 **구독 요청**
    - (2) **Publisher**가 중개자에게 **이벤트 발행** 요청
    - (3) **중개자**가 해당 이벤트를 **모든 구독자**에게 **알림 전달**
---

## 자바의 모듈화-직소 프로젝트
- Java 플랫폼 **모듈화**, 애플리케이션을 **모듈 단위**로 개발
- **주요 특징**
    - 관련 **패키지/리소스 그룹화**, **의존성 명시적 선언**
    - `module-info.java`로 모듈 정의
    - **캡슐화 강화**: 내부 API 비공개, 공개할 패키지 선택적 노출
    - **의존성 관리 개선**: 대규모 애플리케이션 구성/배포 용이