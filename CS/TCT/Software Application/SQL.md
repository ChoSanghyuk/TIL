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
- ORDER BY 절에는 단순 컬럼 기술 외에 다양한 함수 (ex CASE) 사용 O



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
      - s : 소수점 이하 자리. 미지정 시, 소수점 이하 반올림
  
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
    - `LPAD(COL1, 10, *)`
  - RPAD
  - TRIM : 선행문자와 후행문자를 모두 자름
    - `TRIM ('S' FROM COL1)` => 좌우의 S들 전부 제거
  - REPLACE
    - `REPLACE(COL1, 'St', 'Ok')`
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
  - 실제 업무에서는 GROUP 함수와 함께 사용
  - 비교 연산자 사용 X

  

## SQL 작성 응용



### ANSI-SQL 특징

- 국제 표준 => DBMS간 높은 호환성
- 조인 조건을 명확히 기술
- 일부 작성이 불가능한 OUTER JOIN도 표현 O
- 각 DBMS 벤더 제공 기능 활용에 제약 O
  - 벤더 dependent SQL : 각 벤더사 자체의 SQL은 자체 구조에 맞는 쿼리에 성능상 이점 존재



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



### :bulb: 행/열 바꾸기

- 카타시안 곱의 활용

  ```SQL
  SELECT DEPARMENT_ID
  	  ,JOB_ID
  	  ,DECODE(LV, 1, '결혼여부', 2, '공채여부', 3, '특별관리대상여부') 구분
  	  ,DECODE(LV, 1, MARRY_YN, 2, OPENRECUIT_YN, 3, SPECIALMMG_YN) 값
  FROM EMPLOYEES
  	,(SELECT LEVEL LV
        FROM DUAL
        CONNECT BY LEVEL <=3)
  WHERE 1=1
  AND JOB_ID LIKE 'M%'
  AND DEPARMENT_ID IN (20,30)
  ORDER BY 1,2,3
  ```

- PIVOT 함수 활용

  - PIVOT 개요

    - 개념

      - 식에 있는 한 열의 값들을 회전하여 테이블의 열(column)으로 지정

    - 문법

      ```SQL
      SELECT 집계함수의 열 + 기준열의 값
      FROM (
          SELECT 원본테이블
      ) AS TEMP
      PIVOT (
          집계함수 FOR 기준열 IN (기준열의 값)
      ) AS PVT
      ```

      - `집계함수의 열`과 `기준열의 값`로 GROUP BY 한 집계함수의 결과에서 `기준열의 값` 과 집계함수 값을 열로 회전시킴

    - 예제

      ```SQL
      SELECT deptno
           , p_president
           , p_analyst
           , p_manager
           , p_salesman
           , p_clerk
        FROM (
               SELECT deptno
                    , job
                    , sal
                 FROM emp
             )
       PIVOT (
               SUM(sal) FOR job IN ( 'PRESIDENT' AS p_president
                                   , 'ANALYST'   AS p_analyst
                                   , 'MANAGER'   AS p_manager
                                   , 'SALESMAN'  AS p_salesman
                                   , 'CLERK'     AS p_clerk )
             )
      ```

- DECODE 활용

  ```SQL
  SELECT deptno
       , SUM(DECODE(job, 'PRESIDENT', sal)) AS p_president
       , SUM(DECODE(job, 'ANALYST', sal))   AS p_analyst
       , SUM(DECODE(job, 'MANAGER', sal))   AS p_manager
       , SUM(DECODE(job, 'SALESMAN', sal))  AS p_salesman
       , SUM(DECODE(job, 'CLERK', sal))     AS p_clerk
    FROM emp
   GROUP BY deptno
   ORDER BY deptno
  ```

  



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

  - 데이터를 특정 용도로 분석하여 그 결과를 반환하는 함수
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
  - order_by 절 : 계산대상 그룹에 대해 정렬작업을 수행
  - windowing 절 : `PARTITION BY`에 의해 나누어진 기준 그룹에 또 다시 소그룹을 생성 (생략 시, 소그룹 = 기준그룹)

