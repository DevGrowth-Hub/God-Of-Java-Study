# Ch.32 Java 8에 추가된 것들은?

---
### Optional (null 안전 컨테이너)
- Optional.empty() : 비어 있는 객체
- Optional.of(x) : null 불가
- Optional.ofNullable(x) : null 허용
- 값 확인 → isPresent()
- 값 꺼내기 → get(), orElse(), orElseGet(), orElseThrow()

---
### Default Method (인터페이스 기본 메서드)
- default 키워드 사용 → 인터페이스에 구현된 메서드 추가 가능
- 기존 구현체 수정 없이 새로운 기능 추가 가능 → 하위 호환성 보장

---
### 날짜/시간 API (java.time)
- 기존 Date/SimpleDateFormat 단점 보완 → 불변 + 스레드 안전
- 주요 클래스 
  - LocalDate, LocalTime, LocalDateTime (시간대 없음)
  - OffsetDateTime (UTC 오프셋 포함)
  - ZonedDateTime (시간대 포함)
  - DayOfWeek enum + getDisplayName(Locale, TextStyle) → 지역화된 요일 표시

---
### 병렬 배열 정렬
- Arrays.sort() → 단일 스레드 
- Arrays.parallelSort() → 내부적으로 Fork/Join 활용, 병렬 정렬

---
### StringJoiner
- 문자열 연결 시 구분자/접두사/접미사 지정 가능
   예:
   new StringJoiner(",", "[", "]")
   .add("A").add("B").add("C"); // [A,B,C]