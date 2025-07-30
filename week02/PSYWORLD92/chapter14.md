# Ch.14 다 배운 것 같지만, 예외라는 중요한 것이 있어요

자바에서 **모든 예외/오류의 뿌리**는 `java.lang.Throwable` 입니다.  
`Throwable` 아래에 **두 갈래**가 있습니다.

```
Throwable
 ├─ Error                // 시스템/환경 문제 (복구 불가)
 └─ Exception            // 애플리케이션 문제 (복구 가능성 있음)
     └─ RuntimeException // 미검사(언체크드) 예외
```

---

## 1) Error vs Exception

| 구분 | Error | Exception |
|---|---|---|
| 의미 | JVM 밖/시스템 환경에서 발생하는 **치명적 상황** | 프로그램 로직/입력/상태 문제 등 **앱 내부**에서 발생 |
| 영향 범위 | **프로세스** 전체에 영향 → 보통 **중단** | 보통 **현재 쓰레드**에 국한, 적절히 처리하면 **계속 진행 가능** |
| 예시 | `OutOfMemoryError`, `StackOverflowError`, `LinkageError` | `IOException`, `SQLException`, `IllegalArgumentException` |
| 처리 원칙 | **잡지 말 것**(대부분 복구 불가) | **잡거나(복구/대체) 위로 전파** |
| 상속 | `java.lang.Error` | `java.lang.Exception` |

> 교재가 *Error를 “예외 종류”에 넣고 싶지 않다*고 한 이유:  
> **예외(Exception)** 는 “처리해서 정상 흐름으로 복구할 가능성”을 전제로 하지만, **에러(Error)** 는 대부분 **복구 불가**이기 때문입니다.

---

## 2) 예외의 분류(자바 관점)

1. **Checked Exception (검사 예외)**  
   - `Exception`의 하위이지만 **`RuntimeException`은 제외**  
   - **컴파일러가 강제**: `throws` 선언 또는 `try-catch` 필요  
   - 예: `IOException`, `SQLException`, `ParseException`

2. **Runtime Exception (Unchecked, 언체크드)**  
   - `RuntimeException` 하위  
   - **컴파일 타임 강제 없음**(개발자 선택)  
   - 예: `NullPointerException`, `IllegalArgumentException`, `IndexOutOfBoundsException`, `IllegalStateException`

3. **Error**  
   - 앞 섹션 참고(대개 잡지 않음)

> 선택 가이드  
> - **복구 가능**하고 **호출자에게 조치**를 요구해야 하는 상황 → *Checked*  
> - **프로그래밍 오류/사전검증으로 예방 가능**한 상황(잘못된 인자, 상태 위반 등) → *Runtime*

---

## 3) `try-catch` 작성 규칙 (컴파일 에러 포인트)

- **구체적인(자식) 예외를 먼저**, 더 **일반적인(부모) 예외를 나중에**.
- 맨 앞에서 `Exception`을 잡으면 **뒤의 모든 catch는 도달 불가** → 컴파일 에러.

```java
try {
    // ...
} catch (Exception e) {        // ❌ 이렇게 쓰면
} catch (IOException e) {      // 컴파일 에러(도달 불가)
}
```

**올바른 순서**
```java
try {
    // ...
} catch (FileNotFoundException e) {
    // ...
} catch (IOException e) {
    // ...
} catch (Exception e) {
    // 마지막 방어선
}
```

**멀티 캐치**(동일 처리 로직일 때)
```java
try {
    // ...
} catch (IOException | SQLException e) {
    // 공통 처리
}
```

**finally / try-with-resources**
```java
// 자원을 무조건 닫아야 한다면 try-with-resources 권장
try (BufferedReader br = Files.newBufferedReader(path)) {
    return br.readLine();
} catch (IOException e) {
    // 로깅 + 대체/전파
    throw new UncheckedIOException(e); // 예: 런타임으로 변환
}
```

---

## 4) `printStackTrace()`를 지양하는 이유와 대안

### 왜 지양?
- **과도한 출력**: 레벨 제어 없이 모든 스택트레이스를 콘솔에 덤프.  
- **수집 누락**: `System.err`로만 출력되어 **로그 파일/모니터링**에 연동되지 않을 수 있음.  
- **분석 어려움**: 요청 컨텍스트(사용자/트랜잭션 ID 등)와 **상황 정보가 없음**.

### 대안(권장 패턴)
```java
private static final Logger log = LoggerFactory.getLogger(MyService.class);

try {
    // ...
} catch (IOException e) {
    log.error("파일 읽기 실패: path={}, user={}", path, currentUserId, e); // ★ 예외 객체를 함께 전달
    // 필요하면 의미 있는 도메인 예외로 변환
    throw new FileReadFailedException(path, e);
}
```

