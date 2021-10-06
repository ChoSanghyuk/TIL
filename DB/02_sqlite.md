## CRUD

### DB 생성

```
$ sqlite3 <파일이름> 
sqlite> .database
```

- .은 sqlite 프로그램의 기능을 실행하는 것
- 단순히 sqlite3으로 구동하면, 임시 메모리에 저장되며 작업 수행
- 위의 코드로 파일을 생성하여 작업을 저장함 (보통 ~.sqlite3)



### csv => table

```
sqlite> .mode csv
sqlite> .import <~.csv> <table 이름>
```

- 해당 DB에 있는 table들 확인

```
sqlite> .tables
examples
```



### DDL

-  CREATE

```sql
-- 01_create_table.sql
CREATE TABLE classmates(
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    name TEXT NOT NULL
);
```

```
sqlite> .read 01_create_table.sql
```

​	터미널에 한줄 씩 따라 쳐도 되지만, 수정의 용이함을 위해 sql 파일에서 작성 후 실행

​	@ id 설정안하면, sqlite에서 rowid 자동 설정 (재사용 o)

-  DROP

```sql
-- 02_drop_table.sql
DROP TABLE classmates;
```

- ALTER
  1. table 이름 변경
  2. column 이름 변경
  3. 새 column 추가

```SQL
-- 1. table 이름 변경
ALTER TABLE <테이블이름> RENAME TO <새이름> ;
-- 2. column 이름 변경
ALTER TABLE <테이블이름> RENAME COLUMN <COL이름> TO <새이름> ;
-- 3. 새 column 추가
ALTER TABLE <테이블이름> ADD COLUMN <COL이름> <데이터타입>
ALTER TABLE <테이블이름> ADD COLUMN <COL이름> <데이터타입> NOT NULL DEFAULT <기본값>
```



### 스키마 확인

```
sqlite> .schema <table>
```



### DML

- SELECT

```
sqlite> SELECT * FROM <table>;
```

 : 출력 옵션
 		`sqlite> .headers on`
 		`sqlite> .mode column`

-	INSERT

```sql
INSERT INTO classmates (name, age, address) VALUES 
('홍길동', 30, 'SEOUL'),
('김철수', 30, '대전');
```

​		: column 전체에 데이터 입력시 column 명은 생략 가능

- DELETE

```sql
DELETE FROM classmates WHERE id=5;
```

- UPDATE

```sql
UPDATE classmates SET name = '홍길동', address = '제주도' WHERE id=5;
```



### SELECT와 함께 쓰는 clause

- Limit
  - 쿼리에서 반환되는 행 수를 제한
  - 특정 행부터 시작해서 조회하기 위해 `OFFSET` 키워드와 함께 사용하기도 함
  - `SELECT id, name FROM classmates LIMIT 2 OFFSET 2;`
- Where
  - 쿼리에서 반환된 행에 대한 특정 검색 조건을 지정
  - 논리 연산 : AND, OR
  - `SELECT id, name, address FROM classmates WHERE address="서울";`
  - `WHERE ANIMAL_TYPE IN ("Cat", "Dog")`
  - `WHERE HOUR(DATETIME) BETWEEN 9 AND 20`
- Distinct
  - 조회 결과에서 중복행을 제거
  - DISTINCT 절은 속성 바로 앞에 작성
  - `SELECT DISTINCT age FROM classmates;`



## Clause

### aggregate functions

- COUNT

    ```sql
    SELECT COUNT(age) FROM users;
    ```

- AVG
- MAX

    ```sql
    -- 최고 잔액과 이름을 읽음
    SELECT name, MAX(balacne) from users;
    ```

- MIN
- SUM



### LIKE operator

- 패턴 일치를 기반으로 데이터를 조회하는 방법

- wildcard

  - % : 0개 이상의 문자
  - _ : 임의의 단일 문자

  ```
  SELECT * FROM 테이블 WHERE 컬럼 LIKE '와일드패턴'
  ```

- 기본적으로 대소문자 구분 없이 검색함
  - binary 형식일 경우 구분 함으로, `WHERE BINARY(컬럼) LIKE '와일드패턴'`으로 구별

### ORDER BY

- SELECT 문에 추가하여 조회 결과 집합을 정렬
- keyword
  - ASC   - 오름차순(default)
  - DESC - 내림차순

```SQL
SELECT first_name, last_name FROM users ORDER BY balance DESC LIMIT 10;
```



### GROUP BY

- 행 집합에서 요약 행 집합을 만듦
- 선택된 행 그룹을 하나 이상의 열 값으로 요약 행으로 만듦
- WHERE 절 사용 시, 반드시 WHERE 뒤에 작성

```sql
SELECT last_name, COUNT(*) FROM users GROUP BY last_name;
```



## JOIN

#### LEFT JOIN

```MYSQL
SELECT *
FROM 테이블1 AS T1 LEFT JOIN 테이블2 AS T2 ON T1.KEY = T2.KEY
```

- 테이블1을 기준으로 KEY에 대해서 T1과 값이 같은 T2의 행들을 추가함
- 테이블1과 테이블2가 같은 속성명을 가지고 있을 경우, `테이블.속성` 으로 접근



### FULL OUTER JOIN

- LEFTJOIN 결과와 RIGHTJOIN 결과를 UNION 함으로써 도출



## UNION

- 횡단 병합
- 합치는 두 테이블은 `컬럼명`이 모두 같아야 함
- 컬럼명이 다를 경우 수정해서 합침

### 중복 제거 결과 출력

``` MYSQL
(SELECT 컬럼1, 컬럼2 FROM 테이블1)
UNION
(SELECT 컬럼1, 컬럼2 FROM 테이블2)
```



### 중복 포함 결과 출력

``` MYSQL
(SELECT 컬럼1, 컬럼2 FROM 테이블1)
UNION ALL
(SELECT 컬럼1, 컬럼2 FROM 테이블2)
```





## 기타

### String Slice

- 왼쪽에서 문자열 자르기
  - left( 컬럼명 or 문자열 , 왼쪽에서 자를 문자열 길이)
- 오른쪽에서 문자열 자르기
  - right( 컬럼명 or 문자열 , 오른쪽에서 자를 문자열 길이)
- 중간에서 문자열 자르기
  - substring(컬럼명 or 문자열 , 시작위치, 길이)  # 시작위치는 1부터 시작



### String -> int

문자열 + 0 하면 해당 숫자로 바뀜



### DATETIME

- HOUR( date )

  : 시간만 반환

- DATEFORMAT

  ```mysql
  DATE_FORMAT(DATETIME, '%Y-%m-%d %h-%i-%s')
  ```

  - M : 달 이름, m : 달 숫자
  - D : 요일 이름, d : 요일 숫자



### WITH RECURSIVE

- 재귀 커리를 이용하여 메모리 상에 가상의 테이블을 저장

```mysql
WITH RECURSIVE 테이블명 AS (
	SELECT 초기값 AS 컬럼명1
    UNION ALL
    SELECT 컬럼명1 계산식 FROM 테이블명 WHERE 제어문

)
```



### IF 문

- IF

```MYSQL
SELECT IF( 조건 , 참값 , 거짓값) AS 이름
FROM 테이블;
```

- IFNULL

```MYSQL
SELECT IFNULL(속성, NULL대체값 ) AS 이름
FROM 테이블;
```









