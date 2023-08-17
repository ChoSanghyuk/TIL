# SQL



## 01. 시작하기

### 데이터 딕셔너리

- 개념

  - 테이블, 인덱스 등 데이터베이스 객체에 대한 정보를 저장하고 있는 객체
  - ALL_, DBA_, USER_ 등의 시스템 뷰를 통해 해당 정보를 조회

  | 접두어 | 설명                                                         |
  | ------ | ------------------------------------------------------------ |
  | ALL    | 접근 권한이 있는 모든 정보를 확인                            |
  | DBA    | DBA 권한이 있는 USER가 접근할 수 있는 뷰<br />모든 유저가 소유한 객체에 대한 정보를 확인 가능 |
  | USER   | 로그인한 USER의 정보를 확인할 수 있는 뷰                     |
  | V$     | 성능 관련 데이터를 확인할 수 있는 뷰                         |

- 자주 사용되는 시스템 뷰

  | 오브젝트 명      | 설명                               |
  | ---------------- | ---------------------------------- |
  | ALL_TABLES       | 테이블 정보                        |
  | ALL_TAB_COLUMNS  | 테이블에 있는 컬럼들의 정보        |
  | ALL_VIEWS        | 뷰 정보                            |
  | ALL_SYNONYMS     | 시노님 정보                        |
  | ALL_SEQUENCES    | 시퀀스 정보                        |
  | ALL_CONSTRAINTS  | 제약조건 정보                      |
  | ALL_CONS_COLUMNS | 제약조건을 가진 컬럼들에 대한 정보 |
  | ALL_TAB_COMMENTS | 테이블 주석 정보                   |
  | ALL_COL_COMMENTS | 컬럼 주석 정보                     |
  | ALL_INDEXES      | 인덱스 정보                        |
  | ALL_IND_COLUMNS  | 인덱스 컬럼 정보                   |



## 02. SQL 작성 표준



### SQL 가이드 

- DISTICNT는 정렬 작업을 수반하므로 처리결과 값이 항상 UNIQUE한 경우 불필요하게 사용 X
- OR을 복잡하게 사용하면 FULL TABLE SCAN이 발생하여 성능 :arrow_down:
  - 가능한 UNION ALL 또는 DECODE, IN 등을 활용
- JOIN시, Driving 테이블 (첫 번째로 Access 되는 테이블)의 필터 조건이 있다면, 가장 먼저 기술



### 명명규칙 가이드

- TABLE
  - 테이블 특성 구분
    - M : MASTER TABLE (기본/공통, 명세)
    - F : MASTER성 내역 TABLE (상세)
    - C : 코드, 목록 TABLE (코드, 목록)
    - H : 내역, 이력, 명세 TABLE ( 내역, 이)
    - N : 채번 TABLE
    - G : 기타 (GENERAL)



## 03. SQL 작성 기본



### 데이터 타입

- 문자 데이터 타입

  - CHAR
    - 지정된 길이보다 짧은 데이터가 입력되는 경우, 나머지 공간은 **공백**으로 채움
    - 공백 인지하기 어렵기 때문에 사용 지양
    - 비교 방법
      - 짧은 쪽에 공백을 추가한 후 비교
  - VARCHAR
    - 지정된 길이보다 짧은 데이터가 입력되는 경우, 나머지 공간은 **NULL**로 채움
    - 데이터의 성격이 가변 길이일 때 VARCHAR2 사용
    - 비교 방법
      - NULL이 아닌 문자간 비교

  => CHAR VS VARCHAR 비교 시, 같은 값이 들어있어도 컬럼 길이가 다르다면 불일치 

  :bulb: 오라클에서 문자형 데이터 타입은 NULL과 `''`를 동일 취급 (단, 항상 같은 값 취급은 X => 구분 필요)

- 숫자 데이터 타입

  - NUMBER
    - NUMBER(p,s) 형식으로 입력
      - p : 소수점 포함하는 전체 자리 수. 미지정 시, 입력되는 숫자 크기만큼 저장공간 할당
      - s : 소수점 이하 자리. 미지정 시, 소수점 이하 반올
  
- 날짜 타입

  - DATE
    - 세기, 년도, 월,일.시간,분,초의 날짜와 시간정보
    - 7 Bytes 할당

  - Timestamp
    - 초 이하 단위까지 저장 가능
    - 밀리초를 0~9자리까지 표현




### NULL의 특성

- 연산
  - 산술연산 (+, -, *, /) : 결과 NULL
  - 비교연산 (=, <, >) : NULL 제외하고 연산
