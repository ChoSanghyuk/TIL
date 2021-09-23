# Session



## 정의 및 용도

- 세션은 클라이언트 별로 서버에 저장되는 정보
- 사용자 컴퓨터에 저장되는 쿠키와 다르게 서버에서 관리하므로, 보안이 필요한 데이터를 세션에서 관리
- 서버 종료 or 유효시간이 지나면 사라짐



## 사용

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

