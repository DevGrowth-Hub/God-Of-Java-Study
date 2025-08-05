# ch.17 어노테이션 (Annotations)

자바에서 **어노테이션(Annotation)** 은 클래스, 메소드, 필드 등 선언부에 `@` 기호를 사용하여 부가 정보를 제공하는 **메타데이터(metadata)** 입니다.  
어노테이션은 다음과 같은 용도로 사용됩니다.

- **컴파일러에게 정보 제공**: 컴파일 시 검사나 경고 제어  
- **빌드/배포 과정 지정**: 컴파일 단계나 패키징 단계에서 특정 작업 실행  
- **런타임 처리**: 리플렉션을 통해 실행 중에 동작 제어  

---

## 1. 주요 내장 어노테이션

| 어노테이션       | 설명                                                                         |
|----------------|------------------------------------------------------------------------------|
| `@Override`    | 슈퍼 클래스(또는 인터페이스)의 메소드를 **오버라이드**했음을 컴파일러에 명시               |
| `@Deprecated`  | 더 이상 사용되지 않는 **권장되지 않는** 요소임을 표시 (컴파일 시 경고)                  |
| `@SuppressWarnings` | 지정한 **경고(warning)** 메시지를 숨김 (예: `@SuppressWarnings("unchecked")`) |

```java
@Override
public String toString() {
    return "Example";
}

@Deprecated
public void oldMethod() { /* ... */ }

@SuppressWarnings("unchecked")
List rawList = new ArrayList();
```

---

## 2. 커스텀 어노테이션 정의

사용자 정의 어노테이션은 `@interface` 키워드로 선언합니다.  
```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface MyAnnotation {
    String value();
    int count() default 1;  // 기본값 지정
}
```

- **요소(속성)** 선언 시 메서드 형태로 정의  
- **기본값(default)** 설정 가능

---

## 3. 메타 어노테이션 (Meta-Annotations)

메타 어노테이션은 어노테이션을 **정의할 때** 사용되며, 사용 대상과 유지 정책 등을 지정합니다.

| 메타 어노테이션       | 설명                                                               |
|---------------------|------------------------------------------------------------------|
| `@Target`           | 어노테이션 **적용 대상을** 지정 (`ElementType` 열거형 사용)            |
| `@Retention`        | 어노테이션 **정보 유지 기간** 지정 (`SOURCE`, `CLASS`, `RUNTIME`) |
| `@Documented`       | `javadoc` 생성 시 API 문서에 **포함**되도록 표시                     |
| `@Inherited`        | **상속** 가능 여부 지정 (부모 클래스의 어노테이션이 자식 클래스에 적용) |

### `@Target` 예시
```java
@Target({
    ElementType.TYPE,       // 클래스, 인터페이스, enum
    ElementType.METHOD,     // 메소드
    ElementType.FIELD,      // 필드(멤버 변수)
    ElementType.CONSTRUCTOR,// 생성자
    ElementType.PARAMETER,  // 매개변수
    ElementType.LOCAL_VARIABLE, // 지역 변수
    ElementType.PACKAGE     // 패키지
})
public @interface MyTargeted {}
```

### `@Retention` 예시
```java
@Retention(RetentionPolicy.SOURCE)   // 컴파일러 단계에서 제거
public @interface SourceOnly {}

@Retention(RetentionPolicy.CLASS)    // 클래스 파일까지 유지, VM에는 제거
public @interface ClassRetention {}

@Retention(RetentionPolicy.RUNTIME)  // 런타임 시 리플렉션으로 접근 가능
public @interface RuntimeRetention {}
```

---

## 4. 어노테이션 활용 예

1. **검증 프레임워크**  
   ```java
   @NotNull
   private String name;
   ```
2. **JPA 엔티티 매핑**  
   ```java
   @Entity
   @Table(name="users")
   public class User {
       @Id @GeneratedValue
       private Long id;
       @Column(name="email", nullable=false)
       private String email;
   }
   ```
3. **스프링 DI**  
   ```java
   @Autowired
   private UserService userService;
   ```
4. **테스트 코드**  
   ```java
   @Test
   public void testAdd() {
       assertEquals(5, calculator.add(2, 3));
   }
   ```

---

## 5. 어노테이션 상속

- **직접 상속**(extends) 불가  
- `@Inherited` 메타 어노테이션을 사용하면 **클래스 상속** 시 적용 가능  
- **메소드** 어노테이션은 상속되지 않음

```java
@Inherited
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
public @interface InheritedAnn {}

@InheritedAnn
public class Parent {}

public class Child extends Parent {}  // Child 클래스에 inheritedAnn 적용
```

---

## 6. 요약

- 어노테이션은 **메타데이터**로, 컴파일러·빌드·런타임에 활용  
- 내장 어노테이션(`@Override`, `@Deprecated`, `@SuppressWarnings`) 외 **커스텀 어노테이션** 작성 가능  
- 메타 어노테이션(`@Target`, `@Retention`, `@Documented`, `@Inherited`)로 적용 범위·유지 정책·상속 여부 설정  
- 다양한 프레임워크(JPA, Spring, 테스트 툴 등)에서 **주도적으로** 사용  
