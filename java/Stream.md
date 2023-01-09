# Stream



## 개념



### Lambda

- 정의
  - 익명 함수 지칭
- 특징
  - 일급 객체
    - 다른 객체들에 적용 가능한 연산을 모두 지원하는 객체
    - 함수를 값 / 파라미터 / 변수 대입 등에 사용 O
  - 람다내 사용되는 지역변수는 final이 붙지 않아도 상수로 간주
  - 람다식 내에서 현재 클래스 이름 호출 시, 람다식을 감싼 클래스 명 반환
  - 람다식 밖의 변수 사용 시, final 필요
- 장단점
  - 장점
    - 코드 간결
    - 지연연산 - 불필요한 연산 최소화
    - 병렬처리
  - 단점
    - 재사용 X
    - 디버깅 어려움

- 3단 구성
  - argument list
  
  - arrow token
  
  - body
  
- Functional Interface인 객체에 대해서 사용 O

  - `@FunctionalInterface`
  - 여러개의 `default`, `static` 메소드가 있더라도 추상 메서드가 오직 한 개
  - lambda 식의 argument list 와 매핑되는 메소드 실행됨
  - `Collections.sort( employess, ( e1, e2) ->  o1.getName().compareTo(o2.getName()));`

- body에 여러 라인 작성 시 {} 및 ; 필요

- 객체 타입이 정해져 있는 경우, argument list에 클래스 타입 생략 O





### Stream 이란

- 데이터의 흐름
  - 배열 또는 컬렉션 인스턴스에 함수 여러개를 조합해서 원하는 결과 필터링 , 가공
- 배열과 컬렉션의 함수형 처리 O
  - 코드 양 :arrow_down: , 간결 표현 O
- 병렬처리 O
  - 