- 실행 단계

  1. 일반 질의 처리 : 조인, WHERE, GROUP BY 등
  2. 분석 함수 적용
  3. 정렬

- 종류

  - **RANKING** 함수 : 순위 함수

    - 다른 레코드와 비교되는 순서 계산

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
  
  - LAG / LEAD 함수
  
    - 동일한 테이블에 있는 다른 행의 값을 참조
  
      - 일별 매출 추이와 같은 연속된 데이터 값을 분석 시 사용
  
    - 사용
  
      - LAG
        - 현재 행을 기준으로 이전 값을 참조
        - `LAG(expr, offset, default) OVER (PARTITION BY 절)`
        - offset : 현재 행의 중심으로 offset만큼 이전 행의 값 표시. 행 없을 시, default 값 표기
      - LEAD
        - 현재 행을 기준으로 이후 값을 참조 
        - `LEAD(expr, offset, default) OVER (PARTITION BY 절)`
        - offset : 현재 행의 중심으로 offset만큼 이후 행의 값 표시. 행 없을 시, default 값 표기
  
    - 예시
  
      ```SQL
      -- 현재연도, 이전연도, 이후 연도에 입사한 사원수를 동시에 추출하기
      SELECT
      	TO_CHAR(HIRE_DATE, 'YYYY') 입사연도, COUNT(*) 사원수,
      	LAG(COUNT(*),1,0) OVER (ORDER BY TO_CHAR(HIRE_DATE, 'YYYY')) "이전연도사원수",
      	LEAD(COUNT(*),1,0) OVER (ORDER BY TO_CHAR(HIRE_DATE, 'YYYY')) "이후연도사원수"
      FROM EMPLOYEES
      GROUP BY TO_CHAR(HIRE_DATE, 'YYYY')
      ORDER BY TO_CHAR(HIRE_DATE, 'YYYY') ;
      ```
  
  - RATIO_TO_REPORT 분석함수
  
    - 계산 대상 값 전체에 대한 현재 로우의 상대적인 비율 값을 반환
  
    - 각 로우별로 PARTITION BY 절에 명시된 그룹의 총합에 대한 비율을 반환
  
    - 사용법
  
      - `RATIO_TO_REPORT (expr) OVER (PARTITION BY절)`
      - expr : null일 시, null 반환
  
    - 예시
  
      ```sql
      -- 2008년 전체 영업 실적 중 각 개인별 실적 비율 추출하기
      SELECT EMP, LAST_NAME 이름, SUM(ORD.ORDER_TOTAL) 개인별실적,
      	   ROUND(RATIO_TO_REPORT(SUM(ORD.ORDER_TOTAL)) OVER
                  	(PARTITION BY TO_CHAR(ORD.ORDER_DATE, 'YYYY')),2) RATIO
      FROM ORDERS ORD, EMPLOYEES EMP
      WHERE TO_CHAR(ORD.ORDER_DATE, 'YYYY') = '2008'
      AND ORD.SALES_REP_ID = EMP.EMPLOYEE_ID
      GROUP BY EMP.LAST_NAME, TO_CHAR(ORD.ORDER_DATE, 'YYYY')
      ORDER BY EMP.LAST_NAME ;
      ```
  
  - LISTAGG
  
    - 그룹핑 하여 ROW 데이터를 한 컬럼으로 조회
  
    - 사용법
  
      - `LISTAGG(expr, 구분자) WITHIN GROUP (ORDER BY 절)`
      - WITHIN GROUP() 절에 아무 조건도 주지 않으면 에러 발생
      - 구분자에 예약어 사용 X
  
    - 예시
  
      ```SQL
      -- 부서별로 속한 사원을 한 ROW로 보여주는 SQL 
      SELECT DEPARTMENT_ID DID,
      	LISTAGG(LAST_NAME, ',') WITHIN GROUP (ORDER BY LAST_NAME) AS ENAMES
      FROM EMPLOYEES
      GROUP BY DEPARTMENT_ID
      ORDER BY DEPARTMENT_ID ;
      ```
  
      



