# 📘 Chapter 14. 다 배운 것 같지만, 예외라는 중요한 것이 있어요

## 1. 예외 처리의 개념

### 정의 및 목적
- 자바 프로그램 실행 중 **예상치 못한 상황**에 대한 **안전 장치**
- 예: `null` 객체에 메서드 호출 시 예외 발생

---

## 2. try-catch 블록

### 기본 구조 및 동작
- 예외 발생 가능 코드를 `try`에, 예외 처리 코드를 `catch`에 작성
- 예외 발생 시 `try` 블록 이후는 실행되지 않고 `catch`로 이동
- 예외 없으면 `try` 블록 전체 실행, `catch`는 무시됨

### 주의사항
- `try` 내 변수는 `catch`에서 사용할 수 없음
- `catch`는 여러 개 가능 → **구체적인 예외부터 작성, 가장 상위 Exception은 마지막에**

---

## 3. finally 블록

### 정의 및 용도
- 예외 발생 여부와 관계없이 **항상 실행**
- **리소스 정리**, **코드 중복 제거** 등에 유용

---

## 4. 예외의 종류

| 유형                | 설명                                                                 |
|---------------------|----------------------------------------------------------------------|
| `Error`             | 자바 외부 문제. 심각한 오류로 복구 불가 (예: `OutOfMemoryError`)     |
| `RuntimeException`  | 실행 중 발생. **Unchecked Exception** (예: `NullPointerException`)     |
| `Checked Exception` | 컴파일 시 감지되는 예외. 반드시 예외 처리 필요                        |

---

## 5. java.lang.Throwable 클래스

### 특징
- 모든 예외와 에러의 최상위 부모 클래스
- 공통 메서드:
    - `getMessage()`: 예외 메시지
    - `toString()`: 예외 정보 문자열
    - `printStackTrace()`: 스택 트레이스 출력

---

## 6. throw 키워드

### 정의
- **개발자가 직접 예외를 발생시킬 때** 사용
- 예: `throw new IllegalArgumentException("잘못된 입력")`

---

## 7. throws 키워드

### 정의
- 메서드 선언부에 예외 발생 가능성 명시
- 예외 처리 방식:
    - 호출 측에서 `try-catch`로 감싸거나
    - 다시 `throws`로 전달

---

## 8. 사용자 정의 예외

### 구현 방식
- `Exception` 또는 `RuntimeException` 상속
- 일반적으로 **`RuntimeException` 상속**이 더 선호됨 → **강제 예외 처리 불필요**

---

## 9. 자바 예외 처리 전략

- 예외 처리 시 일관된 **표준 방식 유지**
- `RuntimeException`도 try-catch로 감싸는 것이 **안정성 향상**
- **비어 있는 catch 문 금지** → 로그 기록 등으로 원인 추적 가능하게 해야 함

## ❓
1. system.out.println() vs System.err.println()의 차이는?
2. system.out.print를 지양 해야 하는 이유는?
3. 이미 예외를 throws로 선언한 메서드를 호출하는 쪽에서 다시 throws를 선언하는 것이 바람직한 경우는 언제이며, 그 이유는
