# 자바 ORM 표준 JPA 프로그래밍 - 기본편



## JPA 소개

### SQL 중심적인 개발의 문제점

- SQL에 의존적인 개발
  - 객체는 일반적으로 관계형 DB에 보관
  - 수 많은 SQL이 필요
  - 자바 객체를 SQL로, SQL을 자바 객체로 바꾸는 작업의 무한 반복

- 패러다임의 불일치 : 객체 vs 관계형 데이터베이스

  - 개요

    - 객체 지향 프로그래밍은 추상화, 캡슐화, 정보은닉,  상속, 다형성 등 시스템의 복잡성을 제어할 수 있는 다양한 장치들을 제공

    - 객체를 영구 보관하는 다양한 저장소 (ex. RDB, NoSQL, FILE, ..) 中 현실적인 대안은 관계형 DB

      => 개발자 ≒ SQL 매퍼

  - 객체와 관계형 데이터베이스의 차이

    1. 상속

       ![image-20231108080124159](JPA소개.assets/image-20231108080124159.png)

       - 객체	: 상속 관계

       - Table  : 슈퍼타입 서브타입 관계

         => Insert 시, 객체를 분해하여 두 개 이상의 테이블에 Insert 

         ​	 Select 시, 조인 SQL을 통해 조회 후 각각의 객체 생성 필요

         :heavy_check_mark: 자바 컬럭센 사용의 경우

         ```java
         // 저장
         list.add(album);
         // 조회
         Album album = list.get(albumId) ;
         Item item = list.get(albumId) ;
         ```

    2. 연관관계

       - 객체 : 참조

         ```java
         // 객체 다운 모델링
         class Member {
          String id; //MEMBER_ID 컬럼 사용
          Team team; //참조로 연관관계를 맺는다. 
          String username;//USERNAME 컬럼 사용
          
          Team getTeam() {
          	return team;
          }
         }
         class Team {
          Long id; //TEAM_ID PK 사용
          String name; //NAME 컬럼 사용
         }
         ```

       - 테이블 : 외래 키

         ```java
         // 객체를 테이블에 맞추어 모델링
         class Member {
          String id; //MEMBER_ID 컬럼 사용
          Long teamId; //TEAM_ID FK 컬럼 사용 
          String username;//USERNAME 컬럼 사용
         }
         
         class Team {
          Long id; //TEAM_ID PK 사용
          String name; //NAME 컬럼 사용
         }
         
         ```

       => 객체 다운 모델링을 할 경우,

       			- Member Insert 시, `Team`의 Id에 대한 별도 접근 필요 `member.getTeam().getId()`
  	
       			- Member Select 시, `Member`와 `Team`을 모두 조회 & 관계 설정 필요

       :heavy_check_mark: 자바 컬럭센 사용의 경우

       ```java
       list.add(member);
       
       Member member = list.get(memberId);
       Team team = member.getTeam();
       ```

    3. 데이터 타입

    4. 데이터 식별 방법

- 문제점

  - 엔티티 신뢰 문제
    - `Member member = memberDAO.find(memberId);` 를 했을 경우, 객체 그래프에서 어디까지 가져오는 지 알 수 없다
  - 모든 객체를 미리 로딩할 수는 없다
    - 한번에 모든 연관관계를 가져오는 쿼리는 성능상 떨어짐
    - 상황에 따라 동일한 회원 조회 메서드를 여러벌 생성
  - 객체 그래프 탐색 제한
    - 객체 그래프 탐색 : 객체는 자유롭게 참조 관계로 이어진 객체 그래프를 탐색 할 수 있어야 함

  => 진정한 의미의 계층 분할이 어렵다. (물리적으론 분리, 논리적으론 엮여있음)



### JPA

- JPA

  - Java Persistence API
  - 자바 진영의 ORM 기술 표

- ORM

  - Object-relational mapping
  - 객체는 객체대로 설계
  - 관계형 DB는 관계형 DB대로 설계
  - ORM 프레임워크가 중간에서 매핑

  - JPA는 애플리케이션과 JDBC 사이에서 동작

    <img src="JPA_기본편.assets/image-20231109080956268.png" alt="image-20231109080956268" style="zoom:67%;" />

- JPA 동작

  - 저장(`persist`)
    - Entity 분석
    - Insert SQL 생성
    - JDBC API 사용
    - 패러다임 불일치 해결
  - 조회(`find`)
    - Select SQL 생성
    - JDBC API 사용
    - ResultSet 매핑
    - 패러다임 불일치 해결

- JPA 소개

  - 개요
    - EJB라는 자바 표준 엔터티의 빈의 불편함에서 Hibernate가 오픈 소스로 개발됨 => 이를 바탕으로 자바 표준 JPA 개발
  - 표준 명세
    - JPA는 인터페이스의 모음
    - JPA 2.1 표준 명세를 구현한 3가지 구현체 : \- 하이버네이트, EclipseLink, DataNucleus

- JPA 장

  - SQL 중심적인 개발에서 객체 중심으로 개발

  - 생산성

    - 저장: `jpa.persist(member)`
    - 조회: `Member member = jpa.find(memberId)`
    - 수정: `member.setName(“변경할 이름”)`
    - 삭제: `jpa.remove(member)`

  - 유지보수

    - SQL 의존적인 개발에서는 엔터티의 필드 변경 시 모든 SQL 수정해야함
    - JPA의 경우, 필드만 수정. SQL은 JPA가 생성

  - 패러다임의 불일치 해결

    - JPA와 상속

      - JPA가 상속과 슈퍼-서브타입 관계의 차이에서 오는 다중 SQL 및 조인 쿼리 처리

      - 저장 : `jpa.persist(album);` => `INSERT INTO ITEM ...` , `INSERT INTO ALBUM ..` 

      - 조회 : `Album album = jpa.find(Album.class, albumId);` 

        => `SELECT I.*, A.* FROM ITEM I JOIN ALBUM A ON I.ITEM_ID = A.ITEM_ID`

    - JPA와 연관관계, 객체 그래프 탐색

      - 연관관계 저장과 객체 그래프 탐색에 있어서 자유로움
      - 신뢰할 수 있는 엔터티, 계층

    - JPA와 비교하기

      - JPA 동일한 트랜잭션에서 조회한 엔티티는 같음(`==`)을 보장
      - 캐시

  - 성능

    - 1차 캐시와 동일성(identity) 보장

    - 트랜잭션을 지원하는 쓰기 지연(transactional write-behind)

      - 트랜잭션을 커밋할 때까지 SQL을 모음

        => 비즈니스 로직 수행 동안 DB 로우 락이 걸리지 않는다.

    - 지연 로딩(Lazy Loading)

  - 데이터 접근 추상화와 벤더 독립성

  - 표준