### 윈도우 함수

- 구문

    ```sql
    윈도우함수 OVER (PARTITION BY expr ORDER BY expr [ASC|DESC])
        ROWS | RANGE
            BETWEEN UNBOUNDED PRECEDING | PRECEDING | CURRENT ROW
                AND UNBOUNDED FOLLOWING | CURRENT ROW
    ```

    - OVER : 함수를 적용하기 위한 행의 정렬 기준 또는 대상 행 집합에 대한 윈도우 정의
      - FROM, WHERE, GROUP BY, HAVING 절 처리 이후 적용
    
    - ROWS | RANGE : 윈도우의 크기를 정렬하기 위한 행 집합 정의
    
      - ROWS :  조회된 행이 기준이기에 값의 동일 여부를 떠나 **하나하나 연산**
      - RANGE : 조회된 행의 **값이 기준이기에 같다면 묶어서 합산 후 연산**
    
    - BETWEEN ... AND : 윈도우의 시작 위치와 마지막 위치 지정
    
    - UNBOUNDED PRECEDING |CURRENT ROW |UNBOUNDED FOLLOWING : 부분집합을 결정하기 위한 범위
    
      - UNBOUNDED PRECEDING : 제일 앞
      - CURRENT ROW : 현재 ROW
      - UNBOUNDED FOLLOWING : 제일 끝
      - 상수값 PRECEDING: 현재 값을 기준으로 상수값 만큼의 앞에서부터 지정. (앞에 상수만큼의 데이터가 없을 경우 NULL)
      - 상수값 FOLLOWING : 현재 값을 기준으로 상수값 만큼의 뒤까지의 범위 지정 (뒤에 상수만큼의 데이터가 없을 경우, NULL)
    
    - BETWEEN 없는 경우
    
      - UNBOUNDED PRECEDING : 처음부터 하나씩 계산한 결과 표시
      - CURRENT ROW : 현재행부터 현재행까지 => 현재행의 값만 표시
      - n PRECEDING :  현재행에서 앞선 n개 행까지 범위 지정
    
      추가 참고자료 : https://tiboy.tistory.com/570
    
- 종류

    - SUM
        - 누적합계 구하기
        - `SUM(TOTAL) OVER (PARTITION BY 부서번호 ORDER BY TOTAL)`
            - PARTITION BY 구문으로 세부적인 그룹핑 => 해당 ROW와 같은 부서번호의 누적합에 자신의 값을 더한 값이 표기됨
            - 부서번호 기준으로 누적판매금액 집계



### Paging 처리

- 개념

  - 온라인 프로그램 중 추출된 데이터 건수가 많아 한 화면에 보여주면 성능상 문제 발생 경우 有

    => 페이지 분할 => 최소 데이터만 보여주도록 작성

  - 종류 : Index Paging, NEXT Paging

- INDEX PAGE 개념

  - 전체 페이지(or 전체 건수)와 Page Index를 같이 보여주는 방법

  - 전체 건수 SQL 및 상세 데이터 SQL 두 가지가 실행

  - 사용방법

    ```sql
    SELECT HIRE_DATE
    	 , EMPLOYEE_ID
    	 , FIRST_NAME
    	 , SALARY
    	 , JOB_ID
    FROM (SELECT ROWNUM RN, A.*
         FROM (SELECT HIRE_DATE
              		, EMPLOYEE_ID
              		, FIRST_NAME
              		, SALARY
              		, JOB_ID
              		FROM EMPLOYEES A
              		WHERE DEPARTMENT_ID IN (20,50,80)
              		ORDER BY FIRST_NAME
              ) A
         	WHERE ROWNUM <= (:Page) * 20
         )
    WHERE RN >= (:Page-1) * 20 +1
    ```

    - 부등호 사용 시 , `ORDER BY FRIST_NAME` 부분에서 `(:Page) * 20` 만큼만 정렬하고 STOP

- NEXT PAGE  (요즘 미사용 추정. skip)

  - 개념

    - 전체 건수를 추가적으로 조회하는 일반 Index Paging과 달리, 성능을 고려하여 한번의 조회로 필요한 데이터를 조회

    - 프로그램에서 다음 시작 페이지의 값을 저장

      => 항상 일정한 양의 row 해당 부분을 조회하여 응답시간이 일정

    - 대량의 결과를 page 단위로 보여줄 경우, Next Key를 사용하여 부분처리를 유도함으로써 온라인 성능을 높임

  - 사용 방법

    ```sql
    SELECT ...
    FROM TABLE1
    WHERE 조회조건들..
    AND a >= :a		-- 공통조건
    AND NOT(a = :a AND b < :b)
    AND NOT(a = :a AND b = :b AND c < :c)
    ORDER BY a,b,c 	-- ORDER BY 절에 기술되는 컬럼들이 nextkey
    ```
  
    
  

### GROUP 함수

- ROLLUP

  - 개념

    - ROLLUP 연산자는 GROUP BY 절의 그룹 조건에 따라 전체 행을 그룹화하고 각 그룹에 대해 부분합을 구함
      - 그룹별 소계
      - ex) GROUP BY ROULLUP(a,b) = GROUP BY a,b + GROUP BY a + total
    - 인수로 받은 컬럼 수가 n개 인 경우 ROLLUP에 의해 만들어지는 그룹핑 조합은 n+1개

    | ROLLUP                 | UNION ALL                                                    |
    | ---------------------- | ------------------------------------------------------------ |
    | GROUP BY ROLLUP(a,b,c) | GROUP BY ROLLUP(a,b,c) UNION ALL<br />GROUP BY ROLLUP(a,b) UNION ALL<br />GROUP BY ROLLUP(a) UNION ALL<br />GROUP BY ROLLUP() |

  - ROLLUP 사용하기

    ```SQL
    SELECT DEPARTMENT_ID, JOB_ID, SUM(SALARY)
    FROM EMPLOYEES
    WHERE DEPARTMENT_ID < 30
    GROUP BY ROLLUP(DEPARTMENT_ID, (JOB_ID, MANAGER_ID))
    ;
    ```

    - ROLLUP 의 인자에서 괄호로 묶인 열은 하나의 단위로 처리된다.

- CUBE

  - 개념

    - 그룹핑된 컬럼의 모든 가능한 조합에 대하여 합계를 구함
      - ex) GROUP BY CUBE(a,b) = GROUP BY a,b + GROUP BY a + GROUP BY b + total
    - 인수로 받은 컬럼 수가 n개 인 경우 CUBE에 의해 만들어지는 그룹핑 조합은 2^n개

  - CUBE 사용하기

    ```SQL
    SELECT DEPARTMENT_ID, JOB_ID, SUM(SALARY)
    FROM EMPLOYEES
    WHERE DEPARTMENT_ID < 30
    GROUP BY CUBE(DEPARTMENT_ID, (JOB_ID, MANAGER_ID))
    ;
    ```

    - 지정 가능한 모든 그룹화 조합의 소계와 총계를 산출

