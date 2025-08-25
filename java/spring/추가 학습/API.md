# API



## ResponseEntity

### **RequestEntity** / ResponseEntity 란

- HttpEntity : HTTP 요청 / 응답에 해당하는 HttpHeader와 HttpBody를 포함하는 클래스

- ResponseEntity : HttpEntity를 상속받아 **HttpStatus, HttpHeaders, HttpBody**를 포함

  => 데이터 응답에 더 세밀한 제어 O


### VO

- DTO와 비슷한 개념이지만 read-only 속성 가짐
- 사용 용도
  - 사용자의 데이터 입력이 왔을 때 데이터나 조회 조건을 VO에 담아 DAO에 전달
  - HttpBody에 넣어서 전달한 데이터 생성
- 필요성
  - Network traffic 줄임에 효과적 (?)


### ModelMapper

```xml
<dependency>
    <groupId>org.modelmapper</groupId>
    <artifactId>modelmapper</artifactId>
    <version>2.3.8</version>
</dependency>
```

- 어떤 Object에 있는 필드값들을 자동으로 원하는 Object로 Mapping 시킴

- strategy
  - STRICT : 정확히 일치 필드만 매핑
  - STANDARD : 지능적 매핑
  - LOOSE : 느슨하게 매핑

```JAVA
ModelMapper mapper = new ModelMapper();
mapper.getConfiguration().setMatchingStrategy(MatchingStrategies.STRICT);

UserDto userDto = mapper.map(user, UserDto.class);

ResponseUser responseUser = mapper.map(userDto, ResponseUser.class);
```



## Session

### 정의 및 용도

- 세션은 클라이언트 별로 서버에 저장되는 정보
- 사용자 컴퓨터에 저장되는 쿠키와 다르게 서버에서 관리하므로, 보안이 필요한 데이터를 세션에서 관리
- 서버 종료 or 유효시간이 지나면 사라짐


### 사용

- 서버는 클라이언트를 식별하는 session id를 생성하여 구분함
- 서버는 session id로 key와 value를 저장하는 HttpSession 생성 + session id를 저장하는 쿠키를 클라이언트에 전송
- 서버는 쿠키의 session id로 HttpSession을 찾음


### 세션 생성

```java
HttpSession session = request.getSession(); //HttpSession session = request.getSession(true);
```

- 디폴트가 true 값으로, 서버에 생성된 session이 있다면 세션 반환, 없으면 생성

```java
HttpSession session = request.getSession(false);
```

- 세션이 없으면 null 반환



### 세션에 값 저장

```java
session.setAttribute(<key>, <value>);
// key는 Stirng, value는 Object 
```



### 세션 조회

```java
session.getAttribute(<key>)
```



### 세션 삭제

```java
session.removeAttribute(<key>);
session.invalidate(); // 전제 데이터 삭제
```



















