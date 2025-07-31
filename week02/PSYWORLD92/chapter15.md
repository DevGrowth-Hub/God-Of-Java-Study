# Ch.15 문자열(String)

자바에서 `String`은 **가장 많이 쓰이는 불변(immutable) 참조 타입**입니다.  
불변성, 상수 풀(String Pool), 문자 인코딩, 그리고 성능/메모리 관점의 활용법을 한 번에 정리합니다.

---

## 1) 왜 String이 VVIP인가?

- **불변(Immutable)**: 한 번 생성되면 내용이 바뀌지 않습니다.  
  → 보안/스레드 안전/캐시(예: `hashCode()` 캐싱) 측면에서 유리.
- **리터럴 상수 풀(String Pool)**: 동일한 리터럴을 **공유(reuse)** 하여 메모리 절감.
- **JDK 컴파일러 최적화**: 리터럴 결합은 컴파일 타임에 합쳐지고(상수 폴딩), 런타임 `+`는 `StringBuilder`로 변환됩니다.

```java
String a = "he" + "llo";    // 컴파일 시 "hello"로 상수 폴딩
String b = "hello";
System.out.println(a == b); // true (같은 풀 객체)
```

> 단, **런타임 결합**(변수 포함)에서는 내부적으로 `StringBuilder`가 생성됩니다.

---

## 2) 문자셋(Charset)과 인코딩/디코딩

- **문자 ↔ 바이트**의 변환 규칙이 **인코딩(Charset)** 입니다.  
  → 같은 문자열이라도 **어떤 Charset을 쓰느냐에 따라 바이트 배열이 달라짐**.

```java
byte[] eucKr = "가".getBytes(Charset.forName("EUC-KR"));
byte[] utf8  = "가".getBytes(StandardCharsets.UTF_8);
System.out.println(Arrays.toString(eucKr));
System.out.println(Arrays.toString(utf8));
```

- **웹(브라우저 ↔ Spring Boot) 기본은 UTF-8**:  
  - HTTP `Content-Type` 헤더의 `charset`으로 명시되거나, 명시 없을 시 **기본 UTF-8**로 처리됨.
  - Spring MVC의 `HttpMessageConverter`(예: `StringHttpMessageConverter`, `MappingJackson2HttpMessageConverter`)의 **기본 Charset은 UTF-8**.
- **플랫폼 기본 문자셋 의존 금지**: `new String(bytes)`나 `getBytes()`를 **인자 없이** 쓰면 플랫폼 기본(Charset.defaultCharset())을 사용하므로, **항상 명시**하세요.

```java
String s = new String(bytes, StandardCharsets.UTF_8);
byte[] out = s.getBytes(StandardCharsets.UTF_8);
```

---

## 3) `==` vs `equals()` — 왜 헷갈릴까?

- `==` : **같은 객체(참조)인지** 비교
- `equals()` : **내용(문자열 값)** 비교

**문자열 풀 덕분에** 다음은 `true`가 됩니다.
```java
String x = "java";
String y = "java";  // 풀의 같은 객체 참조
System.out.println(x == y);      // true
System.out.println(x.equals(y)); // true
```

하지만 `new String()`으로 만들면 **새 객체**입니다.
```java
String p = new String("java");  // 힙에 새 인스턴스
System.out.println(p == x);      // false
System.out.println(p.equals(x)); // true
```

> **규칙**: 문자열 비교는 **항상 `equals()`**(혹은 `Objects.equals`)로!

---

## 4) `intern()` — 왜 조심해야 하나?

`intern()`은 **동일 값의 풀 객체 참조**를 반환합니다. 없으면 풀에 추가합니다.

- 장점: **중복 문자열을 강제로 공유**하여 메모리/비교 비용을 줄일 수 있음(예: **반복되는 JSON 필드명**을 처리하는 Jackson의 `InternCache` 등).
- 단점: **모든 값이 풀(공유 저장소)에 남음** → **힙 압박과 GC 부하 증가** 위험.  
  또한 **네이티브(native)** 구현이며 **비싼 동기화/해시 구조 접근**이 발생할 수 있습니다.

> **권장**: 프레임워크 수준에서 **확실한 이득이 검증된 경우**에만 사용. 애플리케이션에서 **무분별한 수동 `intern()` 호출은 지양**.

---

## 5) 불변 String의 비용과 대안 — `StringBuilder` / `StringBuffer`

불변인 `String`을 **더하기/삭제** 등으로 자주 조작하면 **매번 새 객체**가 만들어져 **GC가 증가**합니다.  
이를 줄이기 위해 **가변 버퍼**인 `StringBuilder`/`StringBuffer`를 사용합니다.

- 내부에 `char[]` 버퍼를 두고 **append 시 배열에 채움**, 꽉 차면 **용량을 2배 수준으로 확장**.
- 객체 자체는 그대로 유지되므로 **중간 가비지 생성 최소화**.

```java
StringBuilder sb = new StringBuilder();      // 기본 16, 혹은 길이 지정 가능
sb.append("Hello");
sb.append(' ');
sb.append("World");
String result = sb.toString();
```

### 성능/스레드 비교

| 항목 | StringBuilder | StringBuffer |
|---|---|---|
| 스레드 안전성 | ❌ 비동기(싱글스레드용) | ✅ `synchronized`(멀티스레드 안전) |
| 성능 | ✅ 빠름 | ❌ 상대적으로 느림 |
| 내부 구조 | `char[]` + append/insert | 동일하되 메서드에 동기화 |
| 사용 권장 | 대부분의 일반 코드 | **동일 인스턴스를 여러 스레드가 동시에 조작**할 때만 |
