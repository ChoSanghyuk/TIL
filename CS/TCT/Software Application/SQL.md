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

  - 





























