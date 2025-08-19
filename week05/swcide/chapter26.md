# 📘 자바의 신 VOL.2 – 8장: 파일에 있는 것을 읽고 쓰려면 아이오를 알아야죠

## I/O는…
- I/O는 **Input과 Output**의 약자로 입출력을 통칭하는 용어
- 파일 읽기/저장, 다른 서버나 디바이스로 데이터 전송할 때 사용``
- **JVM 기준**: 읽을 때는 Input, 파일로 쓰거나 외부로 전송할 때는 Output
- **초기**: java.io 패키지의 **스트림 기반** 클래스 제공
    - 바이트 기반: `InputStream`(읽기), `OutputStream`(쓰기)
    - 문자 기반: `Reader`(읽기), `Writer`(쓰기)
    - 스트림 = "개천, 줄기", 끊기지 않고 연속적인 데이터 의미
- **JDK 1.4**: **NIO(New I/O)** 추가 - Buffer와 Channel 기반으로 더 빠른 처리
- **Java 7**: **NIO2** 추가 - 기존 단점 보완, 더 효율적인 파일 처리``

---

## 자바의 File과 Files 클래스
- **File 클래스** (java.io.File): 파일 및 경로 정보를 통제하기 위한 클래스
    - **단점**: 정체가 불분명, 심볼릭 링크 같은 유닉스 계열 파일 기능을 제대로 제공하지 못함
    - **생성자**: `File(String pathname)`, `File(String parent, String child)` 등
- **Files 클래스** (java.nio.file.Files) - Java 7+
    - File 클래스의 단점을 보완
    - **모든 메소드가 static**으로 선언 → 객체 생성 불필요

---

## File 클래스를 이용하여 파일의 경로와 상태를 확인해 보자
- **경로 구분자**: 운영체제마다 다름 (\ 또는 /) → **File.separator** static 변수로 OS에 맞는 구분자 제공
- 윈도우에서는 특수 문자 인식 문제로 **두 개의 역슬래시(\\)** 연달아 사용

**파일 정보 확인 메소드:**
- `boolean exists()`: 파일의 경로가 존재하는지 확인
- `boolean isFile()`: File 객체가 파일을 나타내는지 확인
- `boolean isDirectory()`: File 객체가 디렉터리를 나타내는지 확인
- `boolean isHidden()`: 숨겨진 파일인지 여부 반환
- `boolean canRead()`, `canWrite()`, `canExecute()`: 읽기/쓰기/실행 권한 확인 (Java 6+)
- `long lastModified()`: 파일이나 경로가 언제 수정되었는지 long 타입 현재 시간 반환

---

## File 클래스를 이용하여 파일을 처리하자
**디렉터리 생성:**
- `boolean mkdir()`: 존재하지 않는 **하나의 디렉터리** 생성
- `boolean mkdirs()`: 존재하지 않는 **여러 개의 하위 디렉터리** 생성

**파일 생성:**
- `boolean createNewFile()`: 비어있는 새 파일 생성, 동일한 이름의 파일이 이미 존재하면 false 반환
- **IOException 발생** → try-catch 문으로 처리 필요

**파일 삭제:**
- `boolean delete()`: 파일 삭제

**경로 출력 메소드:**
- `String getAbsolutePath()`, `File getAbsoluteFile()`: **절대 경로** 반환
- `String getCanonicalPath()`, `File getCanonicalFile()`: **절대적이고 유일한 경로** 반환
    - **Absolute vs Canonical**: Canonical 경로는 상대 경로일 경우에도 항상 유일한 경로 표현
- `String getParent()`: 파일 이름을 제외한 경로만 반환

---

## 디렉터리에 있는 목록을 살펴보기 위한 list 메소드들
**기본 목록 조회:**
- `static File[] listRoots()`: JVM이 수행되는 OS에서 사용 중인 파일 시스템의 **Root 디렉터리 목록**을 File 배열로 반환
- `String[] list()`: 현재 디렉터리의 하위 목록을 **String 배열**로 반환
- `File[] listFiles()`: 현재 디렉터리의 하위 목록을 **File 배열**로 반환

