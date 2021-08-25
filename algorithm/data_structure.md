# 자료 구조

## 목차

- Queue
- Stack



## Queue

 ## 특징

- 선입선출

### 선형큐

- front : 마지막으로 꺼내진 위치. 초기값 -1

- rear : 저장된 마지막 원소의 인덱스. 초기값 -1

  => out은 front += 1 front 지점 원소 반환

  ​	  in은 rear+=1 & rear에 원소 추가

### 원형큐

- 초기값 : front = rear = 0. 

- front와 rear의 위치가 배열의 마지막 n-1 가르키고 , 다시 0으로 이동 <= 나머지 연산자 이용

  => out : (front + 1) mod n 위치 삭제

  ​      in  : (rear + 1) mod n 위치에 삽입

  **! 주의** 공백 상태와 포화 상태 구분을 위해 front 위치는 항상 빈자리로 둠

- 공백 상태 : front = rear

- 포화 상태 : (rear + 1) mod n = front



### 우선순위 큐

- 우선순위가 높은 순서대로 먼저 나감 => 시뮬레이션, 네트워크 트래픽 제어, 운영체제 스케쥴링

- 배열 ~> 우선순위 큐 구현 : 우선순위 비교하여 적절한 위치에 삽입

  => 삽입 삭제시 시간 or 메모리 소요 큼

```java
Queue<Integer> queue = new LinkedList<>();

queue.add(~);		// ~ 추가 , 성공시 true 반환
queue.offer(~) ; 	// ~ 추가 
queue.peek() ; 		// 첫번째 값 참조
queue.poll() ; 		// 첫번째 값 반환하고 제거
queue.remove() ; 	// 첫번째 값 제거
queue.clear() ; 	// queue 초기화
```



## Stack

### 특징

- 선형구조
- 후입선출

``` java
Stack<타입 > stack = new Stack<>();

~.push(Element item); 	// 데이터 추가
~.pop(); 				// 최근에 추가된(Top) 데이터 삭제
~.peek(); 				// 최근에 추가된(Top) 데이터 조회
~.empty(); 				// stack의 값이 비었는지 확인, 비었으면 true, 아니면 false
~.get( i ) ; 			// i 위치 값 반환
~.seach(Object o); 		// 인자값으로 받은 데이터의 위치 반환

```





