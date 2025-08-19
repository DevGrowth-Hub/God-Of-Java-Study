# 📘 자바의 신 VOL.2 – 9장: Serializable과 NIO도 살펴 봅시다

## Serializable에 대해서 좀 살펴보자
- **직렬화(Serialization)**: 자바 객체를 바이트 스트림으로 변환하여 파일이나 네트워크를 통해 전송 가능하게 만드는 기술
- **Serializable 인터페이스**: 구현만 하면 되는 **마커 인터페이스** (구현할 메소드 없음)
- **serialVersionUID**: 클래스 버전 관리를 위한 **필수 필드**
- `static final long serialVersionUID = 1L;` 형태로 선언
- 클래스 구조 변경 시 역직렬화 호환성 체크에 사용
- **활용 목적**: 객체 상태를 영구 저장하거나 네트워크를 통한 객체 전송

---

## 객체를 저장해보자
- **ObjectOutputStream**: 객체를 파일에 저장하기 위한 스트림
- **writeObject() 메소드**: 직렬화된 객체를 실제로 저장
- **저장 과정**: FileOutputStream → ObjectOutputStream → writeObject()
- **예외 처리**: IOException 등 반드시 처리 필요
- **결과**: 바이너리 형태로 저장된 객체 파일 생성

---

## 객체를 읽어보자
- **ObjectInputStream**: 저장된 객체를 읽어오기 위한 스트림
- **readObject() 메소드**: 직렬화된 객체를 역직렬화하여 복원
- **읽기 과정**: FileInputStream → ObjectInputStream → readObject()
- **타입 캐스팅**: readObject() 반환값은 Object이므로 적절한 타입으로 캐스팅 필요
- **InvalidClassException**: serialVersionUID 불일치 시 발생하는 예외

---

## transient라는 예약어는 Serializable과 떨어질 수 없는 관계다
- **transient 키워드**: 직렬화 대상에서 **제외**하고 싶은 필드에 사용
- **사용 목적**:
- 민감한 정보(비밀번호 등) 직렬화 방지
- 임시 데이터나 계산된 값 제외
- 직렬화 불가능한 객체 포함 필드 제외
- **동작 방식**: transient 필드는 기본값(0, null, false)으로 복원됨
- **JVM 내부 처리**: 직렬화 시 해당 필드를 건너뛰고, 역직렬화 시 기본값으로 초기화

---

## 자바 NIO란?
- **NIO(New I/O)**: JDK 1.4에서 도입된 **새로운 I/O 방식**
- **기존 I/O와의 차이점**:
- **논블로킹**: 데이터 준비될 때까지 기다리지 않음
- **채널과 버퍼**: 스트림 대신 채널과 버퍼 조합 사용
- **효율적 메모리**: 직접 메모리 접근으로 성능 향상
- **주요 구성요소**: Channel, Buffer, Selector
- **장점**: 높은 성능, 확장성, 메모리 효율성
- **단점**: 복잡한 프로그래밍 모델, 학습 곡선

---

## NIO의 Buffer 클래스
- **Buffer 핵심 속성**: capacity(전체 크기), limit(읽기/쓰기 한계), position(현재 위치), mark(표시)
- **상태 관계**: `0 ≤ mark ≤ position ≤ limit ≤ capacity`
- **주요 메소드**:
- **위치 제어**: `flip()`, `rewind()`, `reset()`, `clear()`
- **상태 확인**: `remaining()`, `hasRemaining()`
- **표시 기능**: `mark()`, `reset()`
- **Buffer 종류**: ByteBuffer, CharBuffer, DoubleBuffer, FloatBuffer, IntBuffer, LongBuffer, ShortBuffer
