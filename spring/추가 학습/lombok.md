# Lombok



## @Data



### 활용

- POJO(Plain Old Java Objects)와 bean 관련된 모든 보일러플레이트 (재사용 가능한 코드) 생성
- getter / setter / toString /equals 와 같은 함수들 자동 생성
- @Getter / @Setter / @ToString / @EqualsAndHashCode / @RequiredArgsConstructor 를 합쳐놓음
- 주의!
  - callSuper, includeFieldName, exclude와 같은 파라미터와 같이 사용될 수 없음



## @Value

### 활용

- 생성 후 변경할 수 없는 불변 객체 만들기 위해선 @Data대신 @Value



## @NonNull

### 활용

- 변수에 붙이면 자동으로 null 체크
- 해당 변수가 null로 넘어온 경우, NullPointerException 예외 일으킴



```java
@NonNull @Setter
private String id;
```



## @NoArgsConstructor



## @AllArgsConstructor





## @Cleanup

### 활용

- IO 처리 / JDBC 활용 등 `try-catch-finally`문의 finally로 close() 처리 대신, 해당 자원이 자동으로 닫히는 것을 보장



```java
@Cleanup Connection con = DriverManager.getConnection(url, user, password);
```



## @Synchronized



### 활용

- `synchronized` 키워드 => 객체 레벨에 락이 생성



```java
@Synchronized
public void hello() {
    System.out.println("world");
}
```
