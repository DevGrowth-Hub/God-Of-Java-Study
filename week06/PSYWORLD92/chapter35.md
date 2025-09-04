# ch.35 Java 12 ~ 17 변경된 기능 정리

## Java 12 주요 변경점
| JEP | 기능 요약 |
|-----|-----------|
| 189 | Shenandoah GC (저지연 Garbage Collector, 실험적) |
| 230 | Microbenchmark Suite 추가 |
| 325 | Switch Expressions (미리보기 기능) |
| 334 | JVM Constants API |
| 340 | One AArch64 Port, Not Two |
| 344 | Abortable Mixed Collections for G1 |
| 346 | Promptly Return Unused Committed Memory (G1) |

---

## Java 13 주요 변경점
| JEP | 기능 요약 |
|-----|-----------|
| 350 | Dynamic CDS Archives |
| 351 | ZGC: Uncommit Unused Memory |
| 353 | Reimplement the Legacy Socket API |
| 354 | Switch Expressions (미리보기, 개선) |
| 355 | Text Blocks (미리보기) |

---

## Java 14 주요 변경점
| JEP | 기능 요약 |
|-----|-----------|
| 305 | instanceof 패턴 매칭 (미리보기) |
| 343 | Packaging Tool (실험적) |
| 345 | G1 NUMA-Aware Memory Allocation |
| 349 | JFR Event Streaming |
| 352 | Non-Volatile Mapped Byte Buffers |
| 358 | Helpful NullPointerExceptions |
| 359 | Records (미리보기) |
| 361 | Switch Expressions (정식 기능) |
| 364 | ZGC on macOS |
| 365 | ZGC on Windows |
| 366 | Deprecate ParallelScavenge + SerialOld GC |
| 367 | Remove CMS GC |
| 368 | Text Blocks (2차 미리보기) |
| 370 | Foreign-Memory Access API (Incubator) |

---

## Java 15 주요 변경점
| JEP | 기능 요약 |
|-----|-----------|
| 339 | EdDSA (Edwards-Curve Digital Signature Algorithm) |
| 360 | Sealed Classes (미리보기) |
| 371 | Hidden Classes |
| 372 | Remove Nashorn |
| 373 | Reimplement Legacy DatagramSocket API |
| 374 | Disable Biased Locking |
| 375 | Pattern Matching for instanceof (2차 미리보기) |
| 377 | ZGC: Concurrent Thread-Stack Processing |
| 378 | Text Blocks (정식 기능) |
| 379 | Shenandoah GC (정식 포함) |
| 381 | Remove Solaris / SPARC port |
| 383 | Foreign-Memory Access API (2차 Incubator) |
| 384 | Records (2차 미리보기) |
| 385 | Deprecate RMI Activation |

---

## Java 16 주요 변경점
| JEP | 기능 요약 |
|-----|-----------|
| 338 | Vector API (Incubator) |
| 347 | Enable C++14 language features |
| 357 | Migrate from Mercurial to Git |
| 369 | Migrate to GitHub |
| 376 | ZGC: Concurrent Thread-Stack Processing |
| 387 | Elastic Metaspace |
| 394 | instanceof 패턴 매칭 (정식) |
| 395 | Records (정식) |
| 397 | Sealed Classes (2차 미리보기) |
| 398 | Deprecate Security Manager |
| 396 | Strongly Encapsulate JDK Internals |

---

## Java 17 주요 변경점 (LTS)
| JEP | 기능 요약 |
|-----|-----------|
| 306 | Restore Always-Strict Floating-Point Semantics |
| 356 | Enhanced Pseudo-Random Number Generators |
| 382 | New macOS Rendering Pipeline (Metal) |
| 389 | Foreign Function & Memory API (Incubator) |
| 411 | Deprecate Security Manager |
| 406 | Switch 패턴 매칭 (미리보기) |
| 409 | Sealed Classes (정식) |
| 410 | Remove RMI Activation |
| 411 | Deprecate Security Manager for Removal |

---

# 주요 기능 상세 설명

## 1. Switch-case 확장 (Switch Expressions)
- Java 12에서 미리보기 기능으로 처음 도입, Java 14에서 정식 포함.
- 전통적인 `switch` 문을 **표현식(expression)** 으로 사용할 수 있음.
- `yield` 키워드를 사용하여 값을 반환 가능.

### 예시 (switch expression)
```java
int day = 3;
String result = switch(day) {
    case 1, 2, 3 -> "초반";
    case 4, 5 -> "중반";
    default -> {
        yield "후반";
    }
};
System.out.println(result);
```

---

## 2. Text Block (멀티라인 문자열)
- Java 13에서 미리보기 도입, Java 15에서 정식 기능.
- 따옴표 세 개(`"""`)를 연달아 사용하면 **멀티라인 문자열** 작성 가능.
- 가독성이 높고, 이스케이프 문자를 최소화할 수 있음.

### 예시
```java
String html = """
    <html>
        <body>
            <p>Hello, World</p>
        </body>
    </html>
    """;
```

---

## 3. Record 클래스
- Java 14에서 미리보기, Java 16에서 정식 도입.
- 불변(immutable) 데이터 객체를 간단하게 정의할 수 있음.
- `equals()`, `hashCode()`, `toString()` 자동 생성.
- DTO, VO와 같은 단순 데이터 전달 객체에 적합.

### 예시
```java
public record Person(String name, int age) {}

Person p = new Person("Kim", 30);
System.out.println(p.name()); // "Kim"
System.out.println(p);        // Person[name=Kim, age=30]
```

---

## 4. Sealed 클래스
- Java 15에서 미리보기, Java 17에서 정식 기능.
- 특정 클래스나 인터페이스의 상속을 **제한**할 수 있음.
- `permits` 키워드로 상속 가능한 하위 클래스를 명시.

### 예시
```java
public sealed class Shape permits Circle, Rectangle {}

public final class Circle extends Shape {}
public final class Rectangle extends Shape {}
```

- 목적: **클래스 계층을 더 안전하게 제어**.
- 장점: 유지보수 용이성, 의도하지 않은 상속 방지.

---

# 요약
- **Switch Expressions**: switch문이 값 반환 가능, 더 간결하고 안전함.
- **Text Block**: """로 멀티라인 문자열 작성 가능.
- **Record 클래스**: 불변 데이터 객체를 간결히 정의.
- **Sealed 클래스**: 클래스 계층 구조를 제한하여 안정성 확보.
