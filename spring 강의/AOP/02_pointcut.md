## Pointcut Expression



## 정의



### Pointcut

- A predicate expression for where advice should be applied
- Spring AOP uses AspectJ's pointcut expression language
- start with execution pointcuts
  - applies to execution of methods



### Pointcut Expression Language

```
execution(modifier-pattern? return-type-pattern declaring-type-pattern? 
			method-name-pattern(param-pattern) throws-pattern?)
```

- ? : optional

- modifiers 

  - Spring AOP only supports public

- return

  - void, String, List<>, ...

- Declaring type

  - the class name

- ex) 정확히 한 클래스의 메소드에만 부여하고 싶을 경우

  ```
  @Before("execution(public void com.aopdemo.dao.AccountDao.addAccount())")
  ```

- ex) 같은 이름의 모든 메소드에 부여하고 싶을 경우

  ```
  @Before("execution(public void addAccount())")
  ```

- ex) add로 시작하는 모든 메소드에 부여하고 싶을 경우 : wildcard

  ```
  @Before("execution(public void add*())")
  ```

  

### wildcard

- 이름, return type 에 사용 가능





























