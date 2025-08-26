# 📘 자바의 신 VOL.2 – 18장: Java 11은 얼마나 달라졌을까?

## Oracle JDK의 유료화
- Java 11부터 **Oracle JDK 상용 라이선스** 전환(프로덕션 사용 시 유료).
- 대안: **OpenJDK**(동일 소스 기반) 배포판 선택.

---

## Java 11에 추가되거나 개선된 기능들은?

### 추가되거나 개선된 것
| 항목 | 설명 |
|---|---|
| String 클래스에 메소드 추가 | `isBlank()`, `lines()`, `strip()/stripLeading()/stripTrailing()`, `repeat()` 등 **문자열 편의 메소드** 확장. :contentReference[oaicite:1]{index=1} |
| file 클래스에 메소드 추가 | `Files.readString()`, `Files.writeString()`로 **간단한 파일 입출력** 지원. :contentReference[oaicite:2]{index=2} |
| collection 인터페이스에 `toArray()` 메소드 추가 | `Collection.toArray(IntFunction<T[]>)`로 **타입 안전한 배열 변환** 용이. |
| predicate 인터페이스에 `not()` 메소드 추가 | `Predicate.not(...)`로 **가독성 높은 부정 조건** 구성. |
| lambda 사용시 `var` 변수 사용가능 | 람다 매개변수에 `var` 표기 허용(**일관된 표기** 필요, 혼용 불가). :contentReference[oaicite:3]{index=3} |
| HTTP Client 클래스 추가 | `java.net.http.HttpClient` **표준화**(HTTP/2, WebSocket, 동기·비동기). :contentReference[oaicite:4]{index=4} |
| Nestmate 접근제어 | **Nest-Based Access Control**로 중첩 클래스 간 **private 멤버 접근** 단순화. :contentReference[oaicite:5]{index=5} |

### 기능 보완 및 제거
| 항목 | 설명 |
|---|---|
| Dynamic Class-File Constants | `ConstantDynamic` 도입으로 **상수 계산의 유연성** 향상. |
| Improved AArch64 Intrinsics | ARM64 대상 **핫스팟 인트린식 최적화** 보강. |
| A No-Op Garbage Collector | **Epsilon GC** 추가(할당만, 수거 없음) — **성능/부하 테스트** 용도. :contentReference[oaicite:6]{index=6} |
| Java EE / CORBA 제거 | `java.se.ee` 모듈 제거(JAXB, JAX-WS 등 **JDK에서 제외**). :contentReference[oaicite:7]{index=7} |
| Nashorn JS 엔진, Pack200 툴 Deprecate | Nashorn 및 Pack200/`unpack200` **사용 중단(차기 제거 예정)**. |

---

## 컴파일 없이 java 파일의 실행하는 방법은?
- **단일 파일 실행**: 소스 파일을 **컴파일 없이** 바로 실행 — `java <파일명>.java` 형태.
- **스크립트처럼 실행(셔뱅)**: 파일 첫 줄에 `#!/usr/bin/env java` 등 **shebang** 지정 후 실행 권한 부여 → 직접 실행 가능.
- 용도: **학습/실험/유틸 스크립트**에 적합(빠른 피드백, 빌드 단계 생략). :contentReference[oaicite:8]{index=8}

---

## Lambda에서 var 사용해보기
- 람다 매개변수에 **`var` 사용 가능** → 지역 변수와 **같은 표기 규칙** 적용.
- **제약**: 매개변수가 여러 개인 경우 **모두 `var`** 또는 **모두 타입 명시**로 **일관성 유지**(혼용 불가), 괄호 필수.
- 활용 포인트: **어노테이션 부착**이 필요한 람다 파라미터에 `var` 사용 시 **표현 간결화**. :contentReference[oaicite:9]{index=9}
