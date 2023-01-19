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
- 외부 지역 Array 변수에 값을 추가하는 것은 Array 자체에 변화가 없기에 가능





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
  - `antThen()`
    - `Function.andThen()`은 인자로 Function 객체를 받으며, 다수의 Function들을 앞에서 부터 순차적으로 실행
    - `String result = func1.andThen(func2).apply(10)`
    - `Function<T,R> func3 = func1.andThen(func2)`

  - `compose()`
    - andThen()과 같은 역활. 다수의 Function들을 뒤에서 부터 순차적으로 실행


| Inteface  | Functional Method | Number of Arguments | Returns a value | Can be chained |
| --------- | ----------------- | ------------------- | --------------- | -------------- |
| Consumer  | accept()          | 1 or 2(Bi)          | No              | Yes            |
| Supplier  | get()             | 0                   | Yes             | No             |
| Predicate | test()            | 1 or 2(Bi)          | Yes             | Yes            |
| Function  | apply()           | 1 or 2(Bi)          | Yes             | Ye             |





## Stream




### Stream 이란

- 데이터의 흐름
  - 배열 또는 컬렉션 인스턴스에 함수 여러개를 조합해서 원하는 결과 필터링 , 가공
- 배열과 컬렉션의 함수형 처리 O
  - 코드 양 :arrow_down: , 간결 표현 O
- 병렬처리 O



### 생성

- 배열 스트림

  - `Arrays.stream()` 사용

  ```java
  String[] arr = new String[]{"el1" , "el2", "el3"};
  Stream<String> stream1 = Arrays.stream(arr);
  Stream<String> stream2 = Arrays.stream(arr,1,3); // 1~2번 요소만 Stream으로 생성 
  ```

- 컬렉션 스트림

  - 디폴트 메소드 `stream` 을 이용

    ```java
    List<String> list = Arrays.asList("el1", "el2", "el3");
    Stream<String> stream = list.stream();
    Stream<String> parallelStream = list.parallelStream(); // 병렬 처리 스트림
    ```

- 스트림 초기화

  - `Stream.of()` 사용

    ```java
    Stream<String> stream1 = Stream.of("el1", "el2", "el3");
    ```

- 스트림 연결

  - `Stream.concat()` 사용

    ```java
    Stream<String> concat = Stream.concat(stream1, stream2);
    ```

    




### 가공

- filter
  - 스트림 내 요소들을 하나씩 평가해서 걸러내는 작업
  - `Stream<T> filter(Predicate<? super T> predicate);`

- map
  - 스트림 내 요소들을 하나씩 특정 값으로 변환
  - `<R> Stream<R> map(Function<? super T, ? extends R> mapper);`

- sorted
  - 정렬
  - `Stream<T> sorted(Comparator<? super T> comparator);`
  
- peek()

  - 결과에 영향 X, 특정 연산 수행

  - 주로 중간 결과 확인용으로 사용

  - `Stream<T> peek(Consumer<? super T> action);`


- flatmap()

  - 새로운 스트림을 생성해서 리턴

  - 중첩 구조를 한 단계 제거하고 단일 컬렉션으로 만들어줌

  - `<R> Stream<R> flatMap(Function<? super T, ? extends Stream<? extends R>> mapper);`

    ```java
    // 여러 department에 나누어져 있던 employee들을 하나로 묶어서 처리 
    departments.stream().
        .flatMap(department -> department.getEmployees().stream()) 
        .peek(System.out::println)
    ```



### 결과 만들기

- collect

  - Collector 타입의 인자 받아서 처리

  - Collectors.toList()

    - 스트림에서 작업한 결과 리스트로 반환

      ```java
      List<String> collectorCollection =  productList.stream()
          										.map(Product::getName)
          										.collect(Collectors.toList());
      ```

  - Collectors.groupingBy()

    - 값을 기준으로 요소들을 그룹핑 => Map 반환

      ```java
      Map<Integer, List<Product>> collectorMapOfLists =
       productList.stream()
        .collect(Collectors.groupingBy(Product::getAmount));
      ```

  - Collectors.partitioningBy()

    - 조건식 결과를 기준으로 요소들을 그룹핑 => Map 반환

      ```java
      Map<Boolean, List<Product>> mapPartitioned = 
        productList.stream()
        .collect(Collectors.partitioningBy(el -> el.getAmount() > 15));
      // true, false로 Map key 구성됨
      ```

      

- reduce

  - 파라미터

    - accumulator : 각 요소를 처리하는 계산 로직 (요소 오면 중간 결과 생성)
    - identity : 계산을 위한 초기값
    - combiner : 병령 스트림에서 나눠 계산한 결과 하나로 합침

  - 파라미터 개수별 위치

    ```java
    // 1개 (accumulator)
    Optional<T> reduce(BinaryOperator<T> accumulator);
    
    // 2개 (identity, accumulator)
    T reduce(T identity, BinaryOperator<T> accumulator);
    
    // 3개 (identity, accumulator)
    <U> U reduce(U identity,
      BiFunction<U, ? super T, U> accumulator,
      BinaryOperator<U> combiner);
    
    // BinaryOperator : 같은 타입의 인자 두개 받아서 같은 타입 결과 반환하는 함수형 인터페이스
    ```

  - 예시

    ```java
    Integer reducedParallel = Arrays.asList(1, 2, 3)
      .parallelStream()			// 병렬 스트림
      .reduce(10,				// 초기값
              Integer::sum,		// accumulator
              (a, b) -> {		// combiner
                System.out.println("combiner was called");
                return a + b;
              });
    ```

    1. 1,2,3이 각각 병렬로 실행
    2. 각각 초기값 10과 더해짐
    3. combiner로 병렬 스트림 결과값 합쳐짐
    4. 11 + 12+ 13 = 36




### 기타 메소드

- distinct()
- count()



### method reference

- Special type of lambda expression
- 표현 간결해짐
- 종류
  - Static methods
  - Instance methods of particular objects
  - Instance methods of an arbitrary object of a particular type
  - Constructor
- 예시
  - `System.out::println`



:bulb: 참고자료 

https://futurecreator.github.io/2018/08/26/java-8-streams/

















