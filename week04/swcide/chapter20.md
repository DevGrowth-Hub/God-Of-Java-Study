# 📘 자바의 신 VOL.2 – 2장: 가장 많이 쓰는 패키지는 자바랭

## java.lang 패키지는 특별하죠
- **import 없이 자동 포함**되는 유일한 패키지.
- 자바의 **기본 기능**을 제공하는 핵심 클래스 포함(`String`, `System`, `Math`, 각종 래퍼 클래스 등).
- 메모리 관련 **에러**: `OutOfMemoryError`, `StackOverflowError`.
- 주요 **애너테이션**: `@Deprecated`, `@Override`, `@SuppressWarnings`.
---
## 숫자를 처리하는 클래스들
- **상속 구조**: 숫자 관련 래퍼 클래스들은 모두 `Number` 추상 클래스를 상속받음
- 래퍼 클래스: `Byte`, `Short`, `Integer`, `Long`, `Float`, `Double`(필요 시 `Character`, `Boolean`).
- 공통 기능: `MIN_VALUE`/`MAX_VALUE`(범위), `parseXxx`·`toString`(문자열↔숫자 변환), `compare`(비교).
- **valueOf 사용 권장**: 박싱 시 `new` 대신 `valueOf(...)`를 사용한다(캐싱 등 내부 이점).

### 왜 숫자를 참조 자료형(래퍼)으로도 만들었나
- **컬렉션/제네릭 호환**: `List`, `Map` 등은 **객체**만 저장 → 기본형을 담기 위해 래퍼 필요
- **값 없음 표현**: `null`로 **부재** 표현 가능(기본형은 불가)
- **유틸리티 제공**: `parse*`, `valueOf`, `compare`, `toString` 등 **변환/비교/포맷** 메서드 제공
- **상수 보유**: `MAX_VALUE`, `MIN_VALUE`, `BYTES`, `SIZE` 등 **형 정보** 노출

---
## 각종 정보를 확인하기 위한 System 클래스
- 시간: `currentTimeMillis`(현재 시각, ms), `nanoTime`(경과 시간 측정).
- 시스템 프로퍼티: `System.getProperties` / `System.getProperty(key)`
- 자주 쓰는 키(본문 표): `os.name`, `os.version`, `os.arch`, `java.version`, `java.vendor`, `java.home`, `user.name`, `user.home`, `user.dir`, `file.separator`, `path.separator`, `line.separator`.
- 환경 변수: `System.getenv()` / `System.getenv(name)`.
- 기타 유틸: `arraycopy(...)`, `gc()`, `exit(int)` 등.

## System.out을 살펴보자
- `System.out`은 표준 출력 스트림(`PrintStream`).
- 메서드: `print`, `println`, `printf`.

### 문자열 + `null` 이슈
- “두 번째 출력문은 `null`과 문자열을 합쳤는데도 예외가 발생하지 않았다.”
- 이유: 컴파일러가 문자열 더하기를 `StringBuilder`로 변환하기 때문.
- 결국 `append(null)`이 호출되어 문자열 `"null"`이 출력된다.

---

## ❓
1. 오토박싱/언박싱은 어떻게 동작하는가?
   2. 오토언박싱이 성능에 미치는 영향은 무엇인가? 
   3. 오토박싱/언박싱을 사용하면 안 되는 상황과 그 이유는 무엇인가?
2. Integer.valueOf(int)는 어떻게 동작하는가? 
   3. 내부 캐시 메커니즘은 어떻게 구현되어 있고 캐시 범위가 코드의 동작과 성능에 어떤 영향을 주는가?