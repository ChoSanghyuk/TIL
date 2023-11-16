# MSA



## MSA 개요

### MSA 등장배경

- 복잡성 / 영향도
- 배포
- 확장 / 장애
- 기술 적합성

### MSA 정의

### MSA 목적

- Business Agility (민첩성)
  - Cloud + DevOps 조직 뒷받침 필요
- Project 단위 개발 ⇒ Product 중심 개발
  - 하나의 팀에 해당 Business 개발하는데 필요한 모든 역할 구성

### MSA 장점 / 특징 / 고려사항

- 관점
  - 물리적 분리 / 독립
  - 배포
  - 장애
  - 확장
  - 기술 / 환경 / 조직

### MSA 단점

- 관점
  - 성능
  - 테스트
  - 트랜잭션
  - 데이터 조합
  - 복잡도

### SOA vs MSA

| SOA                 | MSA                        |
| ------------------- | -------------------------- |
| 전사적 Architecture | 프로젝트 중심 Architecture |
| 재사용 우선         | 독립성 우선                |
| 서비스 통합         | 서비스 분산                |
| SOAP(heavey)        | Json (Light)               |



## MSA 모델링

### 1. LG CNS MSA 방법론

### 2. 서비스 모델링 절차



## 서비스 정의

### 1. 다양한 서비스 분리 기준

#### Domain Driven Design

- 서비스 분해하는 방법 중 하나로 DDD 개념 사용
- Bounded Context
  - 업무의 독립 단위
  - 서비스 예상 단위
- Shared Kernel
  - 두 개 이상의 모델에서 공유되는 모델 / 데이터
  - 같이 설계 / 협의
- Aggregation Root
  - 외부에서 객체 참조(CRUD) 할 때에는 ROOT 통해서 접근
  - 내부 숨김
- 모델
  - Highly Cohesive
  - Loosely Coupled

#### 업무 기능 관점

- 중복되지 않고 R&R을 명확히 할 수 있는 단위로 분리

#### Granularity

- Business Context에 맞게 독립적으로 개발 / 배포할 수 있는 단위로 결정

#### Microservice 크기

- 기준
  - 업무중심
  - 확정성 / 장애대응 / 독립성 / 배포
  - 서비스 기능
    - 최소한의 API 노출
    - Library 공유 / 유사 기능은 하나의 서비스로 통합
  - 변경 가능성 / 성능
  - 트랜잭션과 일관성
  - 최적의 기술
  - 조직


### 2.서비스 정의 절차

- 서비스 크기 타당성 검증



### 3.후보 서비스 도출

#### 후보 서비스 도출 절차

- Stakeholeder ( 업무 전문가, IT 담당자, 분석 설계자) 모여서 선정 / 도출

#### 후보 서비스 도출 기법

#### 후보 서비스 도출 사례



### 4.후보 서비스 평가

#### 후보 서비스 평가 절차

#### 평가항목 정의

#### 평가항목별 측정방법 도출

#### 후보 서비스 평가 / 조정



### 5.후보 서비스 연관도 시뮬레이션

#### 후보 서비스간 호출관계 분석

#### 후보 서비스간 테이블 의존성 분석

- 서비스 조인쿼리 복잡도 분석
- R 관계 : 별도 조인쿼리 복잡도 분석 ⇒ 통합 고려

#### 도구를 활용한 시뮬레이션

#### 시뮬레이션을 통한 서비스 조정




### 6.서비스 정의서 작성

#### 서비스 정의서

- 관련 Entity
  - 해당 서비스가 직접 CRUD 하는 Owner Entity
- 관련 기능
  - 기능분해도 최하위 Level인 Elementary Process 등을 바탕으로 정의
  - API 로 노출될 가능성 높음



## 서비스 설계

### 1. 기존 설계와의 차이점

- 각 서비스의 설계 / 개발을 독립적으로 수행 + 서비스 자체 아키 가지는 Polyglot 적용 O
- Polyglot
  - 단일 개발 표준에 국한 X
  - 서비스 별 특징에 맞게 최적화된 환경 적용

### 2. API First Design

#### API 설계 절차

- API First Design
- 도출 순서
  1. 서비스 식별
     - 주로 Level 2 기준 식별
  2. 컴포넌트 식별
     - Level 3 기준 식별
     - 서비스 정련 과정 ⇒ 통합 / 분리
     - DAO / SVI / DI
  3. 오퍼레이션 식별
  4. API 식별
  5. 서비스 간 상호작용

#### 컴포넌트 종류

- DAO
  - DATA 접근 ( 조회, 저장) 역할
- SVI
  - Transaction 관리
- DI
  - 타 서비스에 제공 API 정의

#### API 식별

- 구분
  - Provider 주도 식별
  - Consumer  주도 식별
- 빙산(iceberge) 형태 서비스
  - API
    - 추상화 높음
    - 최소한 노출
    - 변경 최소화 ⇒ 안정적 API
  - Implementation
    - 캡슐화
    - 정보 은닉
    - API 변경 최소화하면서 구현체 수정

