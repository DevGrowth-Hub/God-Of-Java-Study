# ch.13 인터페이스·추상클래스·final·enum

---

## 0) 개발 절차와 ‘계약’의 필요성
- **분석 → 설계 → 개발/테스트 → 릴리즈**로 진행.
- 설계 단계에서 **메서드 시그니처(계약)** 를 먼저 정의하면, 구현(DBMS·외부 API 등)이 달라도 교체가 쉽고 팀 협업이 편해짐.  
  → 이 계약을 **인터페이스**나 **추상클래스**로 명시.

---

## 1) 인터페이스 (interface)

### 핵심 개념
- **몸통 없는 메서드(선언만)** 으로 *규격/계약* 정의.
- `public interface`로 선언, **new 불가**.
- **implements**로 구현하며, *선언된 모든 메서드를 반드시 구현*해야 함(누락 시 컴파일 에러 → 그 클래스도 `abstract`여야 함).

### 계약 정의
```java
// c/service/MemberManager.java
package c.service;
import c.model.MemberDTO;

public interface MemberManager {
    boolean addMember(MemberDTO member);
    boolean removeMember(String name, String phone);
    boolean updateMember(MemberDTO member);
}
```

### 구현 및 사용
```java
// c/service/MemberManagerImpl.java
package c.service;
import c.model.MemberDTO;

public class MemberManagerImpl implements MemberManager {
    @Override public boolean addMember(MemberDTO member) { return false; }
    @Override public boolean removeMember(String name, String phone) { return false; }
    @Override public boolean updateMember(MemberDTO member) { return false; } // 누락 시 컴파일 에러
}

// 다형성 사용 예
import c.service.MemberManager;
import c.service.MemberManagerImpl;

public class InterfaceExample {
    public static void main(String[] args) {
        MemberManager member = new MemberManagerImpl();
    }
}
```

---

## 2) 추상클래스 (abstract class)

### 핵심 개념
- **추상 메서드**(선언만) + **구현 메서드**(몸통 있음)를 혼합해 *부분 구현/공통 로직* 제공.
- 선언에 `abstract` 사용, **new 불가**.
- **extends**로 상속하며, 자식 클래스가 *모든 추상 메서드를 구현*해야 함.

### 정의/상속 예시
```java
// c/service/MemberManagerAbstract.java
package c.service;
import c.model.MemberDTO;

public abstract class MemberManagerAbstract {
    public abstract boolean addMember(MemberDTO member);
    public abstract boolean removeMember(String name, String phone);
    public abstract boolean updateMember(MemberDTO member);
    public void printLog(String data) { System.out.println("Data=" + data); } // 공통 로직
}

// c/service/MemberManagerImpl2.java
package c.service;
import c.model.MemberDTO;

public class MemberManagerImpl2 extends MemberManagerAbstract {
    @Override public boolean addMember(MemberDTO member) { return false; }
    @Override public boolean removeMember(String name, String phone) { return false; }
    @Override public boolean updateMember(MemberDTO member) { return false; }
}
```

---

## 3) 인터페이스 vs 추상클래스 vs 일반클래스

| 항목 | 인터페이스 | 추상클래스 | 클래스 |
|---|---|---|---|
| 선언 키워드 | `interface` | `abstract class` | `class` |
| **추상 메서드 포함** | **가능(필수)** | 가능 | 불가 |
| **구현 메서드 포함** | (자바8+ `default`/`static`로 가능) | **가능** | **가능** |
| `static` 메서드 선언 | 가능(자바8+) | 가능 | 가능 |
| `final` 메서드 선언 | 불가 | 가능 | 가능 |
| **상속(`extends`)** | 불가 | 가능 | 가능 |
| **구현(`implements`)** | 가능 | 불가 | 불가 |
| 인스턴스화 | 불가 | 불가 | 가능 |

> **선택 기준**: 여러 구현 교체/플러그인 구조 → *인터페이스* / 공통 로직 제공+일부 규격 강제 → *추상클래스*

---

## 4) final — 상속·오버라이드·재할당 제한

### 4-1) 클래스에 `final` : 상속 금지
```java
package c.util;
public final class FinalClass { }

// public class FinalChildClass extends FinalClass { } // 컴파일 에러
```

### 4-2) 메서드에 `final` : 오버라이드 금지
```java
package c.util;
public abstract class FinalMethodClass {
    public final void printLog(String data) {
        System.out.println("Data=" + data);
    }
}

// public class FinalMethodChildClass extends FinalMethodClass {
//     @Override public void printLog(String data) { } // 에러: overridden method is final
// }
```

