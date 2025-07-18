# Ch.04 정보를 어디에 넣고 싶은데?
- 자바에는 4가지 변수가 존재함
    - 지역 변수 (local variable)
        - 중괄호 내에서 선언된 변수
    - 매개 변수 (parameters)
        - 메서드에 넘겨주는 변수
    - 인스턴스 변수 (instance variables)
        - 메서드 밖에 클래스 안에 선언된 변수
        - 인스턴스 라이프 사이클에 묶여있는 변수
    - 클래스 변수 (class variables)
        - 메서드 밖 클래스 안에 선언된 변수 중 static 예약어와 함께 선언된 변수
- String 은 참조 자료형중 유일하게 생성시 new 없이 생성가능한 참조 자료형.
    - 물론 new String() 으로도 가능
- `직접해 봅시다`
  ```java
  public class ProfilePrint{
    byte age;
    String name;
    boolean isMarried;
  
    public void setAge(byte age) {
        this.age = age;
    } 
  
    public byte getAge() {return age;0}
    public void setName(String name) {this.name=name;}
    public String getName() {return name;}
    public void setMarried(boolean isMarried) {this.isMarried=isMarried;}
    public boolean isMarried() {return isMarried;}
    
    public static void main(String[] args) {
        ProfilePrint pp = new ProfilePrint();
        pp.setAge(32);
        pp.setName("name");
        pp.setMarried(false);
        System.out.println("age " + pp.getAge());
        System.out.println("name " + pp.getName());
        System.out.println("isMarried " + pp.isMarried());
    } 
  }
  ```
- `정리해 봅시다`
  - 