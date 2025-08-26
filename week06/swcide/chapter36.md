# 📘 자바의 신 VOL.2 – 19장: Java 12~17은 얼마나 달라졌을까?

---

| Java | 기능 (Feature) | Status | 비고/핵심 포인트 |
|---|---|---|---|
| **12** | Switch Expressions | **preview** | `switch`를 값 계산하는 **표현식**으로 시범 도입 |
|  | Shenandoah GC | **incubator** | 낮은 STW 지연을 목표로 한 새로운 GC(실험) |
|  | JVM Constants API | **standard** | 바이트코드 상수 핸들링 단순화 |
|  | Default CDS Archives | **standard** | 기본 클래스 데이터 공유 아카이브 |
|  | G1 개선(혼합 수집 중단/빠른 메모리 반환) | **standard** | 지연시간·메모리 회수 개선 |
|  | Microbenchmark Suite (JMH) | **standard** | JDK 소스 빌드에 벤치 스위트 포함 |
| **13** | Text Blocks | **preview** | 여러 줄 문자열(“""" … """”) 1차 미리보기 |
|  | Switch Expressions (2nd) | **preview** | 미리보기 개선판 |
|  | Dynamic CDS Archives | **standard** | 런타임 생성 아카이브 지원 |
|  | ZGC: 미사용 메모리 반환 | **standard** | 장기 실행 시 메모리 회수 개선 |
|  | 레거시 소켓 API 재구현 | **standard** | 유지보수성·성능 개선 |
| **14** | Switch Expressions | **standard** | **정식 채택** (미리보기 종료) |
|  | Records | **preview** | 불변 데이터 캐리어(보일러플레이트 최소화) |
|  | Pattern Matching for `instanceof` | **preview** | 검사+캐스팅 보일러플레이트 제거 |
|  | Helpful NullPointerExceptions | **standard** | NPE 메시지에 원인식별 정보 포함 |
|  | Foreign-Memory Access API | **incubator** | 자바 힙 외 메모리 접근(1차 인큐베이터) |
|  | JFR Event Streaming | **standard** | 런타임 진단 이벤트 스트리밍 |
|  | Packaging Tool (jpackage) | **incubator** | 네이티브 패키징 툴(후에 정식화) |
| **15** | Text Blocks | **standard** | 여러 줄 문자열 **정식** |
|  | Sealed Classes | **preview** | 상속 허용 목록으로 **계층 제한** |
|  | Records (2nd) | **preview** | 레코드 미리보기 2차 |
|  | Pattern Matching for `instanceof` (2nd) | **preview** | 미리보기 2차 |
|  | Hidden Classes | **standard** | 프레임워크용 동적 클래스를 위한 숨김 클래스 |
|  | ZGC / Shenandoah **제품화** | **standard** | 양 GC를 **프로덕션** 등급으로 승격 |
|  | Foreign-Memory Access API (2nd) | **incubator** | 인큐베이터 2차 |
| **16** | Records | **standard** | **정식 채택** |
|  | Pattern Matching for `instanceof` | **standard** | **정식 채택** |
|  | Sealed Classes (2nd) | **preview** | 미리보기 2차 |
|  | Packaging Tool (jpackage) | **standard** | **정식 채택** |
|  | Unix-Domain Socket Channels | **standard** | 로컬 IPC 채널 |
|  | Vector API | **incubator** | SIMD 벡터 연산(1차 인큐베이터) |
|  | Foreign-Memory Access API (3rd) | **incubator** | 인큐베이터 3차 |
|  | Elastic Metaspace | **standard** | 메타스페이스 메모리 사용 효율화 |
| **17 (LTS)** | Sealed Classes | **standard** | **정식 채택** (LTS 포함) |
|  | Pattern Matching for **switch** | **preview** | 패턴 기반 스위치(미리보기) |
|  | Enhanced Pseudo-Random Number Generators | **standard** | 가변/점프 가능 PRNG 패밀리 |
|  | Vector API (2nd) | **incubator** | 인큐베이터 2차 |
|  | macOS AArch64 포트 / Metal 렌더링 파이프라인 | **standard** | 신규 포트 및 렌더링 백엔드 |
|  | JDK 내부 강한 캡슐화 기본값 | **standard** | 내부 API 은닉 강화를 기본값으로 |

---

## switch-case 문이 좀 달라졌어요.
- `switch`가 **문(statement)**뿐 아니라 **표현식(expression)**이 됨 → 값을 **반환**할 수 있음.
- **화살표(→) 문법** 도입: `case 1, 3, 5 -> ...`
    - 간결하고 **암묵적 break**로 **fall-through 방지**.
    - 여러 라벨을 **쉼표로 묶어** 하나의 분기에서 처리 가능.
- **표현식에서 블록 사용 시 `yield`로 값 반환**.
- 표현식인 경우 **모든 경로가 값을 산출**해야 하므로 `default` 또는 전체 열거 필요.

---

## 따옴표 세개를 연달아 쓰면 text 블록을 사용할 수 있어요.
- **여러 줄 문자열**을 자연스럽게 작성. JDK 13(미리보기) → JDK 15(정식).
- **줄바꿈/공백 보존**, **공통 들여쓰기 자동 제거**(닫는 구분자 위치 기준).
- 기존 이스케이프(`\n`, `\t` 등)와 혼용 가능.
- JSON/SQL/HTML 등 **문자열 템플릿**을 가독성 좋게 표현.

---

## record 클래스가 뭔가요?
- **불변 데이터 캐리어**를 간결하게 정의하는 클래스 형태. JDK 14(미리보기) → 16(정식).
- 컴포넌트 기반으로 **필드/접근자/생성자/`equals`/`hashCode`/`toString` 자동 생성**.
- 클래스는 암묵적으로 `final`, **상속 불가**(인터페이스 구현은 가능).
- **인스턴스 필드 추가 불가**(컴포넌트 외 필드 X), 정적 멤버는 가능.
- **검증은(불변식 체크)** 정규/컴팩트 생성자에서 수행.
- **얕은 불변**이므로 참조 컴포넌트가 가변이면 내부 변경 가능.
- DTO/값 객체, 설정 스냅샷 등 **값 중심 모델**에 적합(JPA 엔티티에는 부적합).

---

## sealed 클래스는 어디에 사용하는 것인가요?
- 상속 계층을 **명시적으로 제한(permits)**하여 **폐쇄형 계층**을 모델링.  
  JDK 15(미리보기) → 17(정식, LTS).
- 허용된 하위 타입은 **`final` / `sealed` / `non-sealed`** 중 하나로 선언.
- **클래스·인터페이스 모두 적용 가능**, 동일 모듈(또는 패키지) 배치 규칙 준수.
- 장점:
    - API 확장 지점 통제로 **캡슐화·보안·유지보수성** 향상.
    - **패턴 매칭 switch**와 결합 시 **분기 완전성 검사**에 유리(모든 경우 처리 확인).

---