> **핵심**: *로깅 레벨*과 *메시지(컨텍스트)* 를 함께 기록하고, 예외 객체를 **파라미터로 전달**하여 스택트레이스를 로거가 남기도록 합니다.

---

## 5) `throw` / `throws` 사용 전략

- **가능하면 최초 발생 지점에서 처리**(복구/대체 값/재시도).  
- **의미 있는 맥락을 추가**해 *번역(translation)하여 전파*하는 것이 좋음.  
- *이미 `throws`한 것을 상위에서 동일하게 다시 `throws`만 반복*하는 건 정보가 늘지 않아 가치가 낮음.

```java
// (나쁨) 맥락 없이 반복 전파
void a() throws SQLException { b(); }
void b() throws SQLException { c(); }

// (좋음) 맥락을 추가해 변환
void service() {
    try {
        repo.save(entity);
    } catch (SQLException e) {
        throw new OrderStoreException("주문 저장 실패: id=" + entity.getId(), e); // 도메인 예외로 변환
    }
}
```

> **원칙**: “**복구할 수 있거나 의미를 추가할 수 있는 곳**에서 처리/변환”.  
> 의미 없이 `throws`만 층층이 올리는 습관은 피합니다.

---

## 6) 커스텀 예외 설계 가이드

- **반드시 호출자가 처리해야 하는 상황** → `Exception`(Checked) 상속
- **프로그래밍 오류/사전 검증으로 막을 수 있는 상황** → `RuntimeException` 상속
- 공통 베이스를 두면 핸들러 구성에 유리

```java
// Checked
public class ResourceNotFoundException extends Exception {
    public ResourceNotFoundException(String id) { super("리소스를 찾을 수 없음: " + id); }
}

// Unchecked
public class BusinessException extends RuntimeException {
    public BusinessException(String message, Throwable cause) { super(message, cause); }
}
```

---

## 7) 빈 `catch`(삼킴) 금지 & 로깅 위치

```java
try {
    // ...
} catch (Exception e) {
    // 아무 것도 안 함 ❌ (문제 원인 추적 불가)
}

// 최소한:
catch (Exception e) {
    log.warn("처리 중 경고: {}", context, e);
}
```

> 같은 예외를 **여러 층에서 중복 로깅**하면 로그가 오염됩니다. 최종 처리 지점 1회 로깅을 권장(혹은 레벨을 달리).

---

## 8) Runtime Exception 요약

- **컴파일 시 강제 없음** → 호출자가 굳이 `try-catch`/`throws`하지 않아도 컴파일됨.  
- 주로 **잘못된 인자/상태/계약 위반**을 표현: `IllegalArgumentException`, `IllegalStateException` 등.  
- 방어 코드(검증)로 **사전 예방**이 기본 전략.

---

## 9) 스니펫 모음

**컨트롤러/핸들러에서 공통 처리(예: 스프링)**  
```java
@RestControllerAdvice
public class GlobalExceptionHandler {
    @ExceptionHandler(ResourceNotFoundException.class)
    public ResponseEntity<?> handleNotFound(ResourceNotFoundException e) {
        return ResponseEntity.status(HttpStatus.NOT_FOUND).body(e.getMessage());
    }

    @ExceptionHandler(Exception.class)
    public ResponseEntity<?> handleUnexpected(Exception e) {
        // 마지막 방어선: 로깅 + 일반 오류 응답
        log.error("Unhandled exception", e);
        return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).build();
    }
}
```

**도메인 입력 검증과 런타임 예외**
```java
public void changeQuantity(int qty) {
    if (qty < 0) throw new IllegalArgumentException("수량은 음수 불가: " + qty);
    this.qty = qty;
}
```

---

## 10) 핵심 요약

- `Throwable` → `Error`(복구 불가) / `Exception`(복구 가능성) → `RuntimeException`(언체크드).  
- **catch 순서**: 자식 → 부모. 맨 앞에서 `Exception` 잡지 말 것.  
- `printStackTrace()` 대신 **로깅 프레임워크** 사용, **맥락**을 담아 기록.  
- `throw/throws`: **최초 발생 지점**에서 처리하거나 **의미 있는 도메인 예외로 변환**.  
- **커스텀 예외**는 처리 요구 여부에 따라 `Exception`(checked) 또는 `RuntimeException`(unchecked) 상속.  
- **빈 catch 금지**, 중복 로깅 주의.  
- **Runtime 예외**는 프로그래밍 오류 신호 → 사전 검증/테스트로 예방.
