# Ch.33 Java 8에서 변경된 것들은?

---
### 람다 표현식 (Lambda Expression)
- 익명 클래스를 사용하면 가독성도 떨어지고 불편하기에 이를 보안하기 위해 만듦.
- 인터페이스의 경우 메서드가 하나인 것들만 적용가능
  - @FunctionalInterface: 메서드 1개만 유지하도록 컴파일러 보장.
- 형태: (매개변수) -> {처리식} => ex: `(a, b) -> a + b;` || `(String s) -> {System.out.print("s는 무엇인가? " + s)};`
- 대표 인터페이스: Runnable, Comparator, FileFilter, 사용자 정의 인터페이스 등.

---
### java.util.function 주요 인터페이스
- Predicate<T> → boolean test(T) : 조건 검사 (and/or/negate 조합 가능).
- Supplier<T> → T get() : T 를 리턴함
- Consumer<T> → void accept(T) : 출력을 할 때 처럼 수행후 결과를 받을 일이 없을 때 사용.
- Function<T,R> → R apply(T) : T를 R로 변환할 때 사용.
- UnaryOperator<T> → T apply(T) : 같은 타입으로 변경할 때 사용.
  - List.replaceAll 등에서 활용됨.
- BinaryOperator<T> → T apply(T,T) : 동일 타입 2항 연산.

---
## 스트림 (Stream)
- 뭔가 연속된 정보를 처리하는데 사용됨:
  - 스트림의 생성: list.stream()
  - 스트림의 중개 연산: filter(), map(), distinct(), sorted(), limit() … (지연 처리, 여러 개 연결 가능)
  - 스트림의 종단 연산: forEach(), collect(), reduce(), count(), findFirst() … (결과 반환)
  - 병렬 처리: parallelStream() 지원 (속도↑ but CPU 부담/순서 보장 X → 보통 stream() 권장).

---
### 메소드 참조 (::)
- 정적 메서드: Integer::parseInt
- 특정 객체 메서드: System.out::println
- 임의 객체 메서드: String::toLowerCase
- 생성자: MyClass::new, int[]::new