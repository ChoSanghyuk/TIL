# Test Driven Development



## 개요



### General Flow

- Red => Green => Refactor
  - :red_circle: A new test will initially fail
  - :green_apple: We work to get the test to pass
  - :large_blue_circle: We finnaly optimize our code, and test



### what to test

- each test will cover a single scenario for a single piece of logic



## General Rules



### Rules of TDD

1. Start with failing test
   -  새로 추가한 테스트가 테스트 케이스에 제대로 들어갔는지 확인

2. Test the expected outcome of an example
3. Don't pre-judge design. Let your tests drive it
4. Write the mininum code required to get you tests to pass
5. Each test should validate one single piece of logic



### Rules of Testing

- test one item of functionality only
  - one `assert` for each test
- test business logic, not methods
- tests must be repeatable, and consistent
  - 코드가 변경되었다면, 이전 테스트들까지 재실행
  - 테스트 결과는 언제 실행되어도 결과가 동일해야 함
- test must be thorough
  - 모든 조건을 테스트할 수 있어야 함



### What tests should I write

- What should the logic be?
- What is the opposite to that logic?
- Are there any edge cases?
- Are there any error conditions?



:bulb: Extra Tip

- 테스트 케이스(Input)의 종류별로 메소드를 새로 생성하는 것이 오류 발견 지점 찾기 쉬움



## JUnit



### IntelliJ 설정

1. 프로젝트 생성
2. test 폴더 생성
3. 우클릭 => Mark Directory as test resources root
4. test 폴더 내에 project 코드랑 동일한 package 생성
5. File => project structure => import library from maven : JUnit
6. 클래스 생성
7. 메소드에 `@Test` 어노테이션 부착 후 옆 화살표로 단건 혹은 전체 메소드 실행



### Eclipse 설정

1. 프로젝트 생성

2. test 폴더 생성

3. 우클릭 => Build Path => Use as Source Folder

4. New => JUnit Test Case

   - JUnit 버전 선택

   - test 폴더 내에 project 코드랑 동일한 package 이름으로 설정

   - 이름 설정

     :bulb: Class under test : 선택한 클래스의 모든 메소드 Test로 선택됨. TDD가 아닌 개발 완료 후 테스트 시 유용



### JUnit Static Method (version 5)

- `fail()`
  - 테스트 강제 실패

- `assertTrue(boolean result, String message)`

  - result가 true시 테스트 성공
  - 실패 시, message 반환

- `assertFalse(boolean result, String message)`

  - result가 false시 테스트 성공
  - 실패 시, message 반환

  ```java
  @Test
      public void checkValidISBN(){
          ValidationISBN validator = new ValidationISBN();
          boolean result = validator.checkISBN("0140449116");
          assertTrue(result, "First Value"); // True시 테스트 성공
  
          result = validator.checkISBN("0140177396");
          assertTrue(result, "Second Value");
      }
      @Test
      public void checkInvalidISBN(){
          ValidationISBN validator = new ValidationISBN();
          boolean result = validator.checkISBN("0140449117");
          assertFalse(result); // False시 테스트 성공
      }
  ```

- `Throwable assertThrows(Class<Throwable>, Executable)`

  - `Class<Throwable> ` : 예상되는 Exception 클래스 작성
  - Executable : 자신의 실행 코드를 lambda 식으로 작성

  ```java
  @Test
      public void nineDigitISBNAreNotAllowed(){
          ValidationISBN validator = new ValidationISBN();
          assertThrows(NumberFormatException.class, () ->  validator.checkISBN("140449117") );
      }
  ```




### Extra JUnit Asserts

- `assertEqual()`
- `assertNull(T t)`
- `assertNotNull(T t)`





## Refactoring



### :large_blue_circle: Optimize Code

- 우선 모든 Test를 다 통과하게끔 코드를 설계 후, 코드 최적화 진행

  => Refactoring 후 Test 재실행으로 코드 보장 O



## Stubs



Test code which has dependencies on third party

Third Party(databse, website)의 상태에 따라서 test 결과가 달라질 수 있음 (consistency X)



### Stub

- 가짜 함수 = 속이 빈 함수
- 단위 테스트 中 사용 케이스
  - 구현되지 않은 함수 or 라이브러리 제공 함수
  - 함수가 반환되는 값을 임의 설정
  - 복잡한 논리 흐름 속, 로직을 단순화



### Mock

- 진짜 객체와 비슷하게 동작. But, 프로그래머가 직접 행동을 관리
- DB 및 외부 API 테스트 시, 동작을 예측하여 구현 => API / DAO 등 구현 X 테스트 O



### Mockito

- Mock 객체를 쉽게 만들고, 관리하고, 검증할 수 있는 방법을 제공하는 프레임워크
- 공식 사이트
  - `site.mokito.org` 
- Dependency 추가
  - jar 필요 추가 => lib 등록
  - Spring 프로젝트 시, `spring-boot-start-test` dependency 추가 시 자동 등
  - :bulb: java 9 이상 버전 사용 시, `module-info.java` 파일 설정 필



### Mockito 사용

when, lookup, thenReturn



### Mockito Methods

- Mock 생성
  - `Myclass myClass = mock(Myclass.class);`
- 

```java
whem(myClass.myMethod(params)).thenReturn(retrun-value);

Verify(myClass, times(?)).myMethod(params); 
```











