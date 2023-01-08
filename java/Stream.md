# Stream



## 개념



### Lambda

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