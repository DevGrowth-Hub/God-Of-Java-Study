# ch.31 Java 8에 추가된 것들

Java 8은 자바 언어의 큰 변화를 가져온 버전으로, 함수형 프로그래밍 스타일을 지원하고 다양한 API를 강화했습니다.

---

## 1. 람다 표현식 (Lambda Expressions)
- 함수형 프로그래밍을 지원하는 핵심 기능.
- 익명 함수를 간단하게 작성할 수 있음.
- 예시:  
  ```java
  List<String> list = Arrays.asList("a", "b", "c");
  list.forEach(s -> System.out.println(s));
  ```

---

## 2. 함수형 인터페이스 (Functional Interface)
- 메소드가 하나만 선언된 인터페이스.
- `@FunctionalInterface` 어노테이션을 통해 명시 가능.
- 예시:
  ```java
  @FunctionalInterface
  interface MyFunc {
      void run();
  }
  ```
- 대표적인 함수형 인터페이스: `Runnable`, `Callable`, `Comparator`, `Function`, `Consumer`, `Supplier`

---

## 3. Stream API
- 데이터 컬렉션을 **선언적이고 간결하게 처리**할 수 있는 API.
- `map`, `filter`, `sorted`, `forEach`, `collect` 등을 제공.
- 내부 반복(Internal Iteration)으로 병렬 처리(parallel stream) 지원.
- 예시:
  ```java
  List<Integer> list = Arrays.asList(1, 2, 3, 4, 5);
  list.stream()
      .filter(n -> n % 2 == 0)
      .map(n -> n * n)
      .forEach(System.out::println);
  ```

---

## 4. Optional 클래스
- `null` 처리의 불편함을 개선하기 위해 추가.
- 값이 존재할 수도, 없을 수도 있는 컨테이너.
- 예시:
  ```java
  Optional<String> opt = Optional.ofNullable(null);
  System.out.println(opt.orElse("기본값"));
  ```

---

## 5. 인터페이스의 기본 메소드 (Default Method)
- 인터페이스에 메소드 구현을 직접 포함할 수 있음.
- 기존 코드와의 하위 호환성을 유지하면서 기능 확장 가능.
- 예시:
  ```java
  interface MyInterface {
      default void print() {
          System.out.println("기본 메소드");
      }
  }
  ```

---

## 6. 새로운 날짜/시간 API (java.time)
- 기존 `Date`와 `Calendar`의 단점을 개선한 새로운 API.
- 주요 클래스:
  - `LocalDate`, `LocalTime`, `LocalDateTime`
  - `ZonedDateTime`
  - `Period`, `Duration`
- 예시:
  ```java
  LocalDate today = LocalDate.now();
  LocalDate tomorrow = today.plusDays(1);
  ```

---

## 7. 병렬 배열 정렬
- `Arrays.parallelSort()` 메소드 추가.
- 내부적으로 Fork/Join 프레임워크를 활용하여 대규모 배열 정렬 속도 향상.

---

## 8. StringJoiner 클래스
- 문자열 결합을 쉽게 처리할 수 있는 클래스.
- 구분자(Delimiter), 접두사(Prefix), 접미사(Suffix) 지정 가능.
- 예시:
  ```java
  StringJoiner joiner = new StringJoiner(", ", "[", "]");
  joiner.add("a").add("b").add("c");
  System.out.println(joiner); // [a, b, c]
  ```

---

# 요약
- **람다 표현식**: 간결한 함수형 코드 작성
- **함수형 인터페이스**: 람다와 함께 사용
- **Stream API**: 선언적 데이터 처리, 병렬 처리 지원
- **Optional**: null-safe 프로그래밍 지원
- **인터페이스 default 메소드**: 하위 호환성 유지
- **새로운 날짜/시간 API**: `java.time` 패키지 도입
- **병렬 배열 정렬**: Fork/Join 활용
- **StringJoiner**: 문자열 연결 편의성 제공
