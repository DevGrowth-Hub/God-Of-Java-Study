# ch.26 파일에 있는 것을 읽고 쓰려면 아이오를 알아야죠

---

## 1) File 클래스는 디렉터리 관련 기능도 제공한다
- `java.io.File` 객체는 **파일/디렉터리** 모두 표현 가능.
- 생성자 예: `new File("C:/temp/test.txt")`, `new File("C:/temp")`
- 주요 정보/검사
  - `exists()`, `isFile()`, `isDirectory()`, `canRead()`, `canWrite()`
  - 경로 정보: `getName()`, `getPath()`, `getAbsolutePath()`
  - 파일 속성: `length()`, `lastModified()`
- 생성/삭제/이름변경
  - 파일 생성: `createNewFile()`
  - 디렉터리 생성: `mkdir()`, 하위 경로까지: `mkdirs()`
  - 삭제: `delete()`
  - 이름/위치 변경: `renameTo(new File(...))`

```java
File dir = new File("sample");
if (!dir.exists()) dir.mkdirs();
File f = new File(dir, "memo.txt");
if (!f.exists()) f.createNewFile();
System.out.println(f.getAbsolutePath());
```

---

## 2) 디렉터리의 파일 목록을 가져오는 list 계열 메서드
- `String[] list()` : 이름만 배열로 반환
- `File[] listFiles()` : `File` 객체 배열로 반환
- **필터링 버전**
  - `String[] list(FilenameFilter filter)`
  - `File[] listFiles(FileFilter filter)`

```java
// .txt만 걸러서 이름 출력
File folder = new File("data");
String[] names = folder.list((d, name) -> name.endsWith(".txt"));
for (String n : names) System.out.println(n);

// 혹은 FileFilter 사용
File[] onlyTxt = folder.listFiles(f -> f.isFile() && f.getName().endsWith(".txt"));
```

---

## 3) 디스크 용량 정보를 확인하자
- `getTotalSpace()` : 전체 용량(Byte)
- `getFreeSpace()` : 남은 용량(Byte)
- `getUsableSpace()` : JVM이 사용 가능한 용량(Byte)

```java
File root = new File("C:/");
System.out.printf("total=%d, free=%d, usable=%d%n",
        root.getTotalSpace(), root.getFreeSpace(), root.getUsableSpace());
```

---

## 4) InputStream과 OutputStream의 기본 동작 살펴보기
- **바이트 스트림**의 최상위 추상 클래스
  - 입력: `InputStream` / 출력: `OutputStream`
- 공통 아이디어: **읽기(read)** 와 **쓰기(write)** 는 보통 **버퍼**를 사용해 반복 처리.
- EOF(파일 끝) : `read(...)`가 **-1**을 반환하면 종료.

### InputStream 핵심 메서드
- `int read()` / `int read(byte[] b)` / `int read(byte[], int off, int len)`
- `long skip(long n)` : 건너뛰기
- `int available()` : 읽을 수 있는 바이트 수(추정)
- `void close()`
- `mark(int readlimit)` / `reset()` / `boolean markSupported()`

### OutputStream 핵심 메서드(요지)
- `void write(int b)` / `void write(byte[] b)` / `void write(byte[], int off, int len)`
- `void flush()` / `void close()`

```java
// 바이트 스트림으로 파일 복사
try (InputStream in = new FileInputStream("a.bin");
     OutputStream out = new FileOutputStream("b.bin")) {
    byte[] buf = new byte[8192];
    int n;
    while ((n = in.read(buf)) != -1) {
        out.write(buf, 0, n);
    }
    out.flush();
}
```

---

## 5) Reader / Writer (문자 스트림)
- 문자 단위 I/O를 위한 최상위 추상 클래스.
- **Reader** 주요 메서드: `read()`, `read(char[])`, `read(char[], off, len)`, `skip`, `ready`, `mark/reset`, `close`
- **Writer** 주요 메서드: `write(int/char[]/String, off, len)`, `append(...)`, `flush`, `close`
- 문자 인코딩 처리를 위해 바이트 스트림과 **브리지**: `InputStreamReader`, `OutputStreamWriter`

