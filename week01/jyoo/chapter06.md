# Ch.06 제가 조건을 좀 따져요
- 이장을 건너 뛸 수 있는 조건:
  - 아래 항목들을 타인에게 **완벽히** 설명할 수 있정도.
    1. if 와 같은 조건 구문
    2. for 나 while 과 같은 반복 구문
    3. continue, break 와 같은 자바 예약어

- **프로그램의 코드를 작성하는 것 -> 조건을 따지는 작업을 반복적으로 수행하는 것**
- 자바에서의 if 문 공식:
  - `if(boolean 값) 처리 문장;`
- 하나의 값이 여러 범위에 걸쳐서 비교되어야 할 경우 -> switch
  - 자바에서의 switch 문:
    - ```java
        switch(비교대상변수) {
            case 점검값1:
            처리문장1;
            ...
            breek;
            case: 점검값2:
            처리문장2;
            ...
            break;
            default:
            기본처리문장;
            break;
        }
        ```
  - switch 문의 비교대상 변수로는 long 을 제외한 정수형과 몇몇 특별한 타입만이 들어갈 수 있다. (char, byte, short, int, Character, Byte, Short, Integer, String, or an enum type)
    - default 는 꼭 맨 마지막에 오지 않더라도 문제가 없다. 그러나 중간에 애매하게 껴있는 경우 원하는 동작이 보장되지 않는다.
      - ```java
          public void failingSwitch(short a) {
            switch (a) {
              case 1:
              case 2:
              case 3:
              case 4:
                  System.out.println("5 보다 작음");
                  return;
              default:
                  System.out.println("10보다도 큼");
              case 5:
              case 6:
              case 7:
              case 8:
              case 9:
                  System.out.println("10보다는 작음");
                  return;
            }
        }
        ```
      - 이 함수의 경우 a = 11 이면 default 만 타서 `10보다도 큼` 민 츨력하고 끝나야 하지만, 실제로는 break 가 없어서 `10보다는 작음` 까지 출력됨. 

### 반복문:
- Java 에는 for 루프와 while 문 두 가지의 반복문이 있음.
  - 이 외에도 stream 등을 이용한 iterating 방법이 있으나, 결국 내부적으로는 for 문이나 while 문을 이용한 구현이 되어 있다.
- `continue` 는 while / for 문에서 사용될 경우, '반복문을 조회하는 조건' 점검 부분으로 다시 가라는 의미.
- do-while 과 while 의 차이 -> do-while 은 반드시 한번은 반복문이 실행됨. 
- 임의로 중지시키지 않으면 무한 루프에 빠지는 while 루프 대신 대부분의 경우엔 `for` 루프가 선호된다.
- for 문의 구조:
  - ```java
    for (초기화  ; 종료조건 ; 조건 값 증가) {
        반복문장;
    }
    ```
- 많이 사용안하는 label
  - continue / break 등과 함께 쓰여야 하며, 두개 이상의 반복문이 중첩되는 경우에 활용이 가능한 예약어로 continue / break 를 할때 어느 지점으로 다시 돌아 / 나갈 것인지를 지정하는 예약어 이다.
  - ```java
    endLabel:
    for (int i = 0; i <10; i++) {
        for (int j = 0; j < 10; j++) {
            if(j >3) break endLabel;
                System.out.println("ij = " +i+ j);
            }
	    }  
    ```
- `직접해 봅시다`
  ```java
    public class InterestManager {
      public static void main(String[] args) {
        InterestManager interestManager = new InterestManager();
        int total = 1000000;
        double addedAmount = 0;
        for (int i = 10; i <= 365; i+=10) {
          addedAmount = interestManager.calculateAmount(i, total);
          if(i%10==0) {
            System.out.println(i+":"+ addedAmount +" ");
                } 
              }  
         }

        public double getInterestRate(int day) {
        if(1 <= day && day <= 90) {
          return 0.005;
        } else if(91 <= day && day <= 180) {
          return 0.01;
        } else if(181 <= day && day <= 364) {
          return 0.02;
        }
        return 0.056;
    }

    public double calculateAmount(int day, int amount) {
        return amount + amount * getInterestRate(day);
    }
  }

## 의문점들:
- if문의 처리 문장 파트는 비어있어도 된다 (ex:`if(true);`)
  - 세미콜론 하나만 있으면 컴파일/실행이 정상적으로 된다.
  - 자바에서 세미콜론(;)은 빈 문장(empty statement)을 의미함. ([JSL-14.6](https://docs.oracle.com/javase/specs/jls/se11/html/jls-14.html#jls-14.6))
  - 즉 빈 문장이기에 무조건 정상적으로 실행이 된다. `if(true);;;;;;` 이러한 문장도 정상 실행.

- switch 에서는 중괄호는 생략할 수 없다. (if와는 다른점)
  - 왜일까? default case 만 있는 switch 는 존재할 수 있는데도 왜 그렇게만 가능하게 만들었을까?
  
- 164 쪽의 내용중, `break 를 사용하면 현재 수행중인 중괄호에서 빠져 나간다` 그러나 메서드의 중괄호는 빠져나갈 수는 없다고 한다. 왜 메서드는 break 을 이용해 중단 할 수 없게 만들었을까?
  - Java에서의 break 의 정의는 책의 저자가 말한 '블록 탈출' 보다는, '루프의 제어'에 더 가까운 것 같다 ([JSR](https://docs.oracle.com/javase/specs/jls/se21/html/jls-14.html#jls-14.15)). 
  - 메서드의 흐름을 제어하는 예약어는 이미 return 이 존재하기에 break 의 역할은 루프의 제어로 잡고 정한 스펙이 아닐까 추정.