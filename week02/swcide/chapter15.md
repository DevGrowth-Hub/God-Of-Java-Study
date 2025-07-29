
## 1. 핵심 특성
- **final 클래스**: 상속 불가 → 안전성·일관성 보장
- **구현 인터페이스**:
    - `Serializable` (직렬화)
    - `Comparable<String>` (`compareTo()`로 사전 순 비교)
    - `CharSequence` (문자열 계열 공통 인터페이스)

---

## 2. 생성 및 변환
- **생성자**: `String(byte[] bytes)`, `String(byte[] bytes, String charsetName)`
- **변환**:
    - `getBytes()` / `getBytes(charset)` → 바이트 배열
    - `String.valueOf(...)` → 기본형·객체 → 문자열

---

## 3. 주요 비교·검색 메소드
- **내용 비교**: `equals()`, `equalsIgnoreCase()`
- **순서 비교**: `compareTo()`
- **검색**: `contains()`, `startsWith()`, `indexOf()`

---

## 4. 성능 최적화
- **불변성의 단점**: 반복적 `+` → 새로운 String 객체 생성 → GC 부담
- **해결책**:
    - **단일 스레드**: `StringBuilder`
    - **다중 스레드**: `StringBuffer`
- **자동 최적화**: `+` 연산이 컴파일 시 `StringBuilder`로 변환

---

## 5. 주의할 점
- **절대 지양**: `intern()` → 문자열 풀 과다 사용 시 성능·메모리 저하  

## ❓
1. String이 불변(Immutable)으로 설계된 이유와 `+` 연산 시 발생하는 메모리·GC 부하를 StringBuilder/StringBuffer가 어떻게 해결하는지?
2. Serializable 인터페이스의 역할은 무엇이며 String 클래스가 이를 구현하는 이유는? 
3. intern() 메서드 사용이 전체 애플리케이션 성능에 미치는 영향과 이를 지양해야 하는 이유?  

