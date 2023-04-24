### programming paradigm

- 절차지향적
  - 절차 지향은 **기능중심**으로 바라보는 방식으로, 무엇을 어떤 절차로 할 지에 초점을 맞춰서 설계한다.
- 객체지향적
  - 객체가 데이터와 로직을 수행하는 함수를 가지게 하여 해당 객체들의 상호작용을 통해 프로그램을 설계
  - 내 blog

### DB

- DB Connection
  - 신규 connection 생성은 다소 resource 소요
  - network connection 생성, DB 서버와의 인증, 자원 할당 등 
  - connection.close 전까지 계속 유지됨
- connection pool
  - 매 DB 작업마다 새로운 connection 생성하는 것이 아닌, 재사용될 connection들을 모아 효과적으로 관리
  - 최대 생성 connection 개수보다 초과하여 요청 시, 대기 필요
  - 성능 및 확장성 향상
- Many connection
  - 여러 유저, thread로 부터 동시적으로 db 요청이 올 시 처리하기 위함 => concurrency (동시성)
  - workload 분산

- DB polling
  - 데이터베이스에 주기적으로(일정한 간격으로) 쿼리를 실행하여 데이터베이스의 상태나 내용을 확인하는 기술
  - 실시간 데이터 감지, 배치 작업 실행
- Trigger
  - 데이터베이스 관리 시스템(DBMS)에서 데이터베이스의 특정 이벤트가 발생할 때 자동으로 실행되는 일종의 저장 프로시저(Stored Procedure)
  - 데이터 일관성 유지, 로그 기록
- DB Library
  - DB Driver
    - Hikari
      - JDBC connection pooling library 중 높은 성능
      - 가볍고 효율적인 pool
      - 빠르고, 믿을만한고, 확장성있는 connection pooling => high-traffic 어플리케이션에 사용
  - ORM Framework
    - Hibernate
      - Hibernate is a widely used implementation of the Java Persistence API (JPA)
      - Java 객체와 관계형 DB를 자동으로 매핑.
      - 객체 지향적으로 DB 작업 수행 O
      - Java class가 DB 테이블을 의미하도록 가능
      -  lazy loading, caching, transaction management, and query generation
- DB 종류
  - Relational Database
    - uses a relational model to organize and store data
    -  data is stored in tables with rows and columns, and relationships between tables are established using keys or identifiers
    -  flexibility, scalability, and ability to handle complex data relationships.
  - NoSQL (Not only SQL)
    -  document-oriented databases. ex) MongoDB
    - Key-value stores. ex) Redis
    - 그래프 db
    - 시계열 db
- Mysql vs oracle
  - mysql
    - open-source RDBMS
    - have limitations in handling very large databases or high concurrent loads => small to medium-sized applications
  - oracle
    - commercial RDBMS
    - scalability and performance capabilities (partitioning, parallel processing, and caching) => handling enterprise-level workloads

### Java

- JPA
  - Java Persistence API, which is a Java-based framework for object-relational mapping (ORM)
  - Object-relational mapping, CRUD operations, Querying
- process vs thread
  - process
    - a unit of execution that has its own memory space (heap)
    - 하나의 프로세서는 한번에 스레드 1개만 실행 가능
  - thread
    - a unit of excecution within a process
    - Every thread in a process shares the process's memory
- lock
  - dead lock 
    - two or more threads are blocked indefinitely, waiting for each other to release a resource
    - 조건 
      - 상호 배제
      - 점유 대기
      - 비선점
      - 순환 대기
    - 참고) https://chanhuiseok.github.io/posts/cs-2/
  - Starvation
    - 여러 Thread가 자원을 가질 동안 특정 Thread가 가지지 못하는 경우
  - Live Lock
    - 타 Thread가 활성화 되어 있으면, 해당 자원 양도  => 반복적인 양도로 작업 수행 X 경우
    -  repeated and unsuccessful attempt to pass each other
  - Slipped Condition
    - 멀티 스레드 환경에서 thread가 접근한 자원이 true 상태였지만, 더 이상 자원을 들고 있는게 없는 경우
  - Atomic Action
    - 실행 도중 지연되지 않고 한번에 실행
    - 완벽하게 끝내거나 실행되지 않거나만 존재
- Generics
  - 다양한 타입의 객체를 다루는 메소드, 컬렉션 메서드에서 컴파일 시에 타입 체크
  - specify the type of data that a particular class, interface, or method can work with, at the time of instantiation or invocation, instead of using raw types which can result in less type safety and require explicit type casting.
- 패턴
  - DAO 패턴
    - DAO
      - DB처리를 전문으로 하는 객체
      - 테이블 당 1개씩
    - DTO
      - transferring data between different layers or components of an application, typically across different tiers or services
      - contains only data and has no behavior or business logic.
    - VO
      - represent a value or a piece of data with a specific meaning or behavior 
      - represents a value or a piece of data within the domain model of an application. It may contain behavior, validation logic
  - MVC 패턴
    - Model - View - Controller
    - 소프트웨어 시스템을 세 가지 타입의 컴포넌트로 분할하는 소프트웨어 패턴
    - MVC
      - Model : 비지니스 로직 및 데이터 담당
        - encapsulates the data and provides methods
        - responsible for retrieving data from databases or other data sources, performing business logic operations, and updating the data
      - View : 사용자 인터페이스 담당
        - rendering the user interface and displaying the data
      - Controller : 시스템 흐름제어 담당
        - intermediary between the Model and the View
        - receives input from the user through the View =>  processes it, and updates the Model 
