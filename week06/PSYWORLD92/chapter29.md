
# 29장: Java 7에서 달라진 것들

Java 7은 개발자가 더 간단하고 안전하게 코드를 작성할 수 있도록 다양한 기능을 도입했습니다. 주요 변경 사항은 아래와 같습니다.

---

## 1. 숫자 표기 방법 보완
- 큰 숫자를 읽기 쉽게 하기 위해 **언더스코어(`_`)** 를 사용할 수 있습니다.
- 예시:  
  ```java
  int million = 1_000_000;
  double pi = 3.14_159_265;
  ```
- 실행 시에는 무시되며, 가독성 향상 목적입니다.

---

## 2. switch문에서 String 사용 가능
- Java 7부터는 `switch` 문에서 문자열을 직접 사용할 수 있습니다.
- 예시:  
  ```java
  String day = "MON";
  switch(day) {
      case "MON":
          System.out.println("월요일");
          break;
      case "TUE":
          System.out.println("화요일");
          break;
  }
  ```
- 내부적으로 `hashCode()`를 사용해 분기합니다.

---

## 3. 제네릭 다이아몬드(Diamond) 연산자
- 제네릭 객체 생성 시 타입을 다시 명시하지 않아도 됩니다.
- 예시:  
  ```java
  Map<String, List<Integer>> map = new HashMap<>();
  ```
- 이전 버전처럼 `new HashMap<String, List<Integer>>()` 를 쓰지 않아도 됩니다.

---

## 4. 예외 처리 (멀티캐치)
- 여러 예외를 한 번에 처리할 수 있습니다.
- 예시:  
  ```java
  try {
      // 실행 코드
  } catch(IOException | SQLException e) {
      e.printStackTrace();
  }
  ```
- 코드 중복을 줄이고 간결하게 작성할 수 있습니다.

---
## 5. Non-reifiable varargs 타입 경고 개선
- 제네릭과 가변 인자를 함께 사용할 때 발생하는 **안전하지 않은 경고**를 억제할 수 있습니다.
- `@SafeVarargs` 어노테이션이 도입되어, 메서드 작성자가 안전성을 보장할 수 있습니다.

---

## 6. 자동 리소스 해제 (try-with-resources)
- `try-with-resources` 문법을 사용하면 `close()`를 명시적으로 호출하지 않아도 됩니다.
- `AutoCloseable` 또는 `Closeable` 인터페이스를 구현한 객체는 자동으로 닫힙니다.
- 예시:  
  ```java
  try (Scanner scanner = new Scanner(new File("data.txt"))) {
      while (scanner.hasNext()) {
          System.out.println(scanner.nextLine());
      }
  }
  ```
- 명시적으로 `finally`에서 닫지 않아도 리소스 누수를 방지합니다.

### 자동으로 닫히는 대표 클래스들
- `Closeable` 구현체들  
  - `FileInputStream`, `FileOutputStream`  
  - `ObjectInputStream`, `ObjectOutputStream`  
  - `Scanner`, `BufferedReader`, `BufferedWriter`
- `Connection`, `ResultSet`, `Statement` (JDBC 관련)
- `FileLock`, `XMLEncoder`, `XMLDecoder` 등

---

# 요약
- 숫자 리터럴에 언더스코어 사용 가능
- switch문에서 String 사용 가능
- 제네릭 다이아몬드 연산자 지원
- 멀티 캐치로 예외 처리 단순화
- @SafeVarargs 추가 (가변 인자 제네릭 경고 억제)
- try-with-resources로 리소스 자동 해제 가능