- GROUPING

  - 개념

    - ROLLUP 또는 CUBE 연산자와 함께 사용하는 함수
    - 인수로 지정된 컬럼이 해당 그룹핑 작업에 사용되었는지 구별
      - 그룹 조합에서 사용 => 0 / 미사용 => 1
    - GROUPING 함수를 이용하여 그룹 결과의 NULL이 원래 저장된 값인지 (0), 그룹 조합 생성시 유도된 값인지 확인 (1)
    - GROUPING 함수에 사용되는 인수는 GROUP BY  절에 지정된 컬럼 중 하나여야 함
    - 매개변수의 컬럼 순서에 맞게 해당 컬럼이 NULL인 경우 1을 반한하고 한 행을 2진수 표기
      - ex. `GROUP BY ROLLUP(a,b,c)`에서 a와c가 그룹에 미참여한 row의 경우 5(101)로 표기됨

  - 사용하기

    ```sql
    SELECT DEPARTMENT_ID DID, JOB_ID JID, SUM(SALARY),
    	   GROUPING(DEPARTMENT_ID) GRP_DEPT,
    	   GROUPING(JOB_ID) GRP_JOB
    FROM EMPLOYEES
    WHERE DEPARTMENT_ID < 30
    GROUP BY ROLLUP(DEPARTMENT_ID, JOB_ID)
    ;
    ```

- GROUPING SETS 

  - 개념
    - GROUP BY 절에서 그룹 조건을 여러 개 지정할 수 있는 함수
    - 구문의 결과는 각 그룹 조건에 대해 별도로 GROUP BY한 결과를 UNION ALL한 결과와 동일
    - 하나의 SQL에 여러 개의 그룹 조건을 한번에 지정하여 복잡한 그룹처리 과정을 단순화

  - 사용하기

    ```SQL
    SELECT DEPARTMENT_ID DID, JOB_ID JID, MANAGER_ID MID, AVG(SALARY)
    FROM EMPLOYEES
    WHERE DEPARTMENT_ID < 30
    GROUP BY GROUPING SETS ((DEPARTMENT_ID, JOB_ID, MANAGER_ID ),
                           	(DEPARTMENT_ID, MANAGER_ID ),
                            (JOB_ID, MANAGER_ID ))
    ;
    ```

    - 세 가지 GROUP에 대한 GROUP BY 결과를 UNION ALL 한 것과 동일
  
  - GROUPING SETS vs ROLLUP, CUBE
  
    - CUBE(a,b,c) = GROUPING SETS( (a,b,c), (a,b), (a,c), (b,c), (a), (b), (c), () )
    - ROLLUP(a,b,c) = GROUPING SETS((a,b,c), (a,b), (a), () )
  
  - 연결된 그룹화
  
    ```sql
    SELECT DEPARTMENT_ID DID, JOB_ID JID, MANAGER_ID MID, AVG(SALARY)
    FROM EMPLOYEES
    WHERE DEPARTMENT_ID < 30
    GROUP BY GROUPING SETS (DEPARTMENT_ID),
             GROUPING SETS (JOB_ID, MANAGER_ID )
    ORDER BY 1,2,3;
    ```
  
    - GROUPING SET에서 가져온 그룹의 CROSS-Product로 실행
      - = GROUPING SETS( (DEPARTMENT_ID, JOB_ID),  (DEPARTMENT_ID, MANAGER_ID) )



## SQL 작성 심화



### SQL 처리 구조

- SELECT / INSERT/ UPDATE / DELETE 처리 절차 및 REDO, UNDO 동작 원리



### SQL 처리 절차

- SQL 처리 단계

  1. Create Cursor
     - SQL 문장 처리를 위해 메모리 일정 영역 점유
  2. Parse SQL
     - SQL 구문 분석과 최적화를 통한 실행 계획 생성
  3. Bind variables
     - 구문실행 전, Bind 변수를 사용한 경우 변수값 대응
  4. Execute SQL
     - DDL, DML => 구문 자체 실행
     - 질의(QUERY) => Row Fetch하기 위한 준비 수행
  5. Fetch Rows
     - Execute 결과값 검색하여 Row 반환
  6. Close Cursor
     - 할당된 자원 반환

