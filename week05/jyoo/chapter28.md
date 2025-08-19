# Ch.28 다른 서버로 데이터를 보내려면 어떻게 하면 되나요?

---
### 네트워크 프로그래밍이란?
##### 네트워킹: 
- 장비와 장비간 데이터를 주고받는 작업
- 계층 구조:
  - 애플리케이션 레이어 
    - (HTTP, ftp, telnet, ...)
  - 트랜스포트 레이어
    - (TCP, UDP, ...)
  - 네트워크 레이어
    - (IP, ...)
  - 링크 레이어
    - (device, driver, ...)
  - 실제로는 OSI 7 layers 로 더 복잡한 레이어가 존재한다고 함
#### TCP:
- 가장 대표적인 통신 방법. `연결 기반 프로토콜`이라고 불림
- 상대방이 데이터를 받았는지를 확실히 보장함
  - 데이터를 보내고 그 송신에 대한 전송의 성공 여부를 통보받음
#### UDP:
- User Datagram Protocol 의 약자
- TCP와 달리 데이터를 제대로 받았는지에 대한 보장을 할 수 없음
#### Port
- 16 비트로 구성되어 65535까지 사용가능
- 0~1023까지는 예약된 포트가 사용에 제한이 있음

### TCP 통신 (Socket / ServerSocket)
- **클라이언트**: `Socket(host, port)` → 서버에 연결, `OutputStream`으로 데이터 전송
- **서버**: `ServerSocket(port)` → `accept()`로 연결 수락, `InputStream`으로 데이터 수신
- **Timeout**: `connect()` / `setSoTimeout()` 으로 연결·읽기 시간 제한 설정 가능
- **통신 순서**:
    1. 서버 `ServerSocket` 열기
    2. 클라이언트 `Socket` 연결
    3. 데이터 송·수신 (Stream)
    4. Stream, Socket 닫기
- **양방향 통신 가능**

### UDP 통신 (DatagramSocket / DatagramPacket)

- **클래스 하나**로 송수신 모두 가능 (`send()`, `receive()`)
- **데이터 단위 = DatagramPacket** (byte[] 버퍼 기반)
- **신뢰성 없음** → 데이터 손실 가능하지만 빠름

### 웹 요청

- `URL` 클래스로 간단 요청 가능하나 기능 제한적
- 실제 개발에선 **Apache HttpComponents** 등 라이브러리 권장

