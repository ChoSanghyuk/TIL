# Stream



## Lambda



### Lambda 개념

- 정의
  - 익명 함수 지칭
- 특징
  - 일급 객체
    - 다른 객체들에 적용 가능한 연산을 모두 지원하는 객체
    - 함수를 값 / 파라미터 / 변수 대입 등에 사용 O
  - 람다내 사용되는 지역변수는 final 혹은 effectively final
  - 람다식 내에서 현재 클래스 이름 호출 시, 람다식을 감싼 클래스 명 반환
  - Functional Interface인 객체에 대해서 사용 O
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
    - 객체 타입이 정해져 있는 경우, argument list에 클래스 타입 생략 O
  
  - arrow token
  
  - body
    - body에 여러 라인 작성 시 {} 및 ; 필요
  



### Lambda 內 지역 변수는 final 혹은 effectively final

- effectively final
  - final이 선언되어 있지는 않지만, 변수의 재할당이 발생하지 X 변수
- 변수별 저장 위치
  - 지역 변수
    - 스택에 저장
    - 쓰레드 공유 X
  - 인스턴스 변수
    - 힙에 저장
    - 쓰레드 공유 O
  - 클래스 변수
    - 메소드 영역에 저장
    - 쓰레드 공유 O
- Lambda Capturing
  - lambda는  다른 쓰레드에서도 사용 O
  - 스택 영역의 변수를 사용할 수 있도록, 지역 변수의 복사본을 타 쓰레드에 제공
- 지역 변수는 쓰레드 공유 X => 타 쓰레드에서 lambda 사용 시, 값의 동기화 보장 X





### Functional Interface

- `@FunctionalInterface`
  - 여러개의 `default`, `static` 메소드가 있더라도 추상 메서드가 오직 한 개
  - lambda 식의 argument list 와 매핑되는 메소드 실행됨
  - `Collections.sort( employess, ( e1, e2) ->  o1.getName().compareTo(o2.getName()));`
- java.util.function
  - 자바 제공 FunctionalInterface
  - Supplier<T>
    - 추상 메소드 : `T get() `
  - Consumer<T>
    - 추상 메소드 : `void accept(T t) `
  - Function<T,R>
    - 추상 메소드 : `R apply(T t)`
  - Predicate<T>
    - 추상 메소드 : `Boolean test(T t)`
    - `Predicate <Integer> predicate = n -> n < 50 ; `
    - 구현 메소드
      - and 
        - 두 개의 predicate 결과 &&
        - `predicate1.and(predicate2).test(7))`
      - or
        - 두 개의 predicate 결과 ||
        - `predicate1.or(predicate2).test(7))`
      - isEqual
        - 일치 여부 반환
        - `Predicate<Integer> predicate = Predicate.isEqual(5);`
- Chaining
- bifunction - chainging 안됨?

| Inteface  | Functional Method | Number of Arguments | Returns a value | Can be chained |
| --------- | ----------------- | ------------------- | --------------- | -------------- |
| Consumer  | accept()          | 1 or 2(Bi)          | No              | Yes            |
| Supplier  | get()             | 0                   | Yes             | No             |
| Predicate | test()            | 1 or 2(Bi)          | Yes             | Yes            |
| Function  | apply()           | 1 or 2(Bi)          | Yes             | Ye             |








### Stream 이란

- 데이터의 흐름
  - 배열 또는 컬렉션 인스턴스에 함수 여러개를 조합해서 원하는 결과 필터링 , 가공
- 배열과 컬렉션의 함수형 처리 O
  - 코드 양 :arrow_down: , 간결 표현 O
- 병렬처리 O
  - 