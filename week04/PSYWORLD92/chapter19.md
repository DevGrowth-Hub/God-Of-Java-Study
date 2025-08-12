## ch.19 이쯤에서 자바의 역사와 JVM에 대해서 알아보자

---

## 1. 자바 언어의 주요 특징

자바는 **"Write Once, Run Anywhere(WORA)"**라는 철학을 기반으로 설계되었습니다.  
다음과 같은 핵심 특징이 있습니다.

1. **단순(Simple)하고 객체지향(Object-Oriented)이며 친숙(Familiar)**
   - C/C++ 기반 문법으로 기존 개발자들이 쉽게 적응 가능
   - 포인터 제거, 메모리 관리 자동화 등으로 단순화

2. **견고(Robust)하며 보안(Security)이 뛰어남**
   - 컴파일 시 엄격한 타입 체크
   - JVM이 제공하는 보안 매커니즘(클래스 로더, 바이트코드 검증 등)
   - 메모리 접근 제한, 버퍼 오버플로우 방지

3. **아키텍처 중립적(Architecture Neutral)이며 이식성(Portable) 높음**
   - OS와 하드웨어에 독립적
   - JVM만 설치되어 있으면 어디서든 실행 가능

4. **높은 성능(High Performance)**
   - JIT(Just-In-Time) 컴파일러로 실행 속도 향상

5. **인터프리터 언어이며, 멀티스레드(Thread) 지원, 동적(Dynamic) 특성 보유**
   - 바이트코드를 해석/실행하는 인터프리터 방식
   - 멀티스레드 라이브러리 내장
   - 런타임 시 동적 로딩(Dynamic Loading) 가능

---

## 2. JIT(Just-In-Time) 컴파일러

- **정의**: 프로그램을 실행할 때 바이트코드를 기계어로 변환하는 기술
- **특징**
  - 동적 변환(Dynamic Translation)
  - 실행 시점에서 컴파일하여 성능 향상
  - 초기 실행은 느리지만 반복 실행 시 속도 개선
- **대표 사용 언어**: Java, .NET(C#)

**동작 방식 예시**
```
[소스 코드] → [컴파일러] → [바이트코드] → [JVM + JIT] → [기계어 실행]
```

---

## 3. HOTSPOT 컴파일러

- 자바의 대표적인 JIT 컴파일러 구현체
- 두 가지 모드 제공
  - **Client Compiler**: 빠른 시작, GUI 프로그램 등에 적합 → `-client` 옵션
  - **Server Compiler**: 실행 속도 최적화, 서버 애플리케이션에 적합 → `-server` 옵션

---

## 4. JVM(Java Virtual Machine)

- 자바 프로그램이 실행되는 가상의 컴퓨터
- 바이트코드를 해석하고, 메모리를 관리하며, 보안 기능 제공

**JVM 주요 구성 요소**
- **Class Loader**: 클래스 파일 로딩
- **Execution Engine**: 바이트코드 해석 및 실행(JIT 포함)
- **Garbage Collector(GC)**: 사용하지 않는 객체 메모리 해제
- **Runtime Data Area**: 프로그램 실행 시 메모리 영역
  - **Heap**: 객체 저장소
    - **Young Generation**: 새로 생성된 객체 저장
    - **Old Generation**: 오래 살아남은 객체 저장
    - **Eden**: Young 영역의 세부 영역
  - **Method Area**: 클래스 메타데이터, static 변수 저장
  - **Stack**: 메서드 호출 정보 저장

---

## 5. GC(Garbage Collector)

- 역할: JVM 내에서 사용하지 않는 객체를 자동으로 메모리에서 제거
- **영(Young) GC** vs **풀(Full) GC**
  - Young GC: Young Generation 영역에서만 수행 → 빠름
  - Full GC: 전체 Heap 영역 검사 → 느림
- 동작 방식:
  1. 더 이상 참조되지 않는 객체 탐색
  2. 해당 객체 메모리 해제
  3. 힙 메모리 재활용

---

✅ **정리**
- 자바는 객체지향, 보안성, 이식성, 성능을 모두 고려한 언어
- JVM과 JIT, GC는 자바 성능과 안정성을 책임지는 핵심 요소