- 함수
  - 일반함수 (TO_CHAR, LENGTH) : 결과 NULL
  - 그룹함수 (SUM, AVG, MAX) : NULL 제외하고 연산 (`COUNT(*)` 제외)



### ROWNUM

- 데이터 정렬
  - NULL 값은 제일 큰 값으로 취급
    - 오름차순 시 맨 뒤 표시 및 내림차순에서는 맨 앞 표시
- 사용
  - `WHERE ROWNUM = 5` , `ROWNUM < 6`
- SELECT문 수행순서
  - FROM => WHERE => ROWNUM => GROUP BY => HAVING => SELECT => ORDER BY 



### 연산자 기본

- `IN` VS `EXISTS`

  - IN

    - 전체 범위 처리를 통하여 서브 쿼리의 조건을 만족하는 모든 데이터 집합을 구한 다음, 메인쿼리의 조건 처리

      => 제공자 역할을 하는 서브쿼리의 결과에 따라 성능이 좌우

    - **서브쿼리의 결과 데이터가 적을 때** 성능상 유리

  - EXSITS

    - 메인 쿼리가 찾은 데이터에 대하여 하나씩 LOOP 형식으로 존재 여부를 확인
    - 메인 쿼리가 먼저 수행되며, 서브 쿼리는 존재 여부만 판단하고 메인 쿼리로 돌아감
    - **메인 쿼리의 결과 데이터가 적을 때** 성능상 유리

- `NOT IN` VS `NOT EXISTS`

  :bulb: NULL은 조인에 참여하지 않는다

  - `NOT IN`
    - 서브쿼리의 결과에 NULL이 있건 없건, 메인 쿼리의  NULL 값은 등장하지 않는다
    - 단, `IN` 절에서는 사용가능
  - `NOT EXISTS`
    - 서브쿼리의 JOIN 절에서 NULL 값은 빠지기 때문에, 해당 ROW는 FALSE 취급 => 전체적으로는 TRUE가 되어 NULL 값이 결과에 등장



### MYSQL 함수

