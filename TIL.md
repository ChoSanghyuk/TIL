https://hgko1207.github.io/2021/06/19/spring-jpa-1/

https://jforj.tistory.com/257



### 한화 과제
- jpaRepository를 통해 기본적인 조회, 삭제 작업을 수행할 수 있다.
- jpaRepository에 @Query를 통해 더 구체적인 작업을 수행할 수 있다.
  - @Query 어노테이션을 사용해서 네이티브 쿼리를 수행하려면 nativeQuery 속성을 true로 한다.
  - In order for JPA to actually run custom @Query which modifies state of the database, the method has to be annotated with @Modifying to tell JPA to use executeUpdate etc.
  - Spring Boot에서 JPA를 사용할 때 update나 delete문을 쿼리로 직접 사용할 경우에는 Transaction 처리를 직접 해주어야 한다. : 직접 호출하는 함수에 **@Transactional** annotation을 추가
- @Scheduled(cron = "* * * * * *") 을 통해 정해진 시간에 함수를 동작 시킬 수 있다. 
  - 이때, run하는 application 클래스에 @EnableScheduling을 추가해야 한다.
- Lob : Large Object의 줄임말로 큰 용량을 저장할 수 있다.



### 221208

- static 변수 초기화



### 230104

- Redis Cache 서버 채번 사용



### 20231018

- String...



- java에서 db 조회 시, fetchSize 설정하면 성능 Up. 



- Stream은 재사용 불가





- a,, 일 경우 ,로 split 해도 길이는 1



