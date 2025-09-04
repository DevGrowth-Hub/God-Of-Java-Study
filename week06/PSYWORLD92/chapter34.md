# ch.34 Java 10에서 변경된 것들

Java 10은 상대적으로 소규모 업데이트였지만, 개발 생산성과 성능 향상에 도움이 되는 기능들이 추가되었습니다.

---
## 1. 지역변수 타입 추론 (`var`)
- `var` 키워드를 사용해 지역변수 타입을 **자동 추론**할 수 있습니다.
- 코드 가독성이 높아지고, 타입 선언 중복을 줄여줍니다.
- 예시:
  ```java
  var list = new ArrayList<String>();  // 컴파일러가 ArrayList<String>으로 추론
  var number = 10; // int로 추론
  ```

---

## 2. 수정 불가능한 Collection
- `List.copyOf()`, `Set.copyOf()`, `Map.copyOf()` 메서드 제공.
- 불변 컬렉션을 쉽게 생성할 수 있습니다.
- 예시:
  ```java
  List<String> list = List.of("A", "B", "C");
  List<String> copy = List.copyOf(list);
  ```

---

## 3. Optional에 `orElseThrow()` 추가
- 값이 없을 경우 예외를 던지는 메서드 추가.
- `NoSuchElementException` 발생.
- 예시:
  ```java
  Optional<String> opt = Optional.empty();
  opt.orElseThrow();  // NoSuchElementException 발생
  ```

---

## 4. 기능 개선

### 1) G1 GC 개선
- **병렬 Full GC** 지원 → GC 성능 향상.

### 2) AppCDS (Application Class-Data Sharing)
- 여러 Java 프로세스 간 클래스 메타데이터를 공유하여 **시작 속도 개선, 메모리 사용 절감**.

### 3) Graal 컴파일러 (실험적)
- 차세대 JIT 컴파일러, 고성능을 목표로 개발.
- `-XX:+UnlockExperimentalVMOptions -XX:+UseJVMCICompiler` 옵션으로 사용 가능.

---

# 요약
- **var 키워드**: 지역변수 타입 추론 기능 제공
- **Immutable Collection**: 불변 컬렉션 생성 메서드 추가
- **Optional.orElseThrow()**: 값이 없을 경우 예외 발생
- **G1 GC 개선**: 병렬 Full GC 도입
- **AppCDS**: 클래스 데이터 공유로 성능 개선
- **Graal 컴파일러**: 실험적 JIT 컴파일러


---

# 18장: Java 11에서 변경된 것들

Java 11은 LTS(Long Term Support) 버전으로, 많은 기능이 추가되고 일부는 제거되었으며, Oracle JDK가 유료화된 중요한 전환점입니다.

---

## 변경된 것들 요약 (표)

| 구분        | 주요 변경/추가 기능 |
|-------------|---------------------|
| **추가된 기능** | String 클래스 메소드 추가, File 클래스 메소드 추가, Collection.toArray(), Predicate.not(), Lambda에서 var 사용, 새로운 HTTP Client, Nest-based 접근 제어 |
| **보완/제거** | Dynamic Class-File Constants, AArch64 Intrinsics 개선, No-Op GC(Epsilon GC), Java EE/Corba 제거, Nashorn JS 엔진 및 Pack200 툴 deprecated |
| **분리된 기능/툴** | Java Flight Recorder, Java Mission Control, JavaFX 모듈 JDK에서 분리 |

---

## 1. 추가된 기능

### 1) String 클래스 메소드 추가
- `isBlank()`, `lines()`, `repeat()`, `strip()`, `stripLeading()`, `stripTrailing()` 등.

### 2) File 클래스 메소드 추가
- `Files.readString(Path)`, `Files.writeString(Path, String)` 등 간편 파일 입출력.

### 3) Collection 인터페이스 개선
- `toArray(IntFunction<T[]>)` 메서드 추가.

### 4) Predicate.not() 추가
- 조건식을 반전하는 메소드.
- 예시:
  ```java
  list.stream().filter(Predicate.not(String::isEmpty));
  ```

### 5) Lambda에서 `var` 사용 가능
- 람다 파라미터에 `var`를 명시적으로 선언할 수 있음.
- 예시:
  ```java
  (var a, var b) -> a + b;
  ```

### 6) 새로운 HTTP Client (정식 포함)
- Java 9/10에서 실험적이던 HTTP/2 클라이언트가 정식 채택됨.
- 비동기 처리, WebSocket 지원.

### 7) Nestmate 접근 제어
- 내부 클래스와 외부 클래스 간 접근 제어를 단순화.

---

## 2. 보완 및 제거된 기능
- **Dynamic Class-File Constants** 지원.
- **AArch64 Intrinsics 개선** → ARM64 아키텍처 성능 향상.
- **No-Op GC (Epsilon GC)** 추가 → GC 없는 환경 테스트 가능.
- **Java EE, CORBA 제거** (더 이상 JDK에 포함되지 않음).
- **Nashorn JavaScript 엔진**, **Pack200 툴** → deprecated.

---

## 3. JDK에서 분리된 기능/툴
- **Java Flight Recorder (JFR)** → 별도 제공.
- **Java Mission Control (JMC)** → 별도 제공.
- **JavaFX 모듈** → JDK에서 제거, 별도 설치 필요.

---

## 4. 추가 Q&A

- **컴파일 없이 Java 파일 실행하는 방법**
  ```bash
  java Hello.java
  ```

- **람다에서 var 사용 예시**
  ```java
  BiFunction<Integer, Integer, Integer> add = (var a, var b) -> a + b;
  ```

---

# 요약

- **Java 10**: var 타입 추론, Immutable Collection, Optional.orElseThrow, G1 GC 개선, AppCDS, Graal 컴파일러  
- **Java 11**: String/File/Collection/Predicate 개선, HTTP Client 정식 추가, Lambda에서 var 사용 가능, Nestmate 접근 제어, JDK 일부 기능 제거 및 모듈 분리, Oracle JDK 유료화
