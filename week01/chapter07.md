## ch.7 여러 데이터를 하나에너을 수는 없을까요?

### ✅ 배열이란?
- 하나의 변수에 **여러 개의 값을 담을 수 있는 자료구조**
- 배열도 **참조 자료형**이기 때문에 `new`로 생성해야 함

```java
int[] lottoNumbers;
int lottoNumbers[];  // 둘 다 가능하지만 첫 번째 방식을 권장
```

> 배열 선언만 하면 **데이터 개수는 알 수 없음**  
> → 반드시 **초기화 필요**

```java
int[] lottoNumbers = new int[7];  // 길이 7로 초기화
```

---

### ✅ 인덱스 주의
```java
lottoNumbers[1] = 15;
```
- 배열의 인덱스는 **0부터 시작**  
- `new int[7]` → 인덱스는 `0~6`까지 존재

---

### ✅ 배열의 기본값
| 타입 | 초기값 예시 |
|------|-------------|
| 기본형 (int, float 등) | `0`, `0.0`, `false` |
| 참조형 (String 등)     | `null` |

---

### ✅ 배열 선언 방법

```java
int[] lottoNumbers = new int[7];  // 기본 선언
int[] lottoNumbers = {1, 2, 3, 4, 5, 6, 7};  // 리터럴 초기화
```

---

### ✅ 2차원 배열

```java
int[][] twoDim = new int[2][3];  // 2행 3열
```

또는 가변 길이 배열도 가능:

```java
int[][] twoDim = new int[2][];
twoDim[0] = new int[3];
twoDim[1] = new int[2];
```

또는 선언과 동시에 초기화:

```java
int[][] twoDim = {
  {1, 2, 3},
  {4, 5, 6}
};
```

---

### ✅ 배열 출력 (반복문 사용)

```java
for (int i = 0; i < arr.length; i++) {
    System.out.println(arr[i]);
}
```

또는 **향상된 for문** 사용:

```java
for (int data : arr) {
    System.out.println(data);
}
```

---
