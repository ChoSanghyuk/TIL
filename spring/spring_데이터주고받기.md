

# Spring Controller jsp와 데이터 주고받기



## jsp -> controller

​	기본적으로 form 형식으로 데이터를 전송한다. form 태그에서 `action`을 통해 어디로 전송할지 설정하고, `method`를 통해 GET, POST와 같은 방식을 설정한다. 

​	form 태그는 전체적인 전송 목적지와 방식에 대해 설정했다면, 전송할 내용은 form 태그 안에 포함되는 input 이나 button 태그에서 결정된다. name 속성은 전송할 값을 담을 parameter 이름이 되며, value가 전송할 값이 된다. 아래 경우에서는 id를 통해 button을 직접적으로 form안에 넣지 않고 연결시켰으며, 전송할 value를 직접 설정하였다. 


```jsp
<form action="/" method="GET" id="my_form"></form>
<button form="my_form" class="button-shape" name="reservation-date" value="<%=sb %>"><%=d%></button>
```



## controller -> jsp

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



## jsp에서 controller가 보낸 데이터 받기

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