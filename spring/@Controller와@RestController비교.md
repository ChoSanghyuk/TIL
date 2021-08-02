# @Controller와 @RestController 비교



​	유튜브를 찾아보며 스프링부트 프로젝트를 진행하고 있는 데, 기본적인 mapping에서 오류가 생겼다. 분명 강의에 나온 그대로 따라했는 데도 불구하고, 나만 mapping이 안 되는 것이다. 

```java
@Controller
public class DemoController {

	@RequestMapping({ "/hello" })
	public String hello() {
		return "Hello World";
	}
}
```

​	아주 기본적인 매핑 코드이지만, 마주하는 건 Whitelabel Error Page... 404에러가 뜨는 것을 보아 페이지와 매핑 자체가 이루어지고 있지 않음을 알 수 있었다. 경로 확인 , ComponentScan , 자바 버전 확인 등 모두 해보았지만 실패하였다. 그러던 중, 이전 수업 정리 파일을 보니 @RestController로 되어 있는 것을 확인하였다. @Controller에서 @RestController로 바꾸자 바로 오류 해결. 이 둘은 무슨 차이가 있는 것일까



## Controller와 RestController의 차이점

​	결론부터 말하면, 이 둘은 `목적`에서 차이점이 있다. Controller는 View를 반환함에 목적이 있고, RestController는 View를 거치지 않고 데이터를 반환함에 목적이 있다. 즉, 다음과 같은 상황에서 나는 단순히 "Hello World" 라는 문자열을 반환하려 했음으로 RestController가 목적에 더 맞았던 것이다.

​	Controller을 사용해서도 데이터를 반환할 수 있다. @ResponseBody를 사용하면 Springdptj HTTP 응답에 값을 자동으로 변환해 준다고 한다. 다만, 모든 메소드에 일일히 다 작성을 해주어야 하기에 RestController을 사용하는 것이 더 편리하겠다. 

```java
// @ResponseBody 사용
@Controller
public class DemoController {

	@RequestMapping({ "/hello" })
	public @ResponseBody String hello() {
		return "Hello World";
	}
}
```

### 정리 

- @Controller => View 반환
- @RestController => Data 반환


