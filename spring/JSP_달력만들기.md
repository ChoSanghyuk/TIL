# JSP로 달력 구현하기



​	Spring 프로젝트로 예약 사이트를 만들던 중, 달력을 구현할 필요성을 느꼈다. 단순히 html table로 정적으로 만드는 것이 아닌, 달이 변함에 따라 자동으로 달력이 바뀌는 페이지가 필요하여 jsp로 동적으로 구현해보고자 한다. jsp에 대한 지식이 없어서 유튜브를 따라하면서 구현했으며, 그 과정에서 배운 내용을 작성하고자 한다.



## JSP란

​	JSP는 JavaServer Pages의 약자로, HTML에 JAVA 코드를 넣어 웹페이지를 동적으로 만들어 주는 도구이다. JSP는 서버로 요청시 Servlet 파일로 변화되어 동적 기능을 작업할 수 있게 된다. (Servlet : 웹페이지를 동적으로 생성하게 하는 서버측 프로그램) Servlet은 jsp 태그와 html을 분리하고, jsp 태그의 내용에 따라 html을 변환하여 반환하게 된다.



## 달력 만들기



### java의 Calendar 인스턴스 생성하기

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8" %>
<%@ page import="java.util.Calendar"%>

<%

Calendar cal = Calendar.getInstance();
int y = cal.get(Calendar.YEAR);
int m = cal.get(Calendar.MONTH);

cal.set(y,m,1);
int dayOfWeek = cal.get(Calendar.DAY_OF_WEEK); // 일:0 ~ 토:7
int lastDay = cal.getActualMaximum(Calendar.DATE);

%>
```

- <% %>은 `스크립 트릿`으로, JAVA의 코드를 삽입할 수 있게 해준다. 
- Calaendar의 인스턴스를 생성해 준 후, 달력을 현재 연, 월로 세팅을 해준다.
- 현재 달의 시작 `요일`과 끝`일`을 각각 변수에 저장해 둔다. 요일은 일 ~ 토로, 각각 0 ~ 7의 숫자와 대응된다.



### for문으로 달력 생성

```jsp
	<table>
		<caption><%=y %>년 <%=m+1 %>월</caption>
        <!--각 요일별로 테이블 head를 만든다. -->
		<tr>
			<th>일</th>
			<th>월</th>
			<th>화</th>
			<th>수</th>
			<th>목</th>
			<th>금</th>
			<th>토</th>
		</tr>
		<tr>

		<%
		int count = 0;
		// 달력의 시작 요일 전은 공란으로 채워준다 
        // 예를 들어, 이번 달의 시작이 수요일이면 앞의 일,월,화 자리는 공란을 둔다.
		for(int s=1 ; s < dayOfWeek ; s++){
			out.print("<td></td>");
			count++;
		}
		
        // 1일부터 마지막 요일까지 달력 칸을 작성하고, count 변수를 통해 7개 간격으로 줄 띄움을 한다. 
		for(int d = 1 ; d<=lastDay ; d++) {
			count++;
		%>
			<td><%=d %></td>
		<%
			if( count%7 == 0){
				out.print("</tr><tr>");
			}
		}
		%>
		</tr>
	</table>
```

- <%=  %>은 `표현식`으로, 결과값을 출력해 준다.
- out.print 구문을 통해 특정 조건 마다 html 태그를 반환할 수 있다.



## 전체 코드

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8" %>
<%@ page import="java.util.Calendar"%>

<%

Calendar cal = Calendar.getInstance();
int y = cal.get(Calendar.YEAR);
int m = cal.get(Calendar.MONTH);

cal.set(y,m,1);
int dayOfWeek = cal.get(Calendar.DAY_OF_WEEK); // 일:0 ~ 토:7
int lastDay = cal.getActualMaximum(Calendar.DATE);

%>

<!DOCTYPE html>
<html>

<head>
	<title>SH hair Reservation</title>
</head>
<style>
	body {
		font-size:9pt;
		color: #55555;
	}
	table {
		border-collapse:collapse;
	}
	th, td {
		border : 1px solid #cccccc;
		width : 50px;
		height : 25px;
		text-align : center;
	}
	
	caption {
		margin-bottom : 10px;
		font-size : 15px;
	}

</style>
<body>
	<table>
		<caption><%=y %>년 <%=m+1 %>월</caption>
		<tr>
			<th>일</th>
			<th>월</th>
			<th>화</th>
			<th>수</th>
			<th>목</th>
			<th>금</th>
			<th>토</th>
		</tr>
		<tr>

		<%
		int count = 0;
		
		for(int s=1 ; s < dayOfWeek ; s++){
			out.print("<td></td>");
			count++;
		}
		
		for(int d = 1 ; d<=lastDay ; d++) {
			count++;
		%>
			<td><%=d %></td>
		<%
			if( count%7 == 0){
				out.print("</tr><tr>");
			}
		}
		%>
		</tr>
	
	</table>
	
</body>

</html>
```

