# Volatile



## Volatile



### 개념

- Java 변수를 Main Memory에 저장
- 변수의 값을 Read 시, CPU cache에 저장된 값이 아닌, Main Memory에서 읽음
- 변수의 값을 Write 시, Main Memory에 접근해서 작성

### 필요 이유

- MultiThread 환경에서 각 Thread는 변수를 각각의 CPU Cache에 저장

  => 변수 값 불일치 문제

### 적합 Case

- MultiThread 환경에서 하나의 Thread가 Write, 나머지가 Read

  => 여러 Thread가 write 시, `synchronized`

  
