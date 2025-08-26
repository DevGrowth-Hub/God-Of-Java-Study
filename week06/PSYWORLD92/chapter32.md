# ch.32 Java 8에서 변경된 것들

Java 8은 기존 자바 문법과 API를 대폭 확장하여 함수형 프로그래밍을 지원하고, **Stream API**와 **java.util.function** 패키지를 본격적으로 도입했습니다.

---

## 1. 람다 표현식 (Lambda Expression)
- 메소드를 간단히 표현할 수 있는 **익명 함수 문법**.
- 인터페이스를 구현할 때 불필요한 보일러플레이트 코드를 제거.
- 예시:
  ```java
  Runnable r = () -> System.out.println("Hello Lambda!");
  ```

---

## 2. java.util.function 패키지
자주 사용하는 함수형 인터페이스들이 제공됩니다.

- **Predicate<T>**: 입력값을 받아 `true/false` 반환  
  ```java
  Predicate<String> isEmpty = s -> s.isEmpty();
  ```

- **Supplier<T>**: 값을 공급 (입력 없음, 출력만 있음)  
  ```java
  Supplier<Double> random = () -> Math.random();
  ```

- **Consumer<T>**: 값을 받아 소비 (출력 없음)  
  ```java
  Consumer<String> print = s -> System.out.println(s);
  ```

- **Function<T,R>**: 입력을 받아 변환 후 반환  
  ```java
  Function<String, Integer> length = s -> s.length();
  ```

- **UnaryOperator<T>**: 동일 타입 입력 → 동일 타입 출력 (단항 연산자)  
  ```java
  UnaryOperator<Integer> square = x -> x * x;
  ```

- **BinaryOperator<T>**: 동일 타입 두 입력 → 동일 타입 출력 (이항 연산자)  
  ```java
  BinaryOperator<Integer> sum = (a, b) -> a + b;
  ```

---

## 3. Stream API 확장

### 주요 중간 연산 (Intermediate Operations)
- **filter()**: 조건에 맞는 요소만 추출  
- **map()**: 요소 변환  
- **flatMap()**: 다차원 스트림 평탄화  
- **sorted()**: 정렬  
- **distinct()**: 중복 제거  

### 주요 종단 연산 (Terminal Operations)
- **forEach() / forEachOrdered()**: 요소 순회  
- **toArray()**: 배열로 변환  
- **reduce()**: 누적 연산  
  - `reduce(BinaryOperator)`  
  - `reduce(identity, BinaryOperator)`  
- **collect()**: 결과를 컬렉션으로 수집  
- **min() / max() / count()**: 최소, 최대, 개수  
- **anyMatch() / allMatch() / noneMatch()**: 조건 검사  
- **findFirst() / findAny()**: 요소 탐색  

### 예시 코드
```java
List<String> names = Arrays.asList("Tom", "Jerry", "Kim", "Lee");

names.stream()
    .filter(s -> s.length() > 3)
    .map(String::toUpperCase)
    .sorted()
    .forEach(System.out::println);
```

---

## 4. 메소드 참조 (Method References)
람다식을 더 간결하게 표현할 수 있는 문법.

- **특정 객체의 인스턴스 메소드 참조**
  ```java
  list.forEach(System.out::println);
  ```

- **특정 유형의 임의 객체 메소드 참조**
  ```java
  list.stream().map(String::toLowerCase);
  ```

- **생성자 참조**
  ```java
  Supplier<List<String>> supplier = ArrayList::new;
  ```

---

## 5. Stream 정리
스트림은 **데이터 처리 파이프라인**을 구성하여 선언적이고 간결하게 데이터 연산을 수행합니다.

### 스트림 처리 단계
1. **스트림 생성**  
   - `collection.stream()`  
   - `Arrays.stream()`  
   - `Stream.of()`

2. **중간 연산 (Intermediate)**  
   - 데이터 가공, 변환, 필터링 → Stream 반환  
   - 여러 중간 연산을 **체인 형식**으로 연결 가능  
   - 예: `filter()`, `map()`, `flatMap()`, `sorted()`

3. **종단 연산 (Terminal)**  
   - 스트림 연산을 종료하고 결과 반환  
   - 예: `forEach()`, `toArray()`, `collect()`, `reduce()`, `count()`

---

# 요약
- **람다 표현식**: 코드 간결화, 함수형 스타일 지원  
- **java.util.function** 패키지: Predicate, Supplier, Consumer, Function, Operator 계열 제공  
- **Stream API 확장**: filter, map, flatMap, reduce, collect 등 다양한 연산 제공  
- **메소드 참조**: 람다식을 더 간단히 표현  
- **스트림 처리 단계**: 생성 → 중간 연산 → 종단 연산 구조
