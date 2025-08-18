# ch.21 실수를 방지하기 위한 제네릭이라는 것도 있어요

---

## 1. 제네릭의 기본 개념
- **제네릭(Generics)**: 클래스나 메서드에서 사용할 데이터 타입을 외부에서 지정할 수 있도록 하는 기능
- 코드의 재사용성과 타입 안정성을 높임
- 잘못된 타입 사용을 **컴파일 시점**에 방지

```java
public class Generic<T> {
    private T object;
    public void setObject(T obj) { this.object = obj; }
    public T getObject() { return object; }
}
```

---

## 2. 제네릭 클래스 사용 예시
```java
Generic<String> g1 = new Generic<>();
g1.setObject("Hello");
String str = g1.getObject();
```

> `T` 대신 구체적인 타입(String, Integer 등)을 지정하면 형변환(casting) 필요 없음

---

## 3. 제네릭 메서드
- 메서드 단위로 타입 매개변수를 선언 가능

```java
public <T> void printArray(T[] array) {
    for (T element : array) {
        System.out.println(element);
    }
}
```

---

## 4. 타입 제한(Bounded Generics)
- `extends` 키워드를 사용해 타입 매개변수 제한 가능
- 상위 클래스나 인터페이스를 상속(구현)한 타입만 허용

```java
public <T extends Number> void printNumbers(T[] numbers) {
    for (T num : numbers) {
        System.out.println(num);
    }
}
```

---

## 5. 와일드카드(Wildcards)
- `?` 키워드를 사용하여 불특정 타입을 표현

### (1) Unbounded Wildcard
```java
public void printList(List<?> list) { ... }
```

### (2) Upper Bounded Wildcard
```java
public void processList(List<? extends Number> list) { ... }
```

### (3) Lower Bounded Wildcard
```java
public void addNumbers(List<? super Integer> list) { ... }
```

---

## 6. 와일드카드와 Bounded Wildcard 차이
- `?` : 모든 타입 가능
- `? extends Type` : Type과 그 하위 타입 허용 (읽기 전용에 적합)
- `? super Type` : Type과 그 상위 타입 허용 (쓰기 가능)

---

## 7. 제네릭 타입의 이점
1. **타입 안정성**: 잘못된 타입을 컴파일 시점에 방지
2. **형변환 불필요**: 코드 간결화
3. **재사용성**: 다양한 타입에 대해 동일한 코드 사용 가능

---

✅ **정리**
- 제네릭은 자바 컬렉션 프레임워크와 함께 자주 사용
- `extends`와 `super`를 통한 타입 제한은 안전하고 유연한 API 설계에 필수
- 와일드카드를 잘 활용하면 범용성과 타입 안전성을 동시에 확보 가능
