# Enum



## 개념



### 개념

- 열거체
- 이름 지어진 값들의 집합체로, 일정 개수의 상수 값을 정의
- 값의 변경 및 추가 X



## 활용



### 생성자와 final 필드

```java
public enum Day {
    MON("Monday"),
    TUE("Tuesday"),
    WED("Wednesday"),
    THU("Thursday"),
    FRI("Friday"),
    SAT("Saturday"),
    SUN("Sunday")
    ;

    private final String label;

    Day(String label) {
        this.label = label;
    }

    public String label() {
        return label;
    }
}
```

- 필드 추가 => Enum 요소에 특정 값 매핑 O
- 해당 필드에는 `final` 키워드를 통해 변경 X 설정
- Enum의 생성자는 외부에서 `new`로 생성하기 위함이 아닌, 내부에서 요소의 필드 값 설정 용도



### 기본 메소드

- `static E values()`
  - 열거체의 모든 상수를 저장한 배열을 생성하여 반환
  
- `static E valueOf(String name)`
  - 전달된 문자열과 일치하는 해당 열거체의 상수를 반환
  
- `String name()`
  - 열거체 상수의 이름을 반환

    ```java
    Day.MON.name()  // MON
    ```
  
  :bulb: `T 필드명()`
  
  - 필드값은 .필드명() 으로 접근
  
    ```java
    Day.MON.label() // Monday
    ```
  
- `int ordinal()`

  - 열거체 상수가 열거체 정의에서 정의된 순서(0부터 시작)를 반환

    

### 순회

```java
for(Day day : Day.values()){
    //
}
```

- `values()`로 Enum 요소 순회 가능



### static field

- 요소에 종속적인 field값 뿐만 아니라, Enum 클래스 필드 설정 O
- Map, 필드값 list 등 자체 제작하여 사용 O

```java
public static Day valueOfLable(String lable){
    // ...
}
```



