# Spring Bean



## Bean, Component, Repository, Service

### Bean 

- 사용자가 컨트롤 하지 못하는 Class 또는 외부 라이브러리에 사용

- Method Level에서 적용

  => @Configuration : 스프링에게 해당 클래스가 Bean을 생성하는 클래스임을 알려줌



### Component

- Class 자체를 빈으로 등록
- Spring 개발에 있어서 일반적으로 Bean을 등록함에 사용됨
- @Respository와 @Service는 특별한 경우의 @Component

### Repository

- DB쪽 데이터를 처리를 위한 Data Repository, DAO 클래스임을 명시함

- Unchecked Exception을 Spring의 Runtime 예외인 DataAccessException으로 처리

  => 데이터 예외 처리에 이점을 둠

### Service

- 비지니스 로직을 가지고 있음을 명시함
- service layer 이외에는 거의 사용되지 않음



## @Controller와 @RestController 비교




### Controller와 RestController의 차이점

- 목적에서 차이점 존재
  - Controller는 View를 반환
  - RestController는 View를 거치지 않고 Data를 반환

### @ResponseBody

- Controller을 사용해서 데이터 반환 O
- @ResponseBody를 사용하면 Springdptj HTTP 응답에 값을 자동으로 변환
- 다만, 모든 메소드에 작성 필요

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



## @Component Bean 수동 주입



### 기본 동작

- Bean의 주입은 IOC Container가 자동으로 수행

​	=> 수동 주입이 필요한 경우에는 ApplicationContextProvider  이용



### 수동 주입

1. ApplicationContextProvider 생성

```java
@Component
public class ApplicationContextProvider implements ApplicationContextAware{
    
    private static ApplicationContext applicationContext;
    
    @Override
    public void setApplicationContext(ApplicationContext ctx) throws BeansException {
        applicationContext = ctx;
    }
    
    public static ApplicationContext getApplicationContext() {
        return applicationContext;
    }

}
```

2. BeanUtils 생성

```java
public class BeanUtils {
    public static Object getBean(String bean) {
        ApplicationContext applicationContext = ApplicationContextProvider.getApplicationContext();
        return applicationContext.getBean(bean);
    }
}
```

3. getbean("빈 이름")

```java
TestSerive testService = (TestSerive) BeanUtils.getBean("testService");
```



## Bean 명칭으로 가져오기



### BeanUtils

```java
BeanClass samplebean = (BeanClass) BeanUtils.getBean("빈이름")
```

- BeanUtils를 사용하여 bean 직접적으로 참조 가능



### 빈 이름 등록

```java
@Service("빈이름")
```

- Bean 등록 시에 어노테이션과 함께 이름 등록 가능





