# 📘 자바의 신 VOL.2 – 5장: 자바랭 다음으로 많이 쓰는 애들은 컬렉션 - Part2(Set과 Queue)

## Set이 왜 필요하지?
- **중복을 허용하지 않는** 집합 자료구조 필요할 때 사용함.
- **순서에 의미가 없고** 포함 여부(멤버십) 판단이 중요할 때 적합함.
- 수학적 **합집합/교집합/차집합** 같은 집합 연산을 표현하기 쉬움.

---

## HashSet에 대해서 파헤쳐 보자
- 가장 많이 쓰이는 `Set` 구현체.
- **해시 테이블 기반**으로 동작하며 **저장 순서를 보장하지 않음**.
- 동일성 판단은 **`equals`와 `hashCode`** 계약에 따름  
  → 두 메서드가 일관되게 재정의돼 있어야 중복 제거가 정확히 동작함.

---

## HashSet의 생성자들도 여러 종류가 있다
- **`HashSet()`**: 기본 용량/부하율로 빈 집합 생성.
- **`HashSet(Collection<? extends E> c)`**: 다른 컬렉션의 요소로 초기화.
- **`HashSet(int initialCapacity)`**: 초기 용량 지정.
- **`HashSet(int initialCapacity, float loadFactor)`**: 초기 용량과 **로드 펙터** 지정.

---

## HashSet의 주요 메소드를 살펴보자
- **추가/삭제**: `add`, `addAll`, `remove`, `removeAll`, `clear`
- **조회/검사**: `contains`, `containsAll`, `isEmpty`, `size`, `iterator`, `toArray`
- **중복 규칙**: `add`가 `false`를 반환하면 **이미 존재**한다는 뜻(중복 미추가)

---

## Queue는 왜 필요할까?
- **FIFO(First-In First-Out)** 처리 모델이 필요할 때 사용함.
- 작업 대기열, 버퍼링, 생산자-소비자 패턴 등에서 **먼저 들어온 것을 먼저 처리**하는 흐름 보장.

---

## LinkedList를 파헤쳐보자
- **양방향 연결 리스트**로 구현됨.
- `List`이면서 동시에 **큐/덱(Deque)** 용도로 사용 가능.
- **양 끝에서의 삽입/삭제가 잦은 경우**에 유리함.

---

## LinkedList의 생성자와 주요 메소드를 살펴보자
- 생성자
    - **`LinkedList()`**: 빈 리스트/큐 생성.
    - **`LinkedList(Collection<? extends E> c)`**: 다른 컬렉션으로 초기화.
- 큐/덱 관련 주요 메소드
    - **삽입**: `offer`, `offerFirst`, `offerLast`, `addFirst`, `addLast`
    - **조회(삭제 없음)**: `peek`, `peekFirst`, `peekLast`, `getFirst`, `getLast`
    - **삭제**: `poll`, `pollFirst`, `pollLast`, `removeFirst`, `removeLast`
- 리스트 공통 메소드도 그대로 사용 가능(`add`, `get`, `remove`, `size` 등)

---
## ❓
1. HashSet의 초기 capacity가 16인 이유와, 2의 거듭제곱으로만 확장되는 이유는? 비트 연산과의 연관성은?