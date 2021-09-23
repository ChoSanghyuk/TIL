# jSP overview



## jsp 중복 사용

```
<jsp:include page="<경로>" flush="false"/>
```

- 해당 위치에 경로에 있는 jsp 파일이 삽입된다.
- parent 파일과 같은 경로에 있는 파일이라면 './이름.jsp'로 접근 가능하다
- flush란, 지정한 JSP 페이지를 실행하기 전에 출력 버퍼를 비울지 지정한다. (true시 비움) 일반적으로 false가 권장되는데, true시 나중에 헤더 정보가 변경될 시 반영할 수 없기 때문이다.
- 위의 방법은 동적 include로, 부모가 호출될 시 include 파일이 삽입된다. => 부모와 include 페이지는 변수를 공유하지 x
- 경로는 현재 파일을 기준으로 상대경로로 작성



## 로그인 정보
### 유저 정보 받아오기

```
<%= request.getSession().getAttribute("currentUser"); %>
```
### 로그인 여부 확인
```
HttpSession session = request.getSession(false);
if (session == null || session.getAttribute("loggedInUser") == null) {
    // user is not logged in, do something about it
} else {
    // user IS logged in, do something: set model or do whatever you need
}
```