### 4-3) 변수에 `final` : 한 번만 할당
- **인스턴스 변수**는 *선언 시* 또는 *생성자에서* 반드시 초기화.
- **매개변수/지역 변수**는 재할당 금지(선언 시 즉시 초기화 필요 X, 단 지역 변수는 사용 전 반드시 초기화).
- **참조형 `final`** 은 *참조 재바인딩 금지*일 뿐, **객체 내부 상태 변경은 가능**.

```java
// 인스턴스 변수
public class FinalVariable {
    final int instanceVariable;
    public FinalVariable() { this.instanceVariable = 1; }
}

// 매개/지역 변수
public void method(final int parameter) {
    final int localVariable = 2;
    // parameter = 4;   // 불가
    // localVariable = 3; // 불가
}

// 참조형 final
import c.model.MemberDTO;
public class FinalReferenceType {
    final MemberDTO dto = new MemberDTO();  // 재바인딩 불가
    public void checkDTO() {
        System.out.println(dto);
        // dto = new MemberDTO(); // 불가
        dto.name = "Sangmin";    // 내부 상태는 변경 가능
        System.out.println(dto);
    }
}
```

---

## 5) enum — 상수의 집합 (열거형)

### 5-1) 기본 선언과 switch 활용
```java
// c/enums/OverTimeValues.java
package c.enums;
public enum OverTimeValues {
    THREE_HOUR, FIVE_HOUR, WEEKEND_FOUR_HOUR, WEEKEND_EIGHT_HOUR
}

// c/enums/OverTimeManager.java
package c.enums;
public class OverTimeManager {
    public int getOverTimeAmount(OverTimeValues value) {
        int amount = 0;
        switch (value) {
            case THREE_HOUR:         amount = 18000; break;
            case FIVE_HOUR:          amount = 30000; break;
            case WEEKEND_FOUR_HOUR:  amount = 40000; break;
            case WEEKEND_EIGHT_HOUR: amount = 60000; break;
        }
        return amount;
    }
}
```

### 5-2) ‘상수에 값 붙이기’ — 생성자 + 필드 + 게터
```java
// c/enums/OverTimeValues2.java
package c.enums;
public enum OverTimeValues2 {
    THREE_HOUR(18000), FIVE_HOUR(30000),
    WEEKEND_FOUR_HOUR(40000), WEEKEND_EIGHT_HOUR(60000);
    private final int amount;
    OverTimeValues2(int amount){ this.amount = amount; }
    public int getAmount(){ return amount; }
}
```

- enum의 생성자는 기본적으로 **private**(명시 안해도 됨), 외부에서 `new` 불가.
- `getAmount()`로 switch 없이 금액을 가져올 수 있어 코드가 간결해짐.

### 5-3) 순서/메서드 활용
- `compareTo()` : 선언 순서(ordinal) 비교 → 같으면 0, 앞이면 음수, 뒤면 양수.
```java
public class OverTimeManager2 {
    public static void main(String[] args) {
        OverTimeValues2 value2 = OverTimeValues2.FIVE_HOUR;
        System.out.println(value2);              // FIVE_HOUR
        System.out.println(value2.getAmount());  // 30000

        OverTimeValues2 value3 = OverTimeValues2.THREE_HOUR;
        System.out.println(value2.compareTo(value3)); // 1 (뒤에 선언된 값과 비교)
    }
}
```
- `values()` : enum **모든 상수 배열** 반환(반복문에 유용).
```java
public class OverTimeManager3 {
    public static void main(String[] args) {
        OverTimeValues2[] valueList = OverTimeValues2.values();
        for (OverTimeValues2 v : valueList) {
            System.out.println(v);
        }
    }
}
```

### 5-4) enum의 부모와 제공 메서드
- **모든 enum의 부모는 `java.lang.Enum`**(컴파일러가 자동 상속). 사용자가 임의로 다른 클래스를 `extends` 할 수 없음.
- 주요 메서드
  - `name()` : 상수 이름
  - `ordinal()` : 선언 순서(0부터)
  - `getDeclaringClass()` : 소속 enum 타입
  - `valueOf(Class<T>, String)` : 이름으로 상수 조회
  - `compareTo(E e)` : 순서 비교
  - (참고) `toString()`은 `name()`과 유사하게 동작하며 enum에서는 일반적으로 Overriding 필요 없음

---
