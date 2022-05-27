# @Before Advice



## 설정

### Adding AspectJ JAR File

- Need to download AspectJ JAR file
- Even though we use Spring AOP, still need AspectJ JAR file



### AOP Project Setup

1. download AspectJ JAR file
   - https://mvnrepository.com/artifact/org.aspectj/aspectjweaver
2. lib directory에 붙여넣기
3. Build Path
   - project 우클릭 => properties => Java Build Path => libraries => classpath => Add JARs => lib에서 방금 추가한 Jar







## Develop



### Development Process

1. Create target object
2. Create Spring java Config class
3. Create main app
4. Create an Aspect with @Before advice



### 2. Create Spring java Config class

```java
@Configuration
@EnableAspectJAutoProxy
@ComponentScan("com.~~~")
public class DemoConfig {
    
}
```

- @Configuration
  - Spring Pure Java Configuration
- @EnableAspectJAutoProxy
  - Spring AOP Proxy Support
- @ComponentScan("com.~~~")
  - Component scan for components and aspects
  - Recurse packages



### 3. Create main app

```java
public class MainDemoApp {

	public static void main(String[] args) {
		
		// read spring config java class
		
		AnnotationConfigApplicationContext context 
            = new AnnotationConfigApplicationContext(DemoConfig.class);		
		
		// get the bean from spring container
		AccountDAO theAccountDAO = context.getBean("accountDAO", AccountDAO.class);
		
		// call the business method
		theAccountDAO.doWork();
		
		// call the accountDAO getter/setter methods
		
		// close the context
		context.close();
	}

}
```



### 4. Create an Aspect with @Before advice

```java
@Aspect
@Component
public class MyLoggingAspect{
    
    @before("execution(pulic void addAccount())")
    public void beforeAddAccountAdvice(){
        ...
    }
}
```

- @Aspect
  - this class can run as part of spy network => listen on communications behind the scenes
- execution(~~) 
  - Pointcut expression

















