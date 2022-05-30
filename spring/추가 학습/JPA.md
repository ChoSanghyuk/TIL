# JPA



## 개념



### JPA 개념

- JPA : Java Persistence API
- 자바 진영에서 ORM 기술 표준으로 사용되는 인터페이스 모음
  - ORM : 기술적으로 어플리케이션 객체를 RDB 테이블에 자동으로 영속화



### JPA 장단점

- 장점
  - SQL문이 아닌 method로 DB 조작 O => 객체 모델을 이용해 비지니스 로직에만 집중 O
  - 객체지향적인 코드 작성 O
  - 테이블 맵핑 및 쿼리 작성 자바 코드 => DB 의존도 낮음 (연결 DB 변경이 쉬움)
- 단점
  - 규모가 크고 복잡해지면 속도 저하 및 일관성 손상 문제 O
  - 복잡한 Query는 SQL문 보다 속도 느려짐



## 연관관계 매핑



### 다중성

- 다대일
- 일대다
- 일대일
- 다대다



### 단방향, 양방향

- 테이블 : 외래키 하나로 조인 O => 양방향 가능

- 객체 : 참조 필드를 가지고 있는 객체만 연관 객체 조회 O

  => 한쪽만 참조 : 단방향 / 양쪽이 참조 : 양방향



### 연관관계 주인

- 외래키를 가지는 쪽이 주인
- 주인은 `@mappedBy` 속성 사용 X
- 1 : N 관계에서 외래키는 항상 N쪽에 있음 => 양방향의 경우 N쪽이 주인
- c.f. 다중성은 왼쪽을 연관관계의 주인으로 정함



### Annotation

- `@Entity`
  - 해당 클래스 이름으로 자동으로 DB 테이블과 매핑
  - (name = "~") : 이름 지정 O
- `@Table`
  - 관계 관점에서 부르는 것이 테이블
  - @Entity의 이름이 기본값
  - 테이블 이름 ~> SQL
- `@Id`
  - 주키 매핑
  - primitive 및 wrapper 타입 사용 O
    - wrapper 타입 사용 시 null 값 O
- `@GeneratedValue`
  - 주키 생성 방법
  - strategy
    - GenerationType.IDENTITY : id가 null 일 때 AUTO_INCREMENT
- `@Column`
  - unique, nullable, length, columnDefinition, insertable = false, updatable = false 지정 O
- `@JoinColumn`
  - foreignkey 명시
  - name = '~' 으로 외래키의 컬럼명 지정 O 
- `@Transient`
  - 컬럼으로 매핑 X 변수
- `@OneToMany`
  - 1:N 관계에서 1임을 명시
  - mappedBy = "N인 Entity의 변수명" 으로 주인 명시

- `@ManyToOne`
  - 1:N 관계에서 N(주인)임을 명시