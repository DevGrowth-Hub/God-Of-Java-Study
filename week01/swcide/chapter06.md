# 📘 자바의 신 VOL.1 – 6장: 제가 조건을 좀 따져요

---

## 1. 조건문 (Conditional Statements)

### ● if 문
- 조건식은 boolean 타입만 가능
- 단일 문장은 `{}` 생략 가능, 여러 문장은 블록 사용
- `else`, `else if`로 조건 분기 가능
- `if`, `if`는 독립 구조 → 조건이 모두 true일 경우 모두 실행
- 중첩 if-else는 가독성이 떨어짐 → 메서드 분리나 early return 권장

### ● switch 문
- 특정 값에 따라 분기
- 사용 가능한 타입: `byte`, `short`, `char`, `int`, `enum`, `String` (JDK 7+)
- `long`은 사용 불가
- break 없으면 다음 case까지 실행됨 (fall-through)
- `default`는 어느 case에도 해당하지 않을 경우 실행

---

## 2. 반복문 (Loops)

### ● while 문
- 조건이 true일 때 반복
- 조건이 처음부터 false면 실행되지 않음

### ● do-while 문
- 조건 검사 전, 무조건 한 번 실행
- 끝에 세미콜론(`;`) 필요

### ● 제어 키워드
- `break`: 반복문 즉시 종료
- `continue`: 현재 반복을 중단하고 조건 검사로 이동  
  (루프의 나머지 부분을 모두 건너뜀)

### ● for 문
- 형식: `for (초기화; 조건식; 증감식)`
- 초기화는 처음 한 번만 실행
- 조건식은 boolean
- 증감식은 반복마다 실행
- 외부 변수 사용 가능하지만 가독성 저하 우려

---

## 3. Label (라벨)

- 반복문에 이름 부여 가능
- 중첩 루프에서 외부 루프로 직접 이동할 때 사용
- `break 라벨`, `continue 라벨` 형식
- 실무에서는 거의 사용되지 않음