- 문자변환 함수
  - LOWER
  - UPPER
  - INITCAP : 첫 글자만 대문자 변환
  - CONCAT
  - SUBSTR
  - LENGTH
  - INSTR : 명명된 문자의 위치를 구함
    - `INSTR(COL1, 'v')`
  - LPAD : 왼쪽 문자 자리 채움
    - `LPAD(COL1, 10, *)
  - RPAD
  - TRIM : 선행문자와 후행문자를 모두 자름
    - `TRIM ('S' FROM COL1)` => 좌우의 S들 전부 제거
  - REPLACE
    - `REPLACE('COL1, 'St', 'Ok')`
- 숫자함수
  - ROUND : 지정된 소수점 자릿수로 값을 반올림 (지정 숫자가 `-`인 경우 정수레벨)
    - `ROUND(4.592,2)`
  - TRUNC : 지정된 소수점 자릿수로 값을 버림
  - CEIL : 지정한 값보다 크거나 같은 수 중에서 가장 작은 정수
  - FLOOR : 지정한 값보다 작거나 같은 수 중에서 가장 작은 정수
  - MOD : 나눈 나머지를 반환
    - `MOD(1600, 300)`

- 날짜함수

  - MONTHS_BETWEEN : 두 날짜 간의 월 수 구하기
    - 결과는 양수/음수 가능
    - 결과에서 정수가 아닌 부분은 월의 일부분을 나타냄
  - ADD_MONTHS : 날짜에 월 추가/감소
  - NEXT_DAY : 지정된 날짜의 다음 날, 혹은 지정 요일의 날짜
    - `NEXT_DAY(SYSDATE, '금요일')` => SYSDATE 이후 첫 금요일

  - LAST_DAY : 월의 마지막 날
  - ROUND : 날짜 반올림
    - `ROUND(SYSDATE)` => 시분초 반올림
    - `ROUND(SYSDATE, 'MONTH')` => 요일에서 반올림
    - `ROUND(SYSDATE, 'YEAR')` =>  달에서 반올림
  - TRUNC : 날짜 절삭



### 데이터 형 변한

- TO_CHAR

  - NUMBER 타입 변한

    | FORMAT 요소 | 결과                |
    | ----------- | ------------------- |
    | 9           | 숫자 표시           |
    | 0           | 0이 표시되도록 강제 |
    | $           | 부동 달러 기호      |
    | L           | 부동 로컬 통화 기호 |
    | .           | 소수점 출력         |
    | ,           | 천 단위 표시자      |

  - `TO_CHAR(SALARY, '$9,999.99')`

- TO_NUMBER

  - 문자열 형식을 숫자로 변환
    - `TO_NUMBER('1234')` => 1234

- 데이터 형 변환 원칙

  - 반드시 명시적 형 변환을 사용
  - 포맷이 필요한 형 변환
  - :bulb: **WHERE 절의 컬럼 변환 제거**
    - WHERE 컬럼은 원칙적으로 변환 X
      - 넓은 데이터 범위에 대한 추가적인 연산
      - 인덱스 컬럼이 대상일 경우, index 활용을 못 하여 FULL Scan 가능성 존재
      - DB별 다른 형변환 처리로 인한, 결과값 상이




### 조건부 표현식

- CASE

  ```sql
  CASE expr WHEN comparison_expr1 THEN return_expr1
  		  [WHEN comparison_expr2 THEN return_expr2
  		  ELSE return_expr3]
    END
  ```

  ```SQL
  CASE WHEN condition1 THEN return_expr1
       [WHEN condition2 THEN return_expr2
        ELSE return_expr3]
    END
  ```

  - 조건에 맞는 값 및 ELSE절 미존재 시, NULL 반환
  - 모든 반환 값은 데이터 유형 일치

- DECODE

  ```SQL
  DECODE ( col, search1, result1 
         		, search2, result2 
         		, default)
  ```

  - 조건에 맞는 값 및 DEFAULT 값 미존재 시, NULL 반환
  - 실제 업무에서는 GROUP 함수와 함께 사
  - 비교 연산자 사용 X

  

## SQL 작성 응용



### ANSI-SQL 특징

- 국제 표준 => DBMS간 높은 호환성
- 조인 조건을 명확히 기술
- 일부 작성이 불가능한 OUTER JOIN도 표현 O
- 각 DBMS 벤더 제공 기능 활용에 제약 O
  - 벤더 dependent SQL : 각 벤더사 자체의 SQL은 자체 구조에 맞는 쿼리에 성능상 이점 존



### JOIN 구분

- 결과 집합 유형

  - INNER : 일치하는 행만 반환
  - OUTER : INNER Join 결과 및 일치하지 않는 행도 반환
    - ORACLE 문법으로는 표기 X

- 조인 연산자 유형

  - EquiJoin : `=` 연산자 이용

  - Non-EquiJoin : `=` 이외의 연산자 이용

    - ex) 카테시안 조인 (Cartesian Join) : 조인 조건이 없거나 누락된 조인

      => 모든 가능한 행들의 조합을 반환 (N행 테이블과 M행 테이블 조인 시, N *M 결과값 가짐)

      ```sql
      -- ORACLE
      SELECT *
      FROM EMPLOYEE E,
      	DEPARTMENT D;
      	
      -- ANSI
      SELECT *
      FROM EMPLOYEES E
      	CROSS JOIN DEPARTMENT D ;
      ```



### 카티시안 곱의 활용

20/132 => 계층 쿼리 학습 후 가로 세로 행/열 바꾸기 



### Oracle OUTER JOIN 제약

- 조인 조건이 하나 이상일 경우, 모든 조인 조건에 (+) 사용
- :bulb: **조인 조건 외에 일반 조건에도 (+) 사용해야 데이터 유실 X**
  - 상수와 비교하는 컬럼에도 사용 필요
- 조인되는 두 개 테이블에 동시에 (+) 사용 X => FULL OUTER JOIN X
- **조인 조건 식에서 (+)가 붙은 컬럼과는 서브쿼리 같이 사용 X**
- 조인 순서가 미리 정해지므로 조인 순서를 이용한 튜닝 X
- Outer Join은 업무적으로 NULL이 필요한 경우에만 사용



### 서브 쿼리

- 종류
  - 인라인 뷰 : 뷰 처럼 활용 => FROM 절 사용
  - 서브쿼리 : 조건역할 => WHERE 절 사용
  - 스칼라서브쿼리 : 하나의  값 => SELECT 문 사용



### 인라인 뷰

- 남용

  - 절차적인 접근으로 사용 시, 반복 엑세하는 인라인 뷰 과다 사용 가능성 O

  - 집합적 사고방식 필요

- 사용 지침

  - **데이터를 감소시켜 처리 범위를 최소화 할 수 있는 경우 사용**

