# Ch.30 Java 7에서 달라진 것들에는?

---
### Java 7에서는...
- [JSR 336](https://download.oracle.com/otndocs/jcp/java_se-7-mrel-spec/license.html)에 정리가 되어 있음
  - JSR = Java Specification Requirement
- 대표적인 변경 사항:
  - 숫자 표시 방법 보안
  - switch 문에서 String 사용
  - 제네릭을 쉽게 사용할 수 있는 Diamond
  - 예외 처리시 다중 처리 기능
- 기능적인 큰 변화보단 편하게 개발을 할 수 있도록 변경됨
  - 가독성 적인 측면에서의 노력이 보인다고 함
---

### 변경된 숫자 표현법
- 기존:
  - byte, short, int, long 등 정수형은 8, 10, 16 진수로 표현
  - 숫자에 별다른 접두사가 없다면 10, 0이 앞에 붙어있으면 8, 0x 가 붙어있으면 16
    - ex: `int dec = 1; oct = 01; hex = 0x1;`
- 변경:
  - 2진수의 표현이 추가됨
    - `0b` 를 접두사로 사용하면 2진수로 취급됨
  - `_`를 사용해 숫자 사이에 넣어 가독성을 증가시킴
    - 숫자로 취급되는 부분에만 사용 가능
      - 정수를 구분하는 접두사 (0, 0x,0b) 접미사 (l,f, L, F,d,D)
  
---
### 확장된 Switch
- 기존:
  - 정수형만 사용가능했음
    - enum의 경우, 컴파일러가 enum의 스위치문을 ordinal() 의 결과로 치환해서 동작하게 해줬음
- 변경:
  - String 도 사용가능해짐 
    - String 의 값이 null 인 경우에는 npe 발생

---
### 제네릭과 다이아몬드
- 기존:
  - 제네릭을 사용시 생성자에도 타입을 명시해 줬어야 함
    - ex: `List<String> list = new ArrayList<String>();`
- 변경:
  - 생성자에는 일일이 타입을 명시해 줄 필요가 없어짐
  - 이미 선언부에 지정해 뒀기 떄문에 불필요하다는 판단인 듯. (Less is more)
    - `List<String> list = new ArrayList<>();`
    - <> 이렇게 표기하기에 다이아몬드라고 칭한다고 함.
  - 제약 사항:
    1. 미지정시 컴파일 경고 발생
       - `List<Integer> list = new ArrayList();` 이렇게 <> 없이 생성해도 컴파일시 경고만 뜸
    2. 메서드 내에서 객체 생성시
    3. 제네릭하면서 제네릭하지 않은 객체 생성시
       - 2중 3중으로 제네릭을 사용하는 경우에 발생가능한 이슈

---
### non-reifiable varargs type
- reifiable -> 실행시에도 타입 정보가 남아있는 타입.
- non-reifiable -> 컴파일시 타입 정보가 손실되는 타입.
  - ex: Collection.addAll 메서드의 매개 변수는 Collection<? super T> c, T... elements 인데, c 는 콜렉션의 구현체 중 하나이며 그 원소들이 뒤에 오는 객체들의 하위인 객체이다.
  - 이 메서드를 자바 6 에서 사용하게 될 경우 경고가 발생하는데, 뒤에 오는 ... 으로 받는 매개변수가 배열로 변환되어 사용이 되는데 이 떄 문제가 발생할 수 있다한다.
    - 제네릭은 타입 소거(type erasure) 때문에 런타임에 구체적인 타입을 알 수 없음 => List<String>... lists 같은 varargs는 실제로는 List[] 배열로 만들어지고, 이게 heap pollution(힙 오염) 위험을 일으킴.
      - 해당 메서드는 일으키지 않는 메서드이다. 이러한 오염을 방지하는 방법이 책에서 말한 
        1. 가변 매개변수를(reifiable하게) 사용
        2. final / static 으로 선언된 메서드
        - 를 적용한 후 @SafeVarArgs 에너테이션을 붙이면 경고가 사라짐.

---
### 예외처리 관련
- catch 블록이 동일할 경우 예외를 collapse 할 수 있게 변경됨 -> 가동성 강화
- try-with-resources 가 도입됨
  - Java7 에서 AutoCloseable 이라는 인터페이스가 추가되었고, 이 인터페이스를 구현하는 구현체들에 대해서는 close 할 필요 없이 try-with-resources 로 예외 블록을 처리하면 close 되지 않는 상황을 완벽하게 방지가 가능하다.