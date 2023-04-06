## Database

- 데이터의 체계화



## RDB

### RDB

- Relational Database
- `키`와 `값`들의 `관계`를 표 형태로 정리한 Database
- 장점들
  - 중복 최소
  - 무결성
  - 일관성
  - 독립성
  - 표준화
  - 보안



### 용어 정리

- 스키마
  - DB에서 구조, 표현방법, 관계 등 **명세를 기술**한 것
- 테이블
  - 열, 행의 모델을 사용한 데이터 요소 집합
- 열
  - 고유한 데이터 형식 지정
- 행
  - 실제 데이터
- 기본키
  - 각 행의 고유 값



## RDBMS

### RDBMS

- Relational Database Management System
- RDB 관리 시스템
- ex) MySQL, SQLite, ORACLE, ...



### SQLite

- 보편성은 떨어지나, 비교적 **가벼운** 데이터 베이스

- 서버 형태가 아닌 파일 형식

  

## SQL

### SQL

- RDBMS의 데이터 관리를 위한 특수 목적 프로그래밍 언어
- DB의 스키마 생성 및 수정
- 자료 검색 및 관리



### SQL 분류

| 분류 |              개념              |              예시               |
| :--: | :----------------------------: | :-----------------------------: |
| DDL  | 테이블, 스키마를 정의하기 위함 |       CREATE, ALTER, DROP       |
| DML  |         CRUD 작업 수행         | INSERT, SELECT, UPDATE, DELETE  |
| DCL  |       사용자의 권한 제어       | GRANT, REVOKE, COMMIT, ROLLBACK |