- Pojo
  - Plain Old Java Object
  - a simple Java class that does not rely on any particular framework or technology
- 팩토리 패턴
  - creational design pattern in Java that provides an interface for creating objects in a super class, but allows subclasses to alter the type of objects that will be created.
  - 종류
    - Simple Factory
      - a factory class has a static method that creates objects based on some input or parameter, and returns the created objects to the client.
    - Factory Method
      - a factory interface is defined with a method that creates objects, and concrete factory classes implement this interface and provide their own implementation of the factory method
      -  Each concrete factory class is responsible for creating objects of a specific type or class.
    - Abstract Factory
      - an abstract factory interface is defined with multiple factory methods that create objects belonging to different families or groups of related objects. 
      - Concrete factory classes implement this interface and provide their own implementation of the factory methods to create objects of specific families or groups.



### Spring

- Why Spring

  - Modularity

    - easy management of application components

    - dependency injection, inversion of control (IoC)

      => making it easier to develop and maintain complex applications.

  - Flexibility
    - configuration and customization
    - XML, Java annotations, and Java code-based configurations
  - Robustness
    - provides built-in features for handling common enterprise-level concerns
    - security, caching, validation, transaction management

  - Testing support :  promotes TDD
  - Community and ecosystem

- spring-security

  - 인증(Authentication), 권한 부여(Authorization), 세션 관리(Session Management)

- @Transactional



### 테이블 관계

- N:M mapping 사용 X
  - Complexity
    - use of intermediate tables or junction tables to represent the relationship between two
  - Performance
    - lead to performance issues in JDBC
  - 중복된 데이터 (한 개의 학생이 여러 개의 과목을 수강하는 경우, 학생 정보와 과목 정보가 중복되어 중간 테이블), 복잡한 쿼리



### 정규화

- 제 1 정규형
  - 모든 속성(컬럼)은 원자값(Atomic Value)을 가지며, 모든 속성은 중복이 없는 유일한 값
- 2차 정규화(2NF)
  - 부분 종속(Partial Dependency)을 제거하여 테이블의 키(Primary Key)에 대해서만 의존성
- 3차 정규화
  - 이행적 함수적 종속을 제거하여 테이블의 컬럼 간에 종속성을 최소화



### 개발 방법론

- TDD
- DDD
  - placing a strong emphasis on the domain or business logic of the application. 
  - 주요 특징
    - Ubiquitous Language :  shared language that is understood by both domain experts and developers
    - Bounded Context :  dividing a complex domain into smaller, more manageable bounded contexts
    - Domain Model : 




### Maven

- 소스코드 파일을 컴퓨터에서 실행할 수 있는 독립 소프트웨어 가공물로 변환하는 과정 / 결과물
- Build Tool, Dependency Management, Repository system, Plugin framework, 
- Goals < Phases < Lifecycle



### Jar & War

- Jar
  - used for packaging Java class files, resources, and metadata into a single archive file
- War
  - used for packaging and distributing Java web applications.
  - contains web components such as servlets, JSP (JavaServer Pages) files, HTML, CSS, JavaScript, and other web resources
  - be deployed on a Java web server, such as Apache Tomcat or Java EE application servers





### Proxy

- intermediary or an intermediate entity that acts on behalf of another entity or client to access a resource or perform an action





### 깃

- 병합 종류
  - Fast Forward : master branch가 브랜치가 커밋해 둔 내용을 그대로 따라감.
  - Auto Merge : master와 branch에 서로 다른 commit이 있을 경우, 두개를 합침
  - Manual Merge : master와 branch의 변경 사항 사이에 충돌이 있는 경우, 문제 파일을 수정하고 병합함. 이때는 merge가 아닌 commit으로 합침.



### MSA 환경

- kafaka
  - kafka 두개 종류

- 도커
- Kubernetes
  - Node vs Pod
    - Node
      - A node is a worker machine
      - can be a physical or virtual machine.

    - Pod
      - smallest and simplest unit in the Kubernetes object model and represents a single instance of a running process in a cluster.




### 기타

- Kotlin

- 세션

- CI/CD
- 



기타 도구

- Jenkins
  - Continuous Integration (CI): Jenkins can automatically build and test code changes as they are committed to a version control repository, allowing developers to quickly identify and fix issues early in the development process.

  - Continuous Delivery (CD): Jenkins can automate the deployment of applications to different environments, such as development, staging, and production, ensuring consistent and reliable deployments.

- Jira
- Kibana
- Control Center
- Whatap
- redis



질문거리

- MSA 빅뱅이 아닌 점진적으로 분리 중, 쪼개지는 개수? 목표 시간?

- msa에서 다른 언어 사용 계획
- 테스트 환경 ( 테스트 코드로 대체 / 별도 테스트 결과 보고 절차)
- Kotlin에서 JPA와 같이 ORM 사용?
- 개발 문화
  - 코드 리뷰 / 스터