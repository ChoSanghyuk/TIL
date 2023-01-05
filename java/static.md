# Static



## 개념

### static 이란

- 클래스에 고정된 멤버
- 클래스 로더가 메소드 메모리 영역에 적재할 때 클래스별로 관리 => 클래스 로딩 시 즉시 사용 O



### Static / Heap 영역

- Static

  - 클래스들이 할당

  - Garbage Collector 관여 X

  - 모든 객체가 메모리 공유

    => 남발 시, 시스템 성능에 악영향

- Heap

  - 객체들이 할당
  - Garbage Collector 관여 O
  - 메모리 공유 X







## 코드



### static map 초기화

```java
public static final Map<String, String[]> keyMap;

static {
    keyMap = new HashMap<>();
    keymap.put();
}
```