- Soft parsing & Hard Parsing

  ```
  					<< Parse >>
  SQL 			=>	1.Check Syntax
  EXECUTE			<=	2.Search shared SQL area 	: SOFT PARSE
  					3.Search data dictionary
  					4.Optimizer
  EXECUTE			<=	5.Save execution plan		: HARD PARSE
  ```

  1. Check Syntax 
     - SQL 구문 문법 체크
  2. Search shared SQL area 
     - 같은 SQL이 shared pool에 있는지 확인
  3. Search data dictionary
     - SQL문에 포함된 모든 object와 column에 대한 dictionary validation 수행 및 권한 체크
  4. Optimizer
     - 최적화된 실행 계획 결정
  5. Save execution plan
     - Shared pool에 parse 정보 저장

- 바인드 변수 필요성

  - 바인드 변수 사용 => Oracle은 SQL을 동일한 것으로 판단

  - Reparse 없이 Shared Pool에 저장된 SQL 실행

  - Hard Parse 감소 => CPU 사용률 감소 & Shared Pool 메모리 공유 

    ==> DB 서버 메모리 사용 감소 및 성능 향상



### 인덱스

- 개념
  - '어떤 데이터가 어디에 있다'라는 위치 정보를 가진 주소록 개념
  - 책의 목차나 색인 역할로 테이블과 연결된 물리적인 객체
  - 인덱스를 생성시킨 Key 컬럼과 RowID로 구성되며 정렬된 상태로 저장
  - 인덱스는 테이블 Row와 하나씩 대응
- 데이터 Access 방식
  - FULL SCAN => 테이블에서 직접 원하는 Data 찾음
  - INDEX SCAN => 우선 색인 Data에서 조건 검색을 수행 후 검색된 색인을 사용하여 Data 조회
- B-Tree 인덱스
  - 2개의 자식만 가지는 Binary Tree 구조에서 확장되어 n개의 자식을 가질 수 있는 트리 구조
    - 단, 노드의 자식수가 비대칭적일 경우 비효율적이기 때문에, 균형을 맞춤 (Balanced)
    - 모든 leaf node가 Root node와 동일한 깊이를 가짐 (동일 Height) => 균일한 성능 보장
  - leaf node 외의 노드들은 키값의 범위와 다른 노드들에 대한 포인터 값을 저장
  - leaf node는 실제 값 또는 DB 내의 실제 data record에 대한 참조 값(Row ID)을 가짐

- 종류

  - 컬럼 값의 중복 여부

    - Unique Index
      - 중복이 없는 유일 값을 가지는 컬럼에 대한 Index
      - 기본키 / 고유키 무결성 제약조건 설정 시, 자동 생성
      - 명시적 생성 : `CREATE UNIQUE INDEX 인덱스이름 ON 테이블 (컬럼) ;`
    - Non-Unique Index
      - 중복된 값을 가지는 컬럼에 대한 Index
      - `CRATE INDEX 인덱스이름 ON 테이블 (컬럼) ;`

  - 컬럼의 결합 여부

    - 단일 인덱스
      - 하나의 컬럼만으로 구성된 인덱스
      - `CRATE INDEX 인덱스이름 ON 테이블 (컬럼) ;`
    - 결합 인덱스
      - 두 개 이상의 컬럼을 결합하여 생성
      - WHERE 조건에서 두 개 이상의 컬럼이 AND로 자주 연결되어 사용되는 경우 생성
      - `CRATE INDEX 인덱스이름 ON 테이블 (컬럼1, 컬럼2) ;`

  - 연산자 또는 함수의 적용

    - 함수기반인덱스

      - 일반적으로 컬럼에 함수 사용 시 인덱스 사용 불가능
      - 함수나 수식으로 계산된 결과에 대해 인덱스 생성 제공

      ```SQL
      SELECT MEMBERID, NAME, EMAIL
      FROM MEMBER
      WHERE UPPER(NAME) LIKE 'A%';
      ```

      - `CRATE INDEX MEMBER_FIX01 ON MEMBER (UPPER(NAME)) ;`
      - :bulb: 함수 기반 인덱스 생성보단 변형을 가하지 않는 대안 찾는 것이 중요

