# Bean, Component, Repository, Service 비교



​	Spring 프로젝트를 진행하던 중, 객체에 Autowired 시키는 방법이 여러 가지 등장함을 발견했다. 분명 모두 비슷하게 객체에 자동으로 등록되는 역할을 하는데 왜 굳이 annotation이 구분되어 있는 걸까? 그 차이에 대해 찾아보고 비교해보고자 한다.

​	각각의 특징은 다음과 같다.



## Bean 

- 사용자가 컨트롤 하지 못하는 Class 또는 외부 라이브러리에 사용

- Method Level에서 적용

  => @Configuration : 스프링에게 해당 클래스가 Bean을 생성하는 클래스임을 알려줌



## Component

- Class 자체를 빈으로 등록
- Spring 개발에 있어서 일반적으로 Bean을 등록함에 사용됨
- @Respository와 @Service는 특별한 경우의 @Component



## Repository

- DB쪽 데이터를 처리를 위한 Data Repository, DAO 클래스임을 명시함

- Unchecked Exception을 Spring의 Runtime 예외인 DataAccessException으로 처리

  => 데이터 예외 처리에 이점을 둠



## Service

-  비지니스 로직을 가지고 있음을 명시함

- service layer 이외에는 거의 사용되지 않음

  
