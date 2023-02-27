# JSP




## Spring Boot JSP 연동하기


​	단순하게 controller 함수에 request mapping하고 jsp 파일을 리턴시키면 된다고 생각하고 실행한 결과, 원하는 대로 jsp 화면이 브라우저에 뜨는 것이 아닌 단순히 jsp 파일이 다운되는 것을 경험하였다. 이를 해결하기 위해 찾아본 결과 내가 원하는 것처럼 jsp 페이지를 띄우기 위해서는 몇가지 추가 작업이 필요하다. 



## Step 1. Dependency 추가하기

​	프로젝트의 pom.xml 파일에 jsp 연동을 위한 dependency 추가가 필요하다. 다음과 같은 dependency를 추가해 주자

```xml
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>jstl</artifactId>
</dependency>
<dependency>
    <groupId>org.apache.tomcat.embed</groupId>
    <artifactId>tomcat-embed-jasper</artifactId>
</dependency>
```



## Step 2. 경로 설정

​	어느 경로에서 jsp 파일을 불러올 지 설정이 필요하다. spring boot에서 어느 위치에서 jsp 파일을 불러오는 지도 어느 정도 자동화가 되어 있어서, 이러한 설정이 따로 필요하다고 한다. spring이 우리의 jsp 파일을 잘 찾을 수 있도록 경로를 application.properties 파일에 설정해 두자.

```properties
#JSP path
spring.mvc.view.prefix=/WEB-INF/jsp/
spring.mvc.view.suffix=.jsp
```

​	참고로 prefix의 기본 값은 `src/main/webapp/` 이다. 해당 위치에 `/WEB-INF/jsp/` 폴더를 생성하고 그 안에 가져오고 싶은 jsp 파일을 넣어두면 된다.



## Step 3. controller 함수와 연동하기

​	이제 준비는 끝났다. mapping 된 controller 함수의 리턴 값으로 원하는 jsp 파일의 이름을 반환하면 된다. 

```java
@Controller
public class BasicControlloer {

	@GetMapping("/")
	public String showCalendar() {
		return "calendar";
	}
}
```





## JSP로 달력 구현하기

### Intro
​	Spring 프로젝트로 예약 사이트를 만들던 중, 달력을 구현할 필요성을 느꼈다. 단순히 html table로 정적으로 만드는 것이 아닌, 달이 변함에 따라 자동으로 달력이 바뀌는 페이지가 필요하여 jsp로 동적으로 구현해보고자 한다. jsp에 대한 지식이 없어서 유튜브를 따라하면서 구현했으며, 그 과정에서 배운 내용을 작성하고자 한다.


### JSP란

​	JSP는 JavaServer Pages의 약자로, HTML에 JAVA 코드를 넣어 웹페이지를 동적으로 만들어 주는 도구이다. JSP는 서버로 요청시 Servlet 파일로 변화되어 동적 기능을 작업할 수 있게 된다. (Servlet : 웹페이지를 동적으로 생성하게 하는 서버측 프로그램) Servlet은 jsp 태그와 html을 분리하고, jsp 태그의 내용에 따라 html을 변환하여 반환하게 된다.



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



### 전체 코드

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



## Spring Controller jsp와 데이터 주고받기



### jsp -> controller

​	기본적으로 form 형식으로 데이터를 전송한다. form 태그에서 `action`을 통해 어디로 전송할지 설정하고, `method`를 통해 GET, POST와 같은 방식을 설정한다. 

​	form 태그는 전체적인 전송 목적지와 방식에 대해 설정했다면, 전송할 내용은 form 태그 안에 포함되는 input 이나 button 태그에서 결정된다. name 속성은 전송할 값을 담을 parameter 이름이 되며, value가 전송할 값이 된다. 아래 경우에서는 id를 통해 button을 직접적으로 form안에 넣지 않고 연결시켰으며, 전송할 value를 직접 설정하였다. 


```jsp
<form action="/" method="GET" id="my_form"></form>
<button form="my_form" class="button-shape" name="reservation-date" value="<%=sb %>"><%=d%></button>
```



### controller -> jsp

​	jsp에서 보낸 값을 받는 방식에는 여러가지가 있지만, 기본적으로 Request를 활용해 받을 수 있다. `HttpServletRequest` 변수를 메소드를 통해 받고, getParameter("<파라미터이름>") 메소드를 통해서 jsp에서 보낸 값을 받을 수 있다. 여기서 주의할 점은, jsp에서 보낸 name 속성의 값을 그대로 사용해야 한다.

​	이후, controller에서 jsp로 다시 값을 보내고 싶은 경우에는 Model를 활용해서 보낼 수 있다. 마찬가지로 Model 변수를 매핑 메소드를 통해 받고, addAttribute("<속성이름>", "<값>")를 통해서, 보내고 싶은 데이터를 모델에 저장한다. 이후 jsp 페이지를 반환할 때, 모델이 자동으로 데이터를 전송해 준다.

```java
@Controller
public class BasicControlloer {

	@RequestMapping("/")
	public String showCalendar() {
		return "calendar";
	}
	
	@GetMapping("/")
	public String send(HttpServletRequest request, Model model) {
		String date = request.getParameter("reservation-date");
        
		model.addAttribute("return-value", "hi");
		return "calendar";
	}
}
```



### jsp에서 controller가 보낸 데이터 받기

​	jsp에서 controller가 보낸 값을 받으려면 다음의 코드를 사용하면 된다. 

 1. java 코드를 사용하는 경우

    getAttribute()에 model에 입력한 attribute 이름을 대입하면, 대응되는 값을 얻을 수 있다. (참고로 request.getParameter()를 통해서는 처음에 jsp에서 controller에 보낸 값도 받아올 수 있다. )

```jsp
<%
	String seletedDate = (String) request.getAttribute("return-value");		      	
%>
```

2. html 내에서 접근하는 경우

```jsp
${"return-value"}
```

​	
