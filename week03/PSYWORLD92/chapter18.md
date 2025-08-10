
# 📘 Chapter 18: 이제 기본 문법은 거의 다 배웠으니 정리해 봅시다

자바의 핵심 문법을 요약 정리한 챕터입니다. 객체지향의 기본 개념부터 클래스, 변수, 반복문, 예외처리, 인터페이스, 어노테이션까지 전반적인 내용을 다룹니다.

---

## 1. 객체지향 핵심 개념 정리

- **클래스(Class)**: 상태(state)와 행동(behavior)을 가지는 자바의 기본 단위
- **상태(state)**: 인스턴스 변수
- **행위(behavior)**: 메서드
- **캡슐화(Encapsulation)**: 정보 은닉 + 메서드 제공
- **메시지(Message)**: 메서드 간 호출을 통해 값 전달
- **객체(Object)**: 클래스로부터 생성된 인스턴스
- **상속(Inheritance)**: `extends` / `implements` 를 통한 기능 확장
- **다형성(Polymorphism)**: 같은 메서드가 자식 클래스에서 다르게 동작

---

## 2. 변수와 타입

- **기본형**: byte, short, int, long, float, double, char, boolean
- **참조형**: 클래스, 배열, 인터페이스 등 객체를 참조하는 변수

### 기본형의 초기값
- byte, short, int, long: 0, 0L
- float, double: 0.0f, 0.0d
- char: ''
- boolean: false

---

## 3. 연산자

- **산술 연산자**: +, -, *, /, %
- **증감 연산자**: ++, --
- **비교 연산자**: ==, !=, >, < 등
- **논리 연산자**: &&, ||, !
- **비트 연산자**: &, |, ^, ~
- **삼항 연산자**: 조건 ? 참 : 거짓
- **대입 연산자**: =, +=, -= 등
- **우선순위와 결합 방향**도 숙지 필요

---

## 4. 제어문

### 조건문
- if, if-else, if-else if, switch-case

### 반복문
- for, while, do-while
- **break**: 루프 탈출
- **continue**: 다음 반복으로 건너뜀

---

## 5. 접근 제어자

| 제어자        | 해당 클래스 | 같은 패키지 | 상속 클래스 | 외부 클래스(import) |
|---------------|-------------|--------------|--------------|----------------------|
| public        | O           | O            | O            | O                    |
| protected     | O           | O            | O            | X                    |
| (default)     | O           | O            | X            | X                    |
| private       | O           | X            | X            | X                    |

---

## 6. 메서드

```java
public void method(String param, int... params) {
    // 메서드 내용
}
```

- 가변 인자 사용 가능
- 반환 타입 void
- 오버로딩: 같은 이름, 다른 매개변수
- 오버라이딩: 부모 클래스의 메서드를 자식이 재정의

---

## 7. 클래스 관련 문법

- **상속**: `extends`, `implements`
- **생성자 호출**: `super()` 사용
- **인터페이스**: 구현 메서드 없음, `implements`
- **추상 클래스**: 일부만 구현된 클래스, `abstract`
- **enum**: 열거형 상수 선언 클래스

---

## 8. Object 클래스

- 모든 클래스의 부모
- 주요 메서드: `toString()`, `equals()`, `hashCode()`, `getClass()`, `clone()`, `finalize()` 등

---

## 9. String 클래스

- 문자열을 다루는 대표 클래스
- 주요 메서드: `length()`, `equals()`, `substring()`, `indexOf()`, `split()`, `concat()`, `contains()` 등

---

## 10. 예외 처리

```java
try {
    // 예외 발생 가능 코드
} catch (Exception e) {
    // 예외 처리
} finally {
    // 항상 실행
}
```

- **throw**: 예외 발생
- **throws**: 예외 전달
- **checked / unchecked exception** 구분

---

## 11. 어노테이션과 메타 어노테이션

- 어노테이션 선언 예
```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface AnnotationDefinition {
    int number();
    String text() default "This is first annotation";
}
```

- `@Override`, `@Deprecated`, `@SuppressWarnings` 등 기본 제공
- 메타 어노테이션: `@Target`, `@Retention`, `@Inherited`, `@Documented`

---

## 12. RetentionPolicy 종류

| 요소 타입 | 대상 설명 |
|-----------|-----------|
| SOURCE    | 컴파일 시 사라짐 |
| CLASS     | 컴파일러에서 보존되나 JVM에서 사라짐 |
| RUNTIME   | 실행 시까지 유지되어 리플렉션으로 참조 가능 |

---

