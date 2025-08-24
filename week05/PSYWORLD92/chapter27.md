# ch.27 Serializable과 NIO도 살펴 봅시다

---

## Serializable 인터페이스 구현하기
- 객체를 파일로 저장하거나 네트워크 전송 시 직렬화 필요.
- `implements Serializable` 만 선언하면 기본 필드 모두 직렬화 대상.
- `serialVersionUID` 필드를 정의하면 클래스 버전 관리에 유리.

```java
public class Person implements Serializable {
    private static final long serialVersionUID = 1L;
    String name;
    int age;
}
```

---

## 객체를 저장하자
- `ObjectOutputStream` 사용 → 객체 단위로 파일에 저장.
- 연결 예시:
  - `FileOutputStream` → `ObjectOutputStream`
- `writeObject(Object obj)` 로 저장.
- 직렬화 대상 클래스는 반드시 `Serializable` 구현 필요.

```java
ObjectOutputStream out =
    new ObjectOutputStream(new FileOutputStream("object.dat"));
out.writeObject(new Person("홍길동", 20));
out.close();
```

---

## 객체를 읽자
- `ObjectInputStream` 으로 읽기.
- `readObject()` 호출 시 Object 반환 → 캐스팅 필요.

```java
ObjectInputStream in =
    new ObjectInputStream(new FileInputStream("object.dat"));
Person p = (Person) in.readObject();
in.close();
```

---

## transient 한정자
- 직렬화 과정에서 특정 필드를 **제외**시킬 때 사용.
- 비밀번호, 민감 정보 같은 값은 `transient` 키워드로 표시.
- 직렬화 후 복원하면 해당 필드는 기본값으로 초기화됨.

```java
class Account implements Serializable {
    String id;
    transient String password;
}
```

---

## NIO Buffer 개념
- `java.nio` 패키지 → 고성능 입출력 제공.
- 핵심: **Channel** + **Buffer**
- Buffer는 고정 크기 메모리 블록.
- position, limit, capacity 개념 중요.

| 용어 | 의미 |
|------|------|
| capacity | 버퍼의 전체 크기 |
| limit    | 읽기/쓰기 제한 위치 |
| position | 현재 읽기/쓰기 위치 |

---

## Buffer 주요 메서드
- `clear()` : 버퍼 초기화 (쓰기 준비)
- `flip()` : 쓰기 → 읽기 모드 전환
- `rewind()` : 처음부터 다시 읽기
- `mark()`, `reset()` : 특정 지점 표시/복원
- `hasRemaining()` : 남은 요소 확인

---

## 예제: FileChannel + ByteBuffer
```java
FileChannel channel = new FileInputStream("data.bin").getChannel();
ByteBuffer buffer = ByteBuffer.allocate(1024);

int bytesRead = channel.read(buffer);
buffer.flip(); // 읽기 모드로 전환

while (buffer.hasRemaining()) {
    System.out.print((char) buffer.get());
}
channel.close();
```

---

## 정리
- **직렬화**: 객체를 파일/네트워크로 보내기 위한 기술 (`Serializable`, `ObjectOutputStream`, `ObjectInputStream`, `transient`)
- **NIO**: Buffer와 Channel을 통한 고성능 I/O, position/limit/capacity 개념과 flip/clear/rewind 등의 메서드 활용.