- 사용 지침

  - 한 테이블에 여러 인덱스가 있더라도 여러 인덱스가 하나의 SQL에서 동시에 사용 X

  - 결합 인덱스를 구성하는 컬럼 중, 첫 번째 컬럼이 WHERE 조건에 지정되어야 해당 인덱스가 정상 사용

  - 테이블의 크기가 적은 것은 인덱스를 사용 않는 것이 유리

  - 넓은 범위를 인덱스로 처리시 성능 감소

    - 읽을 레코드의 건수가 전체 레코드의 20~25%를 넘어서면 인덱스를 타지 않는 것이 효율적

      - 인덱스를 통합 탐색 = 1. 인덱스로 Reference 찾음 + 2. Reference 를 통해 레코드를 찾음

        =>  1건당 비용이 FULL SCAN보다 4~5배 정도 비쌈

    - 해당 경우, 옵티마이저가 FULL SCAN으로 자체 판단

  - 새로 추가된 인덱스는 기존 엑세스 경로에 영향 줄 수 있음

    => 영향도 파악 필요

- 인덱스 사용 X 경우

  | 유형                          | 설명                                                         | 권고안                                                       |
  | ----------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
  | INDEX 구성 컬럼의 외부적 변형 | 함수 사용, 연산 수행 등으로 변형                             | INDEX 컬럼에 대한 외부적 변형 제거                           |
  | INDEX 구성 컬럼의 내부적 변형 | 데이터 형이 상이한 컬럼 간 조인, 데이터 형에 부적절한 값의 대응과 같은 내부적 변형 발생 | 데이터 모델 상의 적합성 정비<br />데이터형에 적합한 연상 수행 유도 |
  | 부정형 비교                   | 부정형 비교 (!=, <>, NOT IN) 수행                            | 가능한 부정형 제거                                           |
  | NULL 값 비교                  | IS (NOT) NULL 비교 => 인덱스에는 NULL 미존재하므로 사용 X 경우 발생 | 테이블 생성 시 인덱스 컬럼에 DEFAULT 값 적용으로 NULL 값 제거 |
  | 기타                          | 결합 index 사용 부적절                                       | 선행 컬럼의 조건 사용                                        |
  | 기타                          | %가 앞에 나오는 등 LIKE 비교 부적절                          | BETWEEN, <= 같은 연산자 사용 및 사용자 입력 값 검토          |
  | 기타                          | HAVING 절의 조건 추가<br />(Having 절에는 Index 미적용)      | WHERE 절에서 조건 추가                                       |



### 조인 처리 방식

- NESTED LOOP JOIN
  - 가장 보편적 조인
  - 선행테이블의 데이터와 매치되는 값을 후행테이블에서 찾아옴
  - 선행테이블에서 추출되는 데이터 건수만큼 후행테이블을 반복 엑세스
  - 후행테이블에 인덱스가 있으면 해당 값을 빨리 찾아올 수 있음
- SORT MERGE JOIN
  - 두 테이블을 각각 읽어 조인 컬럼을 정렬한 후, 결과를 합쳐 조인하는 방식
  - 항상 정렬 작업 발생 => 정렬 크기에 따라 자원 소모 클 수 있음
  - 테이블 순서 상관 X

- HASH JOIN
  - 선행 조인 컬럼의 HASH 테이블을 메모리에 만들고, 후행 테이블의 조인 컬럼과 일치하는 값을 선별
  - SORT MERGE의 시스템 자원 소모의 대안으로 등장
  - 대용량 테이블 조인에 주로 사용


:bulb: 선행테이블, 후행 테이블

- 선행 테이블 (driving table) : 조인 시, 먼저 수행되는 테이블
- 후행 테이블(inner table) : 조인 조건을 상수로 제공받아 나중에 수행되는 테이





