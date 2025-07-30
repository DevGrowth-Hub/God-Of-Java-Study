# ch.12 모든 클래스의 부모는 Object예요

자바에서 모든 클래스는 명시적으로 `extends` 하지 않아도 `java.lang.Object` 클래스를 자동으로 상속받습니다. 따라서 모든 클래스는 `Object`가 제공하는 메서드를 사용할 수 있으며, 필요에 따라 재정의(Overriding)하여 활용합니다.

---

## ✅ Object 클래스의 주요 메서드
- **toString()** : 객체를 문자열로 표현  
- **equals(Object obj)** : 두 객체의 논리적 동등성 비교  
- **hashCode()** : 객체의 해시코드(정수값) 반환  
- **clone()** : 객체 복사 (Cloneable 인터페이스 필요)  
- **getClass()** : 객체의 클래스 정보 반환  
- **finalize()** : GC 수행 직전에 호출 (Java 9부터 비권장)  
- **wait()/notify()/notifyAll()** : 멀티스레드 동기화 제어  

---

## ✅ toString() 메서드
**기본 구현**
```java
getClass().getName() + "@" + Integer.toHexString(hashCode())
```
**자동 호출되는 경우**
- `System.out.println(obj);`
- `"문자열" + obj`

**오버라이딩 예시**
```java
@Override
public String toString() {
    return "MyClass[name=" + name + ", value=" + value + "]";
}
```

---

## ✅ equals(Object obj) 메서드
**기본 동작**: 참조값(주소) 비교 (`==`)  
**재정의 이유**: 실제 객체의 속성 값을 기준으로 동등성 판단 필요

**equals() 오버라이딩 규칙**
1. **반사성(Reflexive)**: `x.equals(x)` → `true`
2. **대칭성(Symmetric)**: `x.equals(y)`가 `true`면 `y.equals(x)`도 `true`
3. **추이성(Transitive)**: `x.equals(y)`이고 `y.equals(z)`면 `x.equals(z)`도 `true`
4. **일관성(Consistent)**: 동일 입력에 대해 반복 호출 시 결과가 변하지 않음
5. **null 비교**: `x.equals(null)`은 항상 `false`

**오버라이딩 예시 (MemberDTO)**
```java
@Override
public boolean equals(Object obj) {
    if (this == obj) return true;
    if (obj == null || getClass() != obj.getClass()) return false;
    MemberDTO other = (MemberDTO) obj;
    return Objects.equals(name, other.name) &&
           Objects.equals(phone, other.phone) &&
           Objects.equals(email, other.email);
}
```

---

## ✅ hashCode() 메서드
**역할**: 객체를 식별하는 해시 기반 정수 반환  
**주의사항**: `equals()`가 `true`인 두 객체는 **동일한 `hashCode()`**를 반드시 반환해야 함 (HashMap/HashSet 등에서 필수)

**오버라이딩 예시 (MemberDTO)**
```java
@Override
public int hashCode() {
    final int prime = 31;
    int result = 1;
    result = prime * result + Objects.hashCode(name);
    result = prime * result + Objects.hashCode(phone);
    result = prime * result + Objects.hashCode(email);
    return result;
}
```
