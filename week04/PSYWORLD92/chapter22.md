# ch.22 자바랭 다음으로 많이 쓰는 애들은 컬렉션 - part1 (List)

---

## 1. List 인터페이스 개요
- 순서가 있는 데이터 집합, **중복 허용**
- 주요 구현체: `ArrayList`, `LinkedList`, `Vector`
- Vector: Thread-Safe, ArrayList: Non Thread-Safe
- `iterator()`로 순차 접근 가능

```java
Iterator<String> it = list.iterator();
while (it.hasNext()) {
    System.out.println(it.next());
}
```

---

## 2. ArrayList 특징
- 배열 기반의 자료구조, 동적 크기 조절
- 인덱스를 이용한 빠른 접근
- 중간 삽입/삭제 시 성능 저하

**상속 구조**
```
List ← AbstractList ← ArrayList
```

---

## 3. ArrayList 생성자
| 생성자 | 설명 |
|--------|------|
| `ArrayList()` | 기본 용량 10 |
| `ArrayList(int initialCapacity)` | 초기 용량 지정 |
| `ArrayList(Collection<? extends E> c)` | 다른 컬렉션 복사 생성 |

---

## 4. ArrayList 주요 메서드
| 메서드 | 반환 타입 | 설명 |
|--------|----------|------|
| `add(E e)` | boolean | 요소 추가 |
| `add(int index, E element)` | void | 지정 위치에 삽입 |
| `addAll(Collection<? extends E> c)` | boolean | 컬렉션 전체 추가 |
| `get(int index)` | E | 요소 반환 |
| `set(int index, E element)` | E | 요소 변경 |
| `remove(int index)` | E | 해당 인덱스 요소 삭제 |
| `remove(Object o)` | boolean | 해당 객체 삭제 |
| `clear()` | void | 모든 요소 삭제 |
| `size()` | int | 요소 개수 |
| `isEmpty()` | boolean | 비었는지 여부 |
| `contains(Object o)` | boolean | 포함 여부 |

---

## 5. null 값 처리
- ArrayList는 null 요소 허용
- `contains(null)` 가능

---

## 6. 성능 고려
- 읽기/탐색 → 빠름
- 중간 삽입/삭제 → 느림
- 멀티스레드 환경에서는 `Collections.synchronizedList()` 사용

---

## 7. ArrayList 예제
```java
List<String> list = new ArrayList<>();
list.add("A");
list.add("B");
list.add(1, "C");
list.remove("B");
System.out.println(list); // [A, C]
```

---

## 8. Stack 클래스 개요
- LIFO(Last-In, First-Out) 구조
- `java.util.Stack` 클래스는 Vector 상속
- 주요 용도: 메서드 호출 기록, 실행 취소, 괄호 검사

---

## 9. Stack 주요 메서드
| 메서드 | 반환 타입 | 설명 |
|--------|----------|------|
| `push(E item)` | E | 요소 추가 |
| `pop()` | E | 맨 위 요소 제거 후 반환 |
| `peek()` | E | 맨 위 요소 반환(제거 안 함) |
| `empty()` | boolean | 비었는지 여부 |
| `search(Object o)` | int | 요소 위치(1부터), 없으면 -1 |

---

## 10. Stack 예제
```java
Stack<Integer> stack = new Stack<>();
stack.push(1);
stack.push(2);
stack.push(3);
System.out.println(stack.pop()); // 3
System.out.println(stack.peek()); // 2
System.out.println(stack); // [1, 2]
```

---

✅ **정리**
- **List**는 순서 있는 자료구조, ArrayList와 Stack은 대표 구현체
- ArrayList: 탐색/끝 삽입에 강점, 중간 삽입/삭제는 약점
- Stack: LIFO 구조로 특정 알고리즘(DFS, 괄호 검사 등)에 적합

✅ **정리**
- Stack은 LIFO 구조의 자료구조
- `push`, `pop`, `peek` 메서드로 데이터 조작
- Vector 기반이므로 동기화 지원(Thread-Safe)
