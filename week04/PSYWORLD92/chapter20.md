# ch.20 가장 많이 쓰는 패키지는 자바랭

---

## 1. System 클래스 개요
- Java에서 제공하는 **java.lang.System** 클래스는 표준 입출력, 시스템 속성, 환경 변수, GC 호출, JVM 종료, 시간 조회 등의 기능을 제공
- 모든 메서드와 필드는 **static**으로 선언되어 있어 객체 생성 없이 사용 가능

---

## 2. 주요 메서드 분류 및 기능

### (1) 기본 출력 관련
| 메서드 | 반환 타입 | 설명 |
|--------|----------|------|
| `System.out.println()` | void | 줄바꿈 포함 출력 |
| `System.out.print()` | void | 줄바꿈 없이 출력 |
| `System.out.printf()` | void | 서식 지정하여 출력 |

---

### (2) 시간 관련
| 메서드 | 반환 타입 | 설명 |
|--------|----------|------|
| `currentTimeMillis()` | long | 현재 시간(밀리초) 반환 |
| `nanoTime()` | long | 현재 시간(나노초) 반환 |

> `currentTimeMillis()`는 주로 시간차 계산, `nanoTime()`은 정확한 성능 측정 시 사용

---

### (3) GC(가비지 컬렉션) 호출
| 메서드 | 반환 타입 | 설명 |
|--------|----------|------|
| `gc()` | void | JVM에 GC 수행 요청 |

> 참고: GC는 요청이 즉시 실행될 것을 보장하지 않음

---

### (4) 시스템 속성(Properties) 관리
| 메서드 | 반환 타입 | 설명 |
|--------|----------|------|
| `getProperty(String key)` | String | key에 해당하는 속성 값 반환 |
| `setProperty(String key, String value)` | String | 속성 값 설정 |
| `getProperties()` | Properties | 모든 시스템 속성 반환 |
| `clearProperty(String key)` | String | 속성 제거 |

**주요 속성 예시**
- `java.version` : 자바 버전
- `java.home` : JDK/JRE 경로
- `os.name` : OS 이름
- `user.name` : 사용자 이름
- `user.home` : 사용자 홈 디렉토리

---

### (5) 환경 변수(Environment Variables) 조회
| 메서드 | 반환 타입 | 설명 |
|--------|----------|------|
| `getenv(String name)` | String | 특정 환경 변수 값 반환 |
| `getenv()` | Map | 모든 환경 변수 반환 |

---

### (6) JVM 종료
| 메서드 | 반환 타입 | 설명 |
|--------|----------|------|
| `exit(int status)` | void | JVM 종료, 0이면 정상 종료 |

---

## 3. Wrapper 클래스 기본 타입 변환
자바는 기본 타입을 객체로 다룰 수 있도록 **Wrapper 클래스** 제공

| 기본 타입 | Wrapper 클래스 |
|----------|---------------|
| byte | Byte |
| short | Short |
| int | Integer |
| long | Long |
| float | Float |
| double | Double |
| char | Character |
| boolean | Boolean |

### 주요 기능
- `parseXXX(String s)` : 문자열을 기본 타입으로 변환
- `valueOf(String s)` : 문자열을 Wrapper 객체로 변환
- `XXX.toString()` : 기본 타입 값을 문자열로 변환

---

## 4. Character 클래스
- 문자 관련 유틸리티 제공
- 주요 메서드
  - `isDigit(char ch)` : 숫자인지 확인
  - `isLetter(char ch)` : 문자인지 확인
  - `isWhitespace(char ch)` : 공백 문자 확인

---

✅ **정리**
- `System` 클래스는 프로그램 실행 환경과 관련된 핵심 기능을 제공
- 표준 출력, 시간 측정, GC 호출, 속성/환경 변수 조회, JVM 종료 등 다방면에서 활용 가능
- Wrapper 클래스는 기본 타입과 객체 타입 간 변환에 유용