- vs `WITH` 절

  - WITH절 사용 시, 반복적 쿼리 블록을 SELECT 문에서 사용 O
  - 특징
    - WITH 절 한개 이상 선언 O
    - WITH 절 내 다른 WITH 절 참조 O
    - 명시적 오브젝트 생성 없이, 임시테이블 처럼 사용 O

  ```SQL
  WITH TEMP1 AS (
  	SELECT 문 ),
  	 TEMP2 AS (
       SELECT 문)
  SELECT *
  FROM TABLE1
  ...
  ```



### 서브쿼리 (조건)

- 상호 연관 서브쿼리

  - 서브쿼리에서 메인쿼리 테이블과 조인 조건을 사용하는 경우
  - 행을 비교할 때마다 결과를 메인으로 반환 => 성능 저하 이슈 有

  ```SQL
  SELECT A.COL1, A.COL2
  FROM TABLE1 A
  WHERE A.COL3 > ( SELECT AVG(COL3)
                 	 FROM TABLE1 B
                 	WHERE A.COL4 = B.COL4)
  ```

- 사용 지침

  - 여러번 사용 시, 조인 방식, 조인 순서가 부적절해 질 가능성 존재. => 제어의 어려움
  - **서브 쿼리는 어느 한쪽 집합이 단순히 조건 체크로 사용되거나, 메인 쿼리 조건에 상수 값을 제공하는 경우에만 사용**
  - 1:1 변경 가능 시, 일반 조인 사용




### 스칼라 서브쿼리

- 특징
  - 한 행에서 정확히 하나의 열 값을 반환하는 서브쿼리
  - 결과 없을 땐 NULL 
  - 데이터 적을 때 성능상 유리
- 상호 연관 스칼라 서브쿼리 수행 순서 : Buffer 활용
  1. 메인 테이블에서 행 조회
  2. 서브 쿼리에서 조건에 맞는 값 조회
  3. 메인의 값과 결과 값 Buffer에 저장
  4. 메인 테이블에서 다음 조회 후, Buffer에 저장된 값이 있는지 확인
  5. 있다면, 해당 값 사용 후 4번 수행. 없다면 1번부터 수행 반복
- 사용 지침
  - **일반적으로 메인테이블과 조인되는 코드 테이블의 코드명을 가져올 때 효과적** 
  - 스칼라 서브쿼리 대상 테이블의 데이터가 많다면 오버헤드 발생
  - 기본적으로는 JOIN 사용



### 분석함수

- 개념

  - 데이터를 특정 용도로 분석하여 그 결과를 반화는 함수
  - DW 업무에서 효과적
  - 간결한 표현 및 성능적 향상
    - PARTITION 절 => 결과 집합을 여러 그룹으로 분할하여 분석
    - WINDOW 기능 => 결과 집합의 일부를 대상으로 분석

- 구문형식

  ```SQL
  분석함수 (파라미터1, 파라미터2, ...)
  	OVER ( <query_partition 절> 
             <order_by 절> 
             <windowing 절> )
  ```

  - 분석함수 : 분석함수 명을 명시
  - 파라미터 : 분석함수에 따라 0에서 3개까지의 파라미터 가짐
  - query_partition 절 : `PARTITION BY`로 시작하여, 분석함수의 계산그룹 대상 지정
  - order_by 절 : 계산대상 그룹에 대해 정렬작업을 수
  - windowing 절 : `PARTITION BY`에 의해 나누어진 기준 그룹에 또 다시 소그룹을 생성 (생략 시, 소그룹 = 기준그룹)

- 실행 단계

  1. 일반 질의 처리 : 조인, WHERE, GROUP BY 등
  2. 분석 함수 적용
  3. 정렬

- 종류

  - **RANKING** 함수 : 순위 함수

    - 다른 레코드와 비교되는 순서 계

    - TOP N 분석에 활용

    - 종류

      - `RANK`
        - 순위 반환
        - 1등이 2건인 경우, 다음 순위는 3등
        - 정렬 결과를 기준으로 전체 순위를 출력
      - `DENSE_RANK`
        - 1등이 2건인 경우, 다음 순위 2등
        - 동일 순위 무시한 연속 순위 출력
      - `ROW_NUMBER`
        - 1부터 시작하여 각 로우 별로 순차적 값 반환
        - 무조건 순서대로 순번 반환

    - 예시

      ```SQL
      SELECT EMPLOYEE_ID, SALARY,
      		RANK() OVER (ORDER BY SALARY DESC) RANKING1,
      		DENSE_RANK() OVER (ORDER BY SALARY DESC) RANKING2,
      		ROW_NUMBER() OVER (ORDER BY SALARY DESC) RANKING3
      FROM EMPLOYEES ;
      ```

      







