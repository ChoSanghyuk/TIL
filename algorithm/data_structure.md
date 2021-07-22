# 자료 구조

## 목차

- Queue
- Stack



## Queue

 **선입선출**

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

 **후입선출**

``` java
Stack<타입 > stack = new Stack<>();

~.push(Element item); 	// 데이터 추가
~.pop(); 				// 최근에 추가된(Top) 데이터 삭제
~.peek(); 				// 최근에 추가된(Top) 데이터 조회
~.empty(); 				// stack의 값이 비었는지 확인, 비었으면 true, 아니면 false
~.get( i ) ; 			// i 위치 값 반환
~.seach(Object o); 		// 인자값으로 받은 데이터의 위치 반환

```





