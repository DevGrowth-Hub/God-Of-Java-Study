# 📘 자바의 신 VOL.2 – 14장: Java 8에 추가된 것들은?

## Optional
- **개요**: `java.util.Optional`(final) — `null` 안전 처리를 위한 컨테이너.
- **생성**
    - `Optional.empty()` : 비어 있는 Optional
    - `Optional.ofNullable(x)` : `x`가 `null`일 수도 있을 때
    - `Optional.of(x)` : `x`는 반드시 `null` 아님
- **존재 확인**: (Java 8 기준) `isPresent()` 사용
- **값 꺼내기**
    - `get()` : 값이 없으면 **`NoSuchElementException`**
    - `orElse(default)` : 없으면 기본값
    - `orElseGet(supplier)` : 없으면 지연 생성
    - `orElseThrow(supplier)` : 없으면 예외 던짐
- **주의**: `Optional`은 NPE 회피에 도움을 주지만, **비어 있는데 `get()` 호출** 같은 오사용 시 예외 발생.

## Default method
- **의미**: 인터페이스에 **기본 구현 메서드**를 정의(키워드 `default`) → **하위 호환성** 확보.
- **용도**: 기존 인터페이스에 기능을 추가하면서, 구현체 수정 없이 동작하도록 함.

## 날짜 관련 클래스들 (`java.time`)
- **배경**: `Date`/`SimpleDateFormat`은 불변 아님, 스레드 안전하지 않음, 사용성·표현력 한계.
- **계층**
    - **Local**: 시간대 없는 날짜/시간(“어느 지역인지” 정보 없음)
    - **Offset**: UTC와의 **오프셋** 포함(예: `+09:00`)
    - **Zoned**: **시간대(예: `Asia/Seoul`)** 포함
- **대표 타입**: `LocalDate`, `LocalTime`, `LocalDateTime`, `ZonedDateTime` 등
- **요일 상수**: `DayOfWeek`(enum) + `getDisplayName(Locale, TextStyle)`로 지역화된 요일 표시
- **메모**: **Local(시간/날짜)** vs **Locale(지역/언어)** 혼동 주의

## 병렬 배열 정렬 (Parallel array sorting)
- **`Arrays.sort(...)`**: 단일 스레드 정렬
- **`Arrays.parallelSort(...)`**: 필요 시 여러 스레드로 분할 정렬(내부적으로 Fork/Join 활용)

## StringJoiner
- **개요**: 순차적으로 문자열을 더할 때 **구분자/접두사/접미사**를 손쉽게 지정.
- **사용**
    - 구분자만: `new StringJoiner(",")` → `add("A")`, `add("B")`, `add("C")` → `"A,B,C"`
    - 3매개변수 생성자: `(delimiter, prefix, suffix)` 지정 가능
