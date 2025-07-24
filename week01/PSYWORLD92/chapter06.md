## ch.6 제가 조건을 좀 따져요

### ✅ 조건문: `if`, `switch`

#### if 문
```java
if (조건) {
    // 실행문
} else if (조건2) {
    // 실행문
} else {
    // 나머지 처리
}
```

#### switch 문
```java
switch (변수) {
    case 값1:
        // 처리1
        break;
    case 값2:
        // 처리2
        break;
    default:
        // 그 외
        break;
}
```

---

### ✅ 반복문 종류

- `for`
- `while`
- `do-while`
- `label` (중첩 반복문 제어용)

---

#### while 문

```java
while (조건) {
    // 반복할 문장
}
```

> 조건이 **참인 동안 반복**,  
> `break`, `continue` 사용 시 **무한 루프 주의!**

---

#### do-while 문

```java
do {
    // 최소 한 번 실행되는 문장
} while (조건);
```

> 조건과 상관없이 **무조건 1번은 실행됨**

---

#### for 문

```java
for (초기화; 조건; 증가식) {
    // 반복 문장
}
```

- `continue`, `break` 사용 가능

---

### ✅ label 문

- 중첩 반복문에서 **특정 루프만 빠져나가거나 건너뛸 때 사용**

```java
outer: for (int i = 0; i < 5; i++) {
    for (int j = 0; j < 5; j++) {
        if (j == 3) break outer;  // outer 루프까지 탈출
    }
}
```

---

> ⚠️ `label + break/continue`는 **복잡한 흐름 제어 시에만 제한적으로 사용하는 것**이 좋다.