**필터를 사용한 목록 조회:**
- `String[] list(FilenameFilter filter)`
- `File[] listFiles(FileFilter filter)`
- `File[] listFiles(FilenameFilter filter)`

**FileFilter vs FilenameFilter:**
- **FileFilter**: `boolean accept(File pathname)` - File 객체 자체가 조건에 맞는지 확인
- **FilenameFilter**: `boolean accept(File dir, String name)` - 디렉터리와 파일 이름이 조건에 맞는지 확인
    - **단점**: 디렉터리와 파일을 구분하지 못함 (.jpg로 끝나는 디렉터리도 필터로 걸러낼 수 없음)

---

## InputStream과 OutputStream은 자바 스트림의 부모들이다
- 자바 I/O는 **추상 클래스** InputStream과 OutputStream을 통해 제공
- **InputStream**: 데이터를 읽을 때 사용, `abstract int read()` 메소드가 유일한 추상 메소드
- **OutputStream**: 데이터를 쓸 때 사용, write() 메소드 제공

**리소스 관리:**
- 두 클래스 모두 **Closeable 인터페이스** 구현 → `close()` 메소드 선언
- 작업 종료 시 해당 리소스를 **"항상" 닫아야 함** (안 지키면 심각한 오류 발생 가능)
- **OutputStream**은 추가로 **Flushable 인터페이스** 구현 → `flush()` 메소드로 버퍼 데이터 강제 쓰기

**주요 구현체:**
- **FileInputStream**: 파일 읽기, 이미지 등 바이트 코드 데이터
- **ObjectInputStream**: 객체 데이터 읽기

---

## Reader와 Writer
**char 기반 문자열 처리:**
- **Reader**: char 기반 문자열을 읽을 때 사용
    - InputStream과 유사하게 `read()` 메소드로 문자(char) 읽기
    - 작업 완료 후 반드시 `close()` 메소드 호출
    - 주요 구현체: BufferedReader, InputStreamReader
- **Writer**: char 기반 문자열을 쓸 때 사용
    - **Appendable 인터페이스** 구현 → `append()` 메소드 제공
    - String 타입은 `write()` 사용 가능, StringBuilder/StringBuffer 타입은 `append()` 사용이 더 편리

---

## 텍스트 파일을 써보자
**FileWriter vs BufferedWriter:**
- **FileWriter**: `write()`나 `append()` 메소드 호출할 때마다 파일에 직접 데이터 쓰기 → **비효율적**
- **BufferedWriter**: **버퍼 공간**에 저장할 데이터를 보관했다가 버퍼가 차면 데이터를 한 번에 저장 → **효율적**
    - 첫 번째 매개변수로 Writer 객체 필요

**주의사항:**
- FileWriter 객체 생성 시 **IOException 발생 가능** → try-catch 문 처리 필요
- **리소스는 가장 마지막에 연(open) 객체부터 닫기**
- finally 블록에서 bufferedWriter 먼저 닫고 → fileWriter 닫기

---

## 텍스트 파일을 읽어보자
**FileReader와 BufferedReader:**
- **BufferedReader의 readLine() 메소드**를 사용하여 **한 줄씩** 데이터 읽기

**Scanner 클래스** (java.util 패키지):
- 텍스트 기반의 기본 자료형이나 문자열 데이터를 편리하게 처리
- **정규 표현식(Regular Expression)**을 사용하여 데이터를 잘라 처리 가능
- `hasNextLine()`으로 다음 줄의 존재 여부 확인, `nextLine()`으로 한 줄씩 읽기

**Files 클래스** (Java 7):
- `Files.readAllBytes(Paths.get(fileName))`와 같이 **한 줄로 파일을 간단하게 읽기** 가능

---

## ❓ 핵심 질문들
1. close() 메소드를 호출하지 않으면 어떤 문제가 발생할 수 있는가?