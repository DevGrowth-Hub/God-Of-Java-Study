# ch.28 다른 서버로 데이터를 보내려면 어떻게 하면 되나요?

## 1. 네트워크 프로그래밍 개요
- 네트워크 프로그래밍은 `Socket`을 기반으로 클라이언트-서버 간 통신을 구현.  
- **TCP(연결 지향)**, **UDP(비연결 지향)** 두 가지 방식 제공.  
- URL, IP, 포트 번호 등을 통해 네트워크 상의 특정 자원 접근.

---

## 2. 소켓 통신 이해하기
- `Socket` : 두 컴퓨터 간 통신을 위한 연결 지점.  
- Java에서는 `Socket`, `ServerSocket`, `DatagramSocket` 클래스를 통해 구현.  
- **TCP 소켓 통신**은 연결을 보장하는 신뢰성 있는 전송.  
- **UDP 소켓 통신**은 빠르지만 신뢰성이 보장되지 않음.

---

## 3. TCP 기반 소켓 통신

### (1) TCP 서버
- `ServerSocket server = new ServerSocket(포트번호);` 로 서버 소켓 생성.
- `server.accept()` : 클라이언트 연결 수락.
- Input/OutputStream을 사용해 데이터 송수신.
- 서버 측 주요 메서드:
  - `accept()` : 연결 요청 수락.
  - `getInputStream() / getOutputStream()` : 입출력 스트림 생성.

### (2) TCP 클라이언트
- `Socket socket = new Socket("localhost", 포트번호);`  
- 서버와 연결 후 스트림을 통해 데이터 송수신.

### (3) 예제 코드
```java
ServerSocket server = new ServerSocket(9999);
Socket socket = server.accept();
OutputStream out = socket.getOutputStream();
InputStream in = socket.getInputStream();
```

---

## 4. UDP 기반 소켓 통신

### (1) UDP 서버
- `DatagramSocket server = new DatagramSocket(포트번호);`
- 데이터 수신 시 `DatagramPacket` 사용.
- 비연결형이므로 `accept()` 과정 없음.

### (2) UDP 클라이언트
- `DatagramSocket client = new DatagramSocket();`
- 서버 IP, 포트 번호와 함께 `DatagramPacket`을 생성 후 `send()` 호출.

### (3) 주요 메서드
| 클래스/메서드 | 설명 |
|---------------|------|
| `send(DatagramPacket p)` | 데이터 전송 |
| `receive(DatagramPacket p)` | 데이터 수신 |
| `DatagramPacket(byte[] buf, int len)` | 패킷 생성 |
| `DatagramPacket(byte[] buf, int len, InetAddress addr, int port)` | 목적지 포함 패킷 생성 |

---

## 5. TCP vs UDP 비교
| 구분 | TCP | UDP |
|------|-----|-----|
| 연결방식 | 연결 지향 | 비연결 |
| 신뢰성 | 높음 (데이터 손실 거의 없음) | 낮음 (패킷 손실 가능) |
| 속도 | 상대적으로 느림 | 빠름 |
| 사용 예시 | 웹, 메신저, 파일 전송 | 동영상 스트리밍, 온라인 게임 |

---

## 6. 예외 처리
- `IOException` : 입출력 오류.
- `SocketException` : 소켓 관련 오류.
- `UnknownHostException` : 잘못된 주소 지정.
- `BindException` : 포트 충돌 발생 시.