#### 서비스 정련

- API 추가 / 변경 사유
  - API간 상호작용
  - 분산 트랜잭션 (보상 트랜잭션)
  - 분산 데이터 조회
  - 요구사항 / 화면 및 보고서 설계



### 3. 동기 / 비동기 호출

#### 비동기 호출

- 단방향 통보
- 요청 / 비동기 응답
  - 로직 중간에 비동기로 타 서비스 호출 ⇒ 응답 대기 ⇒ 응답에 따라 후속 작업 수행
  - 응답 기다리지 않고 작업 O / 비동기 요청 대기 설계
- 발행 / 구독



### 4.분산 트랜잭션

:bulb: Tip

- 관계형 모델 X시, 테이블간 조인 기능 없음
- NoSQL

#### 분산 트랜잭션 필요 이유

- 데이터 정합성 유지
  - ACID 대안으로 BASE 개념 필요
- 보상 트랙잭션 이용한 SAGA 패턴 적용 O

#### ACID vs BASE

- ACID
  - 원자성
    - 트랜잭션이 모두 반영 or 모두 반영 X
  - 일관성
    - 처리 결과가 항상 일관
  - 독립성
    - 둘 이상의 트랜잭션 발생 시, 서로 영향 X
  - 지속성
    - 성공적 완료 시, 영구적으로 반영
- BASE
  - BA (Basically Available)
    - 항상 가용성 중시
  - S (Soft-State)
    - 노드 상태(DB 상태값)는 내부 정보가 아닌 외부 전송 데이터에 결정
  - E (Eventually Consistency)
    - 실시간은 아니여도, 최종적 데이터 일관성 유지

#### 보상 트랜잭션

- 기존의 트랜잭션 무효화
- 순방향 트랜잭션의 역순으로 보상 트랜잭션 실행
- Rollback 기능 반드시 설계
  - API를 쌍으로 설계
- 보상 트랜잭션 실패 시 로직
  - 재호출, 실패 로그, 배치 등

#### SAGA

- 분산 트랜잭션 대신 사용할 수 있는 매커니즘
- Orchestration
  - Command 기반
  - 중앙 통제 하, 타 서비스 Local Transaction 호출
  - 여러 개의 Service API / Operation 묶어서 새로운  API 생성
- Choreography
  - Event 기반
  - 각 Local Transaction 완료 시, 이벤트 PUB ⇒ 타 서비스 실행
  - 서로 처리 로직 알 필요 X
  - 추적 힘듦 / 처리 step 변경 시 혼란

#### 장단점

|      | Orchestration                                           | Choreography                                               |
| ---- | ------------------------------------------------------- | ---------------------------------------------------------- |
| 장점 | 구현 / 테스트 쉬움<br />순환 종속성 감소                | 결합도 낮음<br />각자 역할만 충실                          |
| 단점 | 중앙에 많은 로직<br />추가 서비스 필요<br />높은 결합도 | 이해 어려움<br />순환 종속성 증가<br />추적 / Debug 어려움 |



### 5. 분산 데이터 조회

#### 분산 데이터 조회 어려움

- Monolith / Single Service
  - Join
  - DB Link
- 분산 데이터 조회
  - API Composition
  - CQRS

#### API Composition

- 여러 API 결과 조합 ⇒ Response
- 비동기로 병렬 호출
- 응답 속도에 따라 성능 갈림
- 대량 / 복잡한 데이터에 적절 X

#### CQRS (Command Query Responsibility Segregation)

- Command(저장)과 Query(조회) 책임 분리
- 서비스 간 데이터 동기화 / 복제는 Event-Message Queue 활용
- 장점
  - 개별 확장성
  - 성능
  - 독립 서비스
- 데이터 복제 방식 유형
  - 데이터 복제 ⇒ 대량 자료 조회 유리
  - 특정 시간안에만 동기화해도 문제 X 시 사용
  - 유형
    - CDC (Changed Data Capture)
      - Entity Copy
    - Batch / ETL
      - 주기적 대량 데이터 복제
    - Message Queue
      - MSA 환경 사용
- Shared kernel 설계
  - 서비스 간 데이터 공유 유형 정의

| 설계방법      | 자료복제 | 적시성 | 대량자료발생 | 대량자료조회 | 응답성 | 기타  |
| ------------- | -------- | ------ | ------------ | ------------ | ------ | ----- |
| Shared DB     | X        | 상     | 적합         | 적합         | 상     |       |
| DB Link       | X        | 상     | 적합         | 낮음         | 중     | 권장X |
| CDC/Replica   | O        | 중     | 중간         | 적합         | 상     |       |
| Message Queue | O        | 중하   | 중간         | 적합         | 상     | MSA   |
| Batch/ETL     | O        | 하     | 적합         | 적합         | 상     |       |
| Cache         | X        | 중     | 낮음         | 중간         | 상     |       |
| Service API   | X        | 중     | 중간         | 낮음         | 중     |       |

