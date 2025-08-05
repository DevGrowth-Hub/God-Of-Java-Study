# ch.16 클래스 안의 클래스
자바에서는 클래스 내부에 또 다른 클래스를 선언할 수 있는데, 이를 **Nested 클래스**라고 합니다.  
Nested 클래스를 사용하는 주된 이유는  
- 코드의 **논리적 묶음**  
- **캡슐화** 강화  
- **가독성** 및 **유지보수성** 향상  

---

## 1. Nested 클래스의 종류

1. **Static Nested 클래스**  
2. **Inner 클래스 (비정적 Nested 클래스)**  
   1. **로컬(Local) 내부 클래스**  
   2. **익명(Anonymous) 내부 클래스**

---

## 2. Static Nested 클래스

```java
public class Outer {
    private static String staticVar = "static 변수";
    private String instanceVar = "인스턴스 변수";

    public static class StaticNested {
        void print() {
            // Outer 클래스의 static 멤버만 접근 가능
            System.out.println(staticVar);
            // System.out.println(instanceVar); // 컴파일 에러
        }
    }
}
```

- **정의**: `static` 키워드를 붙여 선언  
- **특징**  
  - 외부 클래스의 **static 멤버**만 직접 참조 가능  
  - 외부 인스턴스 생성 없이 사용 가능  
  - 일반적으로 외부 클래스와 논리적으로 연관된 유틸리티 클래스를 정의할 때 사용  

---

## 3. Inner 클래스 (비정적 Nested 클래스)

### 3.1. 일반 내부 클래스

```java
public class Outer {
    private String instanceVar = "인스턴스 변수";

    public class Inner {
        void print() {
            // Outer 클래스의 모든 멤버 접근 가능 (private 포함)
            System.out.println(instanceVar);
        }
    }
}

// 사용 예시
Outer outer = new Outer();
Outer.Inner inner = outer.new Inner();
inner.print();
```

- **정의**: `static` 없이 선언  
- **특징**  
  - 외부 클래스의 **모든 멤버**(private 포함) 접근 가능  
  - 인스턴스 멤버와 밀접한 로직을 묶을 때 유용  
  - 생성 전, 반드시 외부 클래스 인스턴스가 필요

---

## 4. 로컬(Local) 내부 클래스

```java
public class Outer {
    void method() {
        // LocalInner는 method() 내에서만 사용 가능
        class LocalInner {
            void print() {
                System.out.println("로컬 내부 클래스");
            }
        }
        LocalInner li = new LocalInner();
        li.print();
    }
}
```

- **정의**: 메서드(또는 초기화 블록) 내부에 선언  
- **특징**  
  - 선언된 메서드 내부에서만 사용  
  - final 또는 effectively final인 외부 지역 변수만 참조 가능 (Java 8 이후부터는 effectively final 허용)

---

## 5. 익명(Anonymous) 내부 클래스

익명 내부 클래스는 **이름 없이** 선언과 동시에 **즉시 인스턴스화**합니다. 주로 다음과 같은 경우에 사용합니다.

- **인터페이스**나 **추상 클래스**를 단 한 번만 구현할 때  
- 짧은 이벤트 핸들러, 스레드 구현 등 간단한 콜백 로직  

### 정의 및 문법

```java
// 1) 인터페이스 구현
Runnable r = new Runnable() {
    @Override
    public void run() {
        System.out.println("익명 내부 클래스 Runnable 실행");
    }
};
new Thread(r).start();

// 2) 추상 클래스 확장
abstract class Greeting {
    abstract void say();
}

Greeting g = new Greeting() {
    @Override
    void say() {
        System.out.println("익명 내부 클래스 Greeting");
    }
};
g.say();
```

### 특징

- **클래스 이름이 없으므로**, 생성자는 가질 수 없고 초기화 블록만 사용 가능  
- **단일 인스턴스** 용도, 재사용 불필요한 로직에서 유용  
- 선언과 동시에 인스턴스화하기 때문에 코드가 **간결**해짐  
- 다수의 메서드를 오버라이드해야 할 경우 가독성이 떨어질 수 있음  
- Java 8 이후에는 람다 표현식으로 대체 가능한 경우가 많음

---

## 6. Nested 클래스 사용 시 고려사항

- **캡슐화**: 외부에 노출할 필요 없는 클래스는 내부에 숨겨 관리  
- **가독성**: 너무 많은 수준의 중첩은 오히려 복잡도를 높일 수 있음  
- **메모리**: Inner 클래스는 외부 인스턴스를 참조하므로 GC에 영향이 있을 수 있음  

---

### 요약

- **Static Nested 클래스**: 외부 인스턴스와 독립, static 멤버만 참조  
- **Inner 클래스**: 외부 인스턴스와 강하게 결합, 모든 멤버 참조 가능  
- **Local Inner 클래스**: 메서드 내부에서만 사용  
- **익명 내부 클래스**: 일회성 구현에 사용, 코드 간결화  
