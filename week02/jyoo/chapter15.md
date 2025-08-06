# Ch.15 String
- Java에서 String은 VVIP 클래스
- Character Set에 따라 String 의 바이트 배열의 값이 변경되는 이유는 무엇일까?
  - 인코딩(Charset)은 문자 ↔ 바이트 사이의 매핑 규칙이기 때문에, 어떤 Charset을 쓰느냐에 따라 결과 바이트 배열이 달라짐.
- 일반적인 케이스 (ex: Vue3 클라이언트 + Spring Boot 서버) 인 경우에는 캐릭터셋에 대한 지정이 없는대도 알아서 클라이언트에서 보낸 리퀘스트를 올바른 스트링으로 변환해주는 원리는 뭘까?
  - 클라이언트(브라우저)와 서버(Spring Boot)는 둘 다 기본적으로 UTF-8을 사용하며, HTTP 요청/응답의 Content-Type 헤더에 charset이 명시되거나 기본으로 처리되기 때문.
    - Spring Boot의 HttpMessageConverter (예: MappingJackson2HttpMessageConverter, StringHttpMessageConverter)는 기본 Charset을 UTF-8로 설정해 둠 ([Spring Doc](https://docs.spring.io/spring-framework/docs/3.2.4.RELEASE_to_4.0.0.M3/Spring%20Framework%203.2.4.RELEASE/org/springframework/jms/support/converter/MappingJackson2MessageConverter.html))
- String 에 == 연산이 가능한 이유
  - Constant Pool 에 등록하여 이미 등록되어 있는 스트링이기에 재활용하기에 String 에 대해서는 == 는 equals 와 같은 효과가 있다.
  - 그러나 만일 new String() 을 통해 생성된 경우는 풀에 있더라도 재생성하기에 비교시 실패한다! (equals 로 비교 가능)

### 절대 사용해선 안되는 메서드
#### intern()
- new String() 으로 생성한 문자열 객체라 할지라도 풀에 해당 문자열 값이 있다면 풀에 있는 객체를 리턴하도록 하게하고, 없으면 추가하고 그 참조를 반환하는 메서드.
- 자바가 아닌 C로 구현되어 있는 native 메서드.
- 시스템의 심각한 성능저하를 발생시킬 수도 있는 메서드이다.
  - 이 메서드는 억지로 문자열 풀에 값을 할당하게 만들면 저장되는 영역에는 한계가 있기에 별도로 메모리를 청소하는 단계를 거치게 되고 이는 자바 시스템 전체에 영향을 줄 수 있다.
  - 그러나 이 메서드가 존재한다는 것은, 쓰이고 있는 곳이 있다는 의미일 것이다. 어떤 케이스에 필요한걸까?
    - Jackson 에서는 minor performance optimization 의 일환으로 InternCache 클래스에서 필드명의 반복등이 있을 때 사용하여 메모리와 비교 비용을 줄임.

### immutable한 String의 단점을 보완하는 StringBuffer와 StringBuilder
- String 은 불변이기에 문자열을 조작하는 행위 (더하거나 삭제하는 작업)등을 행할시 기존 문자열은 버려지며 GC의 대상이 된다.
- 이렇게 될 경우, 매번 새로운 String 객체가 생성되며 Heap 메모리에 할당하게되고 이는 GC 부담 + 성능 저하로 이어지게 됨
- 이를 해결하기 위한 클래스들이 StringBuffer 와 StringBuilder.
  - 이들은 어떻게 동작하기에 추가적인 스트링 객체 생성과 GC 부하를 줄이는 걸까?
    - 각 클래스들의 필드로 문자 배열(char[])을 가지고 있고, 매 추가 (append) 마다 해당 배열에 값들을 추가하고 배열이 부족하면 2배 크기로 확장 시킴.
      - 여기서 새로운 배열의 할당이 훨씬 더 메모리 효율이 좋다. 객체가 새로 생성되고 할당되는 작업이 적고, GC 되는 데이터는 결국 하나이기 때문.
- JDK 5 이상부턴 String 의 + 연산 실행시 내부적으로 StringBuilder를 이용하도록 변경해 준다고 함.
- StringBuilder vs StringBuffer 

  |        | StringBuilder                  | StringBuffer                                |
  |----------|--------------------------------|---------------------------------------------|
  | 쓰레드 안전성 | ❌ 비동기 (싱글 스레드용)        | ✅ 동기화된 메서드 (멀티 스레드 안전)         |
  | 성능     | ✅ 더 빠름                       | ❌ 상대적으로 느림                           |
  | 사용 대상  | 일반적 문자열 조작              | 멀티 스레드 환경에서 문자열 조작             |
  | 내부 구조  | `char[]` + `append()` 등        | 구조는 동일하나 `synchronized` 키워드 추가   |
