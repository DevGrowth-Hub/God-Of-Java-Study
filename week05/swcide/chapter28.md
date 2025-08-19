# 📘 자바의 신 VOL.2 – 10장: 다른 서버로 데이터를 보내려면 어떻게 하면 되나요?

## 네트워크 프로그래밍이란?
- **네트워킹**: **장비 간 데이터 송수신**을 다룸
- **계층 구조**: **애플리케이션(HTTP/FTP)** → **트랜스포트(TCP/UDP)** → **네트워크(IP)** → **링크(드라이버)**
- 자바에서는 주로 **TCP/UDP**를 사용
- **포트**: 16비트(0–65535). **0–1023**은 잘 알려진 포트(예약) → 일반 앱은 피함

---

## 소켓 통신을 하기 위해서 알아야 하는 Socket 클래스
- **Socket**: 원격 장비와의 연결 상태 + **입출력 스트림** 제공
- `connect()`에 **timeout** 지정 가능
- `getInputStream()/getOutputStream()`으로 바이트 송수신
- 연결/주소/포트 조회, `close()`로 자원 반납
- **ServerSocket**: 서버 측에서 **클라이언트 접속을 수락**
- `new ServerSocket(port)`(내부적으로 `bind()`)
- `accept()`가 반환하는 **Socket**으로 통신
- `setSoTimeout()`으로 **읽기 대기 시간** 설정
- `close()`로 리스너 종료

---

## 간단하게 소켓 통신을 해보자
1. **서버**: `new ServerSocket(포트)` → `accept()`로 연결 수락
2. **클라이언트**: `new Socket(IP, 포트)`(내부 `connect()`)
3. **클라이언트 → 서버**: `OutputStream`으로 전송
4. **서버 → 클라이언트**: `InputStream`으로 수신(필요 시 응답도 전송)
5. **스트림** `close()` → **소켓** `close()` 양쪽 모두 호출
- TCP는 **연결 기반/양방향/신뢰성 보장**

---

## UDP 통신을 위해서 알아야 하는 Datagram 관련 클래스
- **DatagramSocket**: **하나의 소켓으로 송·수신**
- `receive()`로 대기 및 수신, `send()`로 전송
- **DatagramPacket**: **데이터 컨테이너**
- 생성 시 **버퍼(byte[])**, **길이**, (전송 시) **주소/포트** 지정
- `getData()/getLength()`로 수신 데이터 확인
- UDP는 **비연결/비신뢰**(유실 가능) → 빠름, 단순, **유실 허용 서비스에 적합**

---

## 간단하게 UDP 통신을 해보자
1. **수신 측**: `new DatagramSocket(포트)` → 빈 버퍼로 `DatagramPacket` 준비 → `receive()`
2. **송신 측**: `new DatagramSocket()`(필요 시 바인드) → 데이터가 담긴 `DatagramPacket(주소, 포트)` → `send()`
3. 처리 후 소켓 `close()`

---

## 자바에서 웹 페이지 요청을 하려면 어떻게 해야 하지?
- 표준 `URL`/`URLConnection`으로 **간단 요청** 가능(설정 폭 제한)
- 실무에선 **Apache HttpComponents** 등 **전용 라이브러리** 사용이 일반적(헤더/타임아웃/리다이렉트 등 세부 제어 용이)

---
