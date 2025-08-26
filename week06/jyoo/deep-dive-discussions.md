## Week05 deep-dive-discussions

---
1. 저자는 자바의 Fork / Join 은 전부 `Fork/Join 기능은 java.util.concurrent 패키지의 RecursiveAction과 RecursiveTask라는 abstract 클래스를 사용해야 한다.` 라고 명시했으나, 자바8에서 등장한 CompletableFuture의 경우에는 해당 클래스들을 상속받거나 사용하지 않고 있다. CompletableFuture는 어떻게 Thread의 Fork  / Join을 가능하게 할까?

---
2. 인터페이스의 default 메서드는 어떻게 가능한 것일까?
   - 컴파일 시 모든 구현체에 해당 메서드가 포함되는 구조인가?