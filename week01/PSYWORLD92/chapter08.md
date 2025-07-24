## ch.8 참조 자료형에 대해서 더 자세히 알아봅시다

### ✅ 자바 타입의 종류

- **기본 자료형**: `byte`, `short`, `int`, `long`, `float`, `double`, `boolean`, `char`
- **참조 자료형**: 배열, 클래스, 인터페이스 등  
  → `new`로 생성
- **예외**: `String`은 참조형이지만 `new` 없이도 사용 가능

---

### ✅ 기본 생성자란?

- 클래스에 **생성자를 따로 정의하지 않으면 자바가 자동 생성**해주는 기본 생성자
- 단, **다른 생성자가 있으면 기본 생성자는 자동 생성되지 않음**

---

### ✅ 생성자의 목적

- 객체를 생성하고 초기화하기 위해 사용
- 클래스명과 이름이 같고 **리턴 타입이 없음**

```java
public class MemberDTO {
    public String name;

    // 생성자
    public MemberDTO(String name) {
        this.name = name;  // this: 인스턴스 변수 구분
    }
}
```

---

### ✅ this 키워드

- **객체의 인스턴스 변수와 매개변수를 구분**할 때 사용

```java
this.name = name;  // 왼쪽은 인스턴스 변수, 오른쪽은 매개변수
```

- 만약 이름이 다르면 `this` 생략 가능

```java
public MemberDTO(String paramName) {
    name = paramName;
}
```

---

### ✅ 메서드 오버로딩

- **메서드 이름은 같지만**,  
  **매개변수의 개수, 타입, 순서가 다르면 다른 메서드로 인식**

---

### ✅ 메서드 종료 조건

1. 모든 문장 실행 시
2. `return` 문에 도달했을 때
3. 예외 발생 시

> `return` 이후 코드가 있으면 컴파일 에러 발생  
> 단, `if`로 분기한 경우는 예외

```java
public void wantToStop(boolean flag) {
    if (flag) return;  // 중간 종료 가능
    // 나머지 처리
}
```

---

### ✅ static 메서드와 일반 메서드

- `static`은 **객체 생성 없이 사용 가능**
- 예: `System.out.println()`도 `static` 메서드

```java
public static void sayHi() {
    System.out.println("Hi");
}
```

- 일반 메서드는 반드시 객체를 통해 호출해야 함
- `static` 메서드는 **클래스 변수만 접근 가능**

> 너무 많이 쓰면 **공유 변수 문제**로 위험할 수 있음

---

### ✅ static 블록

```java
static {
    // 클래스 로딩 시 단 한 번만 실행됨
}
```

- 메서드 밖, 클래스 안에서만 선언 가능  
- 객체 없이 실행되는 초기화 블록

---

### ✅ 값 전달 방식 (Pass by...)

| 구분 | 설명 |
|------|------|
| Pass by Value | 기본형: 값 복사 → 원본 영향 없음 |
| Pass by Reference | 참조형: 주소 복사 → 내부 변경 시 원본 영향 있음 |

> 단, 참조형도 **new로 새 객체를 생성하면 바깥 객체에 영향 없음**

---

### ✅ 가변 인자 (Varargs)

- `...`을 사용하여 **매개변수 개수를 가변적으로 처리**
- **반드시 마지막 매개변수로만 사용 가능**
- **한 메서드에 하나만 허용**

```java
public void printAll(String title, int... numbers) {
    System.out.println(title);
    for (int n : numbers) {
        System.out.println(n);
    }
}
```

> ❌ 아래와 같은 형태는 불가

```java
public void printAll(String... title, int... numbers); // ❌
public void printAll(String... title, int numbers);    // ❌
```