```java
// UTF-8 텍스트 파일 복사
try (Reader r = new BufferedReader(new InputStreamReader(
         new FileInputStream("a.txt"), StandardCharsets.UTF_8));
     Writer w = new BufferedWriter(new OutputStreamWriter(
         new FileOutputStream("b.txt"), StandardCharsets.UTF_8))) {
    char[] buf = new char[4096];
    int n;
    while ((n = r.read(buf)) != -1) {
        w.write(buf, 0, n);
    }
    w.flush();
}
```

---

## 6) 보조(필터) 스트림을 익히자
> **데코레이터 형태**로 기본 스트림을 감싸 **기능을 확장**한다.

- **BufferedInputStream/BufferedOutputStream** : 버퍼링으로 성능 향상
- **DataInputStream/DataOutputStream** : 기본형/문자열을 포맷에 맞게 읽고 쓰기
- **ObjectInputStream/ObjectOutputStream** : 객체 직렬화 입출력
- **PrintStream/PrintWriter** : 편의 출력(줄바꿈, 포맷팅 등)

```java
// Buffered 스트림 연결 예
try (InputStream in = new BufferedInputStream(new FileInputStream("big.dat"));
     OutputStream out = new BufferedOutputStream(new FileOutputStream("copy.dat"))) {
    byte[] buf = new byte[8192];
    for (int n; (n = in.read(buf)) != -1; ) out.write(buf, 0, n);
}
```

**주의 사항**
- 가장 바깥의 보조 스트림만 `close()` 하면 **안쪽 스트림도 함께 닫힘**.
- 필요 시 `flush()`로 버퍼 비우기.
- 예외/자원 관리는 **try-with-resources** 사용.

---

## 7) 파일을 읽을 때의 전형적인 패턴(EOF)
- `read()`/`read(buffer)`가 **-1**을 반환할 때까지 반복.
- `while ((n = in.read(buf)) != -1) { ... }` (바이트)
- `while ((n = r.read(cbuf)) != -1) { ... }` (문자)

---

## 8) Files(NIO) 간단 사용
- `java.nio.file.Files`는 파일 생성/복사/이동/삭제를 **편리한 정적 메서드**로 제공.
- (예) `Files.copy(Path src, Path dst)`, `Files.move(...)`, `Files.deleteIfExists(...)`

```java
Path src = Path.of("a.txt");
Path dst = Path.of("b.txt");
Files.copy(src, dst, StandardCopyOption.REPLACE_EXISTING);
```
## 9) 표준 입출력
- 자바에서 표준 입출력은 `System.in`, `System.out`, `System.err`로 제공된다.  
- `System.in` : 키보드 입력을 처리하는 **InputStream**  
- `System.out` : 콘솔에 출력하는 **PrintStream**  
- `System.err` : 에러 출력 스트림

---

## 10) InputStreamReader / BufferedReader
- `System.in`은 **바이트 단위 입력 스트림**이므로, 문자를 입력받기 위해서는 변환이 필요하다.
- `InputStreamReader` : 바이트 → 문자 변환  
- `BufferedReader` : 문자 스트림을 버퍼링하여 효율적으로 읽기

```java
BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
String data = br.readLine();
```

---

## 11) Scanner 클래스
- 키보드 입력을 손쉽게 처리할 수 있는 클래스  
- `nextLine()`, `nextInt()`, `nextDouble()` 등 다양한 타입 입력 지원

```java
Scanner scanner = new Scanner(System.in);
int num = scanner.nextInt();
```

---

## 12) 예외 처리와 자원 해제
- `BufferedReader`, `Scanner` 같은 입출력 객체는 사용 후 반드시 닫아야 함  
- `try-catch-finally` 또는 `try-with-resources` 사용  

```java
try (Scanner scanner = new Scanner(System.in)) {
    int num = scanner.nextInt();
    System.out.println("입력값: " + num);
}
```
