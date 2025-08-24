# ch.24 자바랭 다음으로 많이 쓰는 애들은 컬렉션 - Part3(Map)
---

## 1. Map 특징
- `Map<K,V>`: 키-값 쌍 저장, **키 중복 불가**, 값 중복 가능
- 주요 구현체: `HashMap`, `LinkedHashMap`, `TreeMap`, `Hashtable`, `ConcurrentHashMap`
- 컬렉션과의 차이: `Collection`은 값 모음, `Map`은 **키-값 사전**

---

## 2. HashMap · Hashtable · ConcurrentHashMap 비교
- **HashMap**
  - 동기화 X, **가장 많이 사용**, 평균적으로 빠른 조회/삽입
  - `null` **키 1개**, `null` 값 **여러 개** 허용
- **Hashtable (레거시)**
  - 모든 메서드가 동기화됨(단일 락) → 성능 저하
  - `null` **키/값 모두 불가**
- **ConcurrentHashMap**
  - 동시성 지원(세분화된 락/분할) → 다중 스레드 환경에서 권장
  - `null` **키/값 불가**
- **정리 포인트**
  - 단일 스레드/일반 사용: **HashMap**
  - 멀티스레드 고성능: **ConcurrentHashMap**
  - 레거시 호환 외에는 **Hashtable 지양**

### 간단 사용 예
```java
Map<String, Integer> map = new HashMap<>();
map.put("a", 1);
map.put("b", 2);
System.out.println(map.get("a")); // 1
for (Map.Entry<String,Integer> e : map.entrySet()) {
    System.out.println(e.getKey() + "=" + e.getValue());
}
```

---

## 3. HashMap 사용 시 주의사항
- 키 비교는 `equals`, 버킷 배치는 `hashCode`에 의존 → **둘 다 규약대로 재정의**
- **키로 사용한 객체의 상태 변경 금지**(hashCode/equals 결과가 바뀌면 탐색 실패)
- 성능을 위해 데이터가 많으면 **초기 용량**을 적절히 지정
- 이터레이터는 **fail-fast** (순회 중 구조 변경 시 예외)

---

## 4. TreeMap 핵심 (정렬/범위 조회)
- 키의 **자연 순서** 또는 **Comparator** 기준으로 **정렬 보장**
- 범위/탐색 메서드 사용: `firstEntry()`, `lastEntry()`, `floorEntry()`, `ceilingEntry()`
- 구간 뷰: `subMap(fromKey, toKey)`, `headMap(toKey)`, `tailMap(fromKey)`

### 간단 예
```java
NavigableMap<Integer,String> tm = new TreeMap<>();
tm.put(10, "ten");
tm.put(5, "five");
tm.put(20, "twenty");
System.out.println(tm.firstEntry());      // 5=five
System.out.println(tm.ceilingEntry(6));   // 10=ten
System.out.println(tm.subMap(5, true, 15, false)); // {5=five, 10=ten}
```

---

## 5. Properties (설정 파일) 기본
- `Properties`: 설정 정보를 **키/값(String 중심)** 으로 저장/로딩
- 전통적 `.properties` 포맷: `key=value`, `#` 주석
- 주요 메서드
  - 조회/설정: `getProperty`, `setProperty`
  - 로딩/저장: `load(InputStream)`, `store(OutputStream, comment)`
  - XML: `loadFromXML(InputStream)`, `storeToXML(OutputStream, comment, encoding)`
- 시스템 프로퍼티: `System.getProperties()`, `System.getProperty(key)`, `System.setProperty(key,val)`

### 파일에서 읽기/쓰기
```java
Properties props = new Properties();

// 읽기
try (InputStream in = new FileInputStream("app.properties")) {
    props.load(in);
}
String url  = props.getProperty("db.url");
String user = props.getProperty("db.user");

// 쓰기
props.setProperty("mode", "prod");
try (OutputStream out = new FileOutputStream("app.properties")) {
    props.store(out, "app settings");
}
```

### XML로 저장/읽기
```java
// 저장
try (OutputStream os = new FileOutputStream("app.xml")) {
    props.storeToXML(os, "settings", "UTF-8");
}

// 읽기
try (InputStream is = new FileInputStream("app.xml")) {
    props.loadFromXML(is);
}
```

### 클래스패스 리소스에서 읽기
```java
Properties p = new Properties();
try (InputStream in = MyClass.class.getResourceAsStream("/config/app.properties")) {
    p.load(in);
}
```
---

