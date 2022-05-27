# Spring Boot JSP 연동하기



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

