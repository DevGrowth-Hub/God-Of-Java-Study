# 📘 자바의 신 VOL.2 – 15장: Java 8에서 변경된 것들은?

## Lambda 표현식(expression)
- **도입 배경**: 익명 클래스의 장황함을 줄이고 가독성을 높이기 위해 추가.
- **적용 대상**: **추상 메서드가 1개인 인터페이스(SAM, Functional 인터페이스)**.
- **상호 전환**: 익명 클래스 ⇄ 람다 표현식 전환 가능(Runnable, Comparator, FileFilter 등).
- **형태**: `(매개변수) -> 처리식/블록` 예) `(int x, int y) -> x + y`
- **@FunctionalInterface**: 해당 인터페이스가 **추상 메서드 1개**만 갖도록 **컴파일 타임 보장**.

## java.util.function 패키지
- **Predicate<T>**: `boolean test(T t)` — 조건 판별. `and()`, `or()`, `negate()`, `isEqual()`.
- **Supplier<T>**: `T get()` — 값 공급(매개변수 없음).
- **Consumer<T>**: `void accept(T t)` — 소비(반환값 없음). `andThen()`로 연쇄 처리.
- **Function<T,R>**: `R apply(T t)` — 변환(입력 T → 결과 R).
- **UnaryOperator<T>**: `T apply(T t)` — **동일 타입** 변환(T → T).
- **BinaryOperator<T>**: `T apply(T t1, T t2)` — **동일 타입** 2항 연산((T,T) → T).

## stream
- **대상**: 배열·컬렉션 등 **연속된 데이터**를 선언적 파이프라인으로 처리.
- **처리 단계**
    1) **생성**: `Collection.stream()` 등으로 `Stream` 획득
    2) **중개 연산**(가공): 여러 개 연결 가능, **지연(lazy)**
    3) **종단 연산**(결과): 수치/집계/수집 등으로 종료
- **병렬 처리**: `parallelStream()` 제공(속도 이점 ↔ 스레드/CPU 부하·순서 보장 이슈) → 일반 웹 앱에선 보통 `stream()` 권장.

## stream forEach( )
- **종단 연산**: 각 요소를 꺼내 **부수 효과 작업** 수행 시 사용.
- **주의**: 순서 보장이 필요하면 컬렉션 특성/연산에 유의(필요 시 `forEachOrdered` 고려).

## 메소드 참조 (Method Reference, `::`)
- **정적 메서드**: `클래스명::정적메서드` (예: `Integer::parseInt`)
- **특정 객체의 인스턴스 메서드**: `객체참조::메서드`
- **특정 타입의 임의 객체 인스턴스 메서드**: `타입::메서드`
- **생성자**: `클래스명::new`, 배열 생성자 `타입[]::new`

## stream map( )
- **중개 연산**: 각 요소를 **다른 값으로 변환**(예: `Student → String name`).
- **활용**: 도메인 객체에서 특정 필드만 추출, 타입 변환 등.

## stream filter( )
- **중개 연산**: **조건에 맞는 요소만 통과**. 이후 연산/종단 연산의 입력 축소.

## Stream을 다시 한번 정리해 보자
- **생성**: `stream()`
- **중개 연산**: `filter()`, `map()`, `distinct()`, `sorted()`, `limit()`, `skip()` …
- **종단 연산**: `forEach()`, `toArray()`, `reduce()`, `collect()`, `min()`, `max()`, `count()`, `anyMatch()`, `findFirst()` …