- NoSQL 사용
  - 장점
    - 성능 좋고, 확장성 우수
    - 풍부한 데이터 모델
    - 스키마 제한 없음
  - 단점
    - Transaction 처리 제한
    - Query 제한

#### API Composition / CQRS

|      | API Composition                                    | CQRS                                                         |
| ---- | -------------------------------------------------- | ------------------------------------------------------------ |
| 장점 | 단순 / 이해 쉬움<br />Traffic이 적을 때 유리       | Command와 Query 분리<br />   ⇒Model이단순<br />독립적 서비스 O<br />대량 데이터 처리 각 서비스 구현 로직에 집중<br />Polyglot O |
| 단점 | 모든 서비스가 가용 상태<br />응답 속도에 따라 성능 | 영향도 및 Debug 어려움<br />일시적 정합성 맞지 X             |



## 5. MSA 고려사항

### 5.1 Refactoring

#### Frontend와 Backend 분리

- Frontend와 Backend 독립적으로 개발 / 확장 가능

#### 점진적 개선

- Incrementally Refactoring (점진적 개선)이 효율적
- 하나의 큰 Application을 조금씩 쪼개가는 방법
- Strangler Pattern

#### 서비스 분리 절차

1. 분리 Module 선정
2. 다른 Module과의 Decoupling
   - API로 연결되도록 분리
3. 테이블 분리 (Join 제거)
4. 분리 Module용 Service 생성
5. New Service 준비에서 운영으로 전환
   - Client Request를 New Service로
   - Monolith와 통신 연결
   - Table은 아직 Monolith
6. New Table 생성
7. 데이터 동기화 후 New Table 사용
8. Monolith에서 기존 코드 / 데이터 삭제



### 5.2 Anti Pattern

#### Data-Driven Migration Anti Pattern

- 데이터 위주 전환 = 서비스 기능과 데이터 함께 전환
- 문제 : Too Many Data Migration
- 해결 : Functionality First, Data Last

#### Timeout Anti Pattern

- 문제 : Timeout 활용으로 과도한 응답대기 발생
- 해결 : Circuit Breaker 사용
  - 특정 시스템에 일정 횟수 이상 에러 ⇒ Circuit Open ⇒ 해당 시스템 호출 X

#### The “I Was Taught To Share” Anti Pattern

- 문제 : Too Many Dependency
- 해결 : Replication ~> Dependency  회피
  - 복제를 통해 Bounded Context 형태 유지
  - 변경 거의 없는 안정적인 공유 모듈에만 적용
  - DRY 원칙 위반

#### Reach-in Report Anti Pattern

- 문제 : 보고서를 위한 데이터 신속 조회 / 서비스 경계 유지 어려움
- 해결 : Asynchronous Event Pushing (데이터 펌프)
  - 데이터 업데이트를 비동기 전송 ⇒ 수집 서비스가 보고서 DB 업데이트

#### 기타 권고안

- 서비스간 Interaction을 바탕으로 서비스 단위가 너무 작아지지 않게함
- Library 공유 / 유사 로직 서비스들은 통합 고려
- DB는 하나의 서비스에만 접근하며, 타서비스는 API 호출
- API는 Iceberg와 같이 최소한만 노출



## LG CNS의 MSA

### MSA 아키텍처 구성요소

#### MSA Components

- API / Edge Gateway
  - Routing
  - 외부 API  요청 ~> 내부 API
- Service Mesh
  - 내부 서비스간 통신 관리 및 운용을 위한 서비스 제공
  - 서비스간 통신
- Container Mgmt
  - 서비스 실행을 위한 Container 기반 환경
- Backing Service
  - 서비스간 비동기 통신 / 이벤트 전달
  - Message Queue / MOM (Message-Oriented Middleware)
- CI / CD
  - 소스 ⇒ Commit ⇒ 배포까지의 자동화 절차와 도구
  - CI : 소스관리 ~ 빌드 ~ 릴리즈
  - CD : 배포
- Telemetry
  - Diagnostics / Monitoring
  - 다수 분산 서비스의 실행 상태 / 로그 / 추적 / 분석 도구
  - 모니터링 , 통보, 로깅, 추적, 분석

:bulb:  Release

- 배포할 준비가 된 제품의 스냅샷

#### API / Edge Gateway

- 모든 client 요청의 진입점
- 인증 / 인가, 통합 모니터링, Load Balancing, Message Format 변환, Logging

#### Service Mesh

- Library 방식 ( AS-IS )
  - 종속성, 복잡도
  - 서비스 IP 변경, 호출 방식 변경 등 대응 어려움
- 외부 Proxy 위임
  - 서비스는 통신을 위한 수정 X
  - Service Discovery, Routing, Load Balancing, Circuit Breaker