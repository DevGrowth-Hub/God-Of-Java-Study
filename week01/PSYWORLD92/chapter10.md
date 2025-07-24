## ch.10 자바는 상속이라는 것이 있어요

### ✅ 상속 (Inheritance)

```java
public class Child extends Parent { }
```

- `Child`는 `Parent`의 모든 `public`, `protected` 멤버 사용 가능

---

### ✅ 부모 생성자 호출 규칙

- 부모에 **기본 생성자(매개변수 없는 생성자)**가 없을 경우,
  - 자식 클래스에서 `super(...)`로 명시적으로 호출해야 함

```java
public class Parent {
    public Parent(String name) { ... }
}

public class Child extends Parent {
    public Child() {
        super("childarg");  // 부모 생성자 호출
    }
}
```

> `super(...)`는 **생성자 첫 줄**에만 올 수 있음  
> 생략 시 자동으로 `super()`가 컴파일러에 의해 추가됨

---

### ✅ 메서드 오버라이딩 (Overriding)

- 자식 클래스에서 부모 클래스의 메서드를 **재정의**하는 것

```java
@Override
public void sayHello() {
    System.out.println("Hi!");
}
```

- 조건:
  - 메서드 이름, 리턴타입, 매개변수 개수/순서 동일
  - 접근 제어자는 **동일하거나 더 넓어야 함**

| 부모 접근자 | 자식에서 가능한 범위 |
|-------------|-----------------------|
| `public`    | `public`              |
| `protected` | `protected`, `public` |
| `private`   | 오버라이딩 불가       |

---

### ✅ 참조 자료형 형변환

- 자식 → 부모: **업캐스팅 (자동)**
- 부모 → 자식: **다운캐스팅 (명시적)**

```java
Parent p = new Child();         // ✅ 업캐스팅
Child c = (Child) p;            // ✅ 다운캐스팅
```

> **업캐스팅은 안전**, 다운캐스팅은 반드시 실제 객체가 자식이어야 함

---

## ✅ 다형성 (Polymorphism)

> "형태가 여러 가지다"  
> 즉, **하나의 타입(부모 타입)**으로 **여러 자식 클래스의 객체를 담을 수 있음**

```java
class Animal {
    public void speak() {
        System.out.println("Animal speaks");
    }
}

class Dog extends Animal {
    public void speak() {
        System.out.println("멍멍");
    }
}

class Cat extends Animal {
    public void speak() {
        System.out.println("야옹");
    }
}
```

```java
Animal a1 = new Dog();   // 업캐스팅
Animal a2 = new Cat();   // 업캐스팅

a1.speak(); // 멍멍
a2.speak(); // 야옹
```

- `a1`, `a2`는 모두 `Animal` 타입이지만, 실제 객체는 `Dog`, `Cat`
- 메서드를 호출하면 **실제 객체 기준으로 동작**한다

---

### ✅ 다형성의 장점

- 여러 자식 클래스 객체를 하나의 부모 타입으로 처리 가능
- 반복문, 배열, 메서드 등에서 **코드 유연성 증가**

```java
public void makeAnimalSpeak(Animal animal) {
    animal.speak();
}
```

- `makeAnimalSpeak(new Dog())`  
- `makeAnimalSpeak(new Cat())`  
→ **메서드는 하나지만 다양한 동작**

---

### ✅ 비유로 이해하는 다형성

> 부모 타입 = 리모컨, 자식 객체 = TV, 에어컨  
> 리모컨(부모)으로 어떤 기기든 조작 가능하지만, 실제 동작은 기기에 따라 달라짐

---

### ✅ 요약

| 개념 | 설명 |
|------|------|
| 상속 | 부모 클래스의 속성과 메서드를 자식이 물려받음 |
| 오버라이딩 | 자식이 부모의 메서드를 재정의함 |
| 업캐스팅 | 자식 → 부모 (자동) |
| 다운캐스팅 | 부모 → 자식 (명시적) |
| 다형성 | 하나의 부모 타입으로 여러 자식 객체를 다룰 수 있음 |
