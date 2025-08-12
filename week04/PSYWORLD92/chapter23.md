# ch.23 자바랭 당므으로 많이 쓰는 애들은 컬렉션 - Part2(Set과 Queue)

## 1. Set이 왜 필요할까?
- **중복을 허용하지 않는 집합 자료구조**가 필요할 때 사용.
- **순서가 중요하지 않고**, 데이터의 존재 여부 확인이 주 목적.
- 수학적 집합 연산(합집합, 교집합, 차집합)에 적합.

### 주요 구현체
1. **HashSet**
   - 해시 테이블 기반 저장, 순서 보장 X
   - 성능 가장 좋음
2. **TreeSet**
   - 저장 시 정렬, Red-Black Tree 기반
3. **LinkedHashSet**
   - 연결 리스트 기반 해시 테이블, 저장 순서 유지
   - 성능 가장 느림

---

## 2. HashSet
### 생성자
- `HashSet()` : 기본 용량 16, 로드 팩터 0.75
- `HashSet(Collection<? extends E> c)` : 다른 컬렉션 복사
- `HashSet(int initialCapacity)` : 초기 용량 지정
- `HashSet(int initialCapacity, float loadFactor)` : 용량 + 로드 팩터 지정

### 로드 팩터
- **로드 팩터(load factor)** = 데이터 개수 / 저장 공간 크기
- 값이 크면 공간 활용률↑, 검색 속도↓
- 값이 작으면 검색 속도↑, 공간 낭비↑

### 주요 메소드
- 추가/삭제: `add`, `addAll`, `remove`, `removeAll`, `clear`
- 조회/검사: `contains`, `isEmpty`, `size`, `iterator`, `toArray`
- `add`가 false → 이미 존재(중복)

---

## 3. HashSet 용량이 16이고 2의 거듭제곱으로 확장되는 이유
1. **기본값 16**
   - `HashMap` 기본값과 동일 (내부적으로 `HashMap` 사용)
   - 성능과 메모리 효율의 균형
2. **2의 거듭제곱 확장**
   - 인덱스 계산 시 `(n - 1) & hash` 사용 → 균등 분포
   - `%` 대신 비트연산(&)으로 속도 향상
3. **비트 연산 장점**
   - 빠른 계산, 충돌 최소화

---

## 4. Queue
- **FIFO(First-In First-Out)** 구조
- 작업 대기열, 버퍼, 생산자-소비자 패턴 등에 사용
- 자바에서는 `Queue`, `Deque` 인터페이스를 통해 사용

---

## 5. LinkedList
- **양방향 연결 리스트**
- `List`, `Queue`, `Deque` 인터페이스 구현
- 양 끝에서 삽입/삭제가 잦을 때 유리
- 중간 데이터 삽입/삭제 시 효율적

### 생성자
- `LinkedList()` : 빈 리스트 생성
- `LinkedList(Collection<? extends E> c)` : 컬렉션 복사

### 주요 메소드
#### 삽입
- `addFirst`, `addLast`, `offer`, `offerFirst`, `offerLast`, `push`
#### 조회(삭제 없음)
- `getFirst`, `getLast`, `peek`, `peekFirst`, `peekLast`, `get(int)`
#### 삭제
- `remove`, `removeFirst`, `removeLast`, `poll`, `pollFirst`, `pollLast`
#### 검색
- `contains`, `indexOf`, `lastIndexOf`
#### Iterator 관련
- `listIterator(int index)` : 지정 위치부터 순차 탐색
- `descendingIterator()` : 역순 탐색

---

## 📌 요약
- **Set**: 중복 없는 집합, HashSet 성능 최고, 로드 팩터/용량 고려 필요
- **HashSet**: 내부 `HashMap` 기반, 기본 용량 16, 2의 거듭제곱 확장
- **Queue**: FIFO, 대기열/버퍼 구조에 적합
- **LinkedList**: 양방향 연결 리스트, 양 끝 삽입/삭제 최적화
