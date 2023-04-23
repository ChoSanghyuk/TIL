# 자바 알고리즘 주요 메소드 정리

## 목차

- Array
- ArrayList
- BufferedReader
- Character
- Comparable vs Comparator
- Collections
- Integer
- Map
- Math
- Queue
- Set
- Stream
- String
- StringBuilder
- 기타



## Array

```java
Arrays.sort(array);   					// arr가 정렬됨. void 반환
Arrays.sort( array , <comparator> ) 
arr.length 
Arrays.copyOfRange( arr, begin, end ) ;  // arr을 begin에서 end-1까지 
```

### 초기화

```java
Arrays.asList(new String[] {"a", "b", "c"});
```

### arrayList -> array

```java
String[] array = arrayList.toArray(new String[arrayList.size()]);
```
### array -> arrayList

```java
ArrayList<String> arrayList = new ArrayList<>(Arrays.asList(array))
```

### 2차원 배열 정렬

```java
Arrays.sort(meetings, new Comparator<int[]>() {
    @Override
    public int compare(int[] a1, int[] a2) {

        return Integer.compare(a1[1], a2[1]) ; 
    }
});
```

- 두번째 열을 기준으로 오름차순 정렬



## ArrayList

```java
arrLi.add(ele)				 // arrayList에 변수 추가  	/  (i, 요소) // i 번째 자리에 요소 추가
arrLi.addAll(arrayList)		 // arrayList에 arrayList 의 값들 다 넣어줌
arrLi.size()  				 // arrayList의 크기 반환
arrLi.get(i)  				 // arrayList의 i번째 요소 반환
arrLi.set( i , ele) 		 // arrayList의 i번째 자리에 요소를 대체함
arrLi.remove(i) 			 // arrayList의 i번째 자리 요소를 없앰
arrLi.contains(ele) 		 // arrayList에 요소가 있으면 참 반환
arrLi.indexOf(ele) 			 // arrayList에서 요소의 위치 반환. 없으면 음수
```



## BufferedReader

### 선언 및 입력 받기

```java
BufferedReader bf = new BufferedReader(new InputStreamReader(System.in)); 	//선언

// 파일에서 입력
FileReader fr = new FileReader("파일명");
BufferedReader bf_f = new BufferedReader(fr);

String s = bf.readLine(); 
int i = Integer.parseInt(bf.readLine()); 

```

- 예외처리 필요함. 주로 `throws IOException`을 통해서 처리함



### 공백단위 데이터 분리

```java
StringTokenizer st = new StringTokenizer(s); //StringTokenizer인자값에 입력 문자열 넣음
int a = Integer.parseInt(st.nextToken()); 
int b = Integer.parseInt(st.nextToken()); 
```

- `StringTokenizer`의 `nextToken()` => 공백 단위로 구분하여 순서대로 호출

```java
String array[] = s.split(" ");
```

- `String`의 `split()` 사용



## BufferedWriter

### 선언

```java
BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
```

### 출력

```java
String s = "abcdefg";   //출력할 문자열

bw.write(s+"\n");   	//버퍼에 있는 값 전부 출력
bw.newLine();			// 개행
    
bw.flush();   			//남아있는 데이터를 모두 출력시킴 (close 전엔 수행)
bw.close();   			//스트림을 닫음
```



## Charactor

```java
Character.isDigit( <char> )	// 해당 문자가 숫자인지 판단
```





## Comparable vs Comparator

`Comparable`

```java
class Point implements Comparable<Point>{
    int x, y;
    
    @Override
    public int compareTo(Point p){
        if(this.x > p.x){
            return 1; 			// x에 대해 오름차순
        }
        else if(this.x == p.x){
            if(this.y < p.y){	// y에 대해 내림차순
                return 1;
            }
        }
        return -1;
    }
}
```

`Comparator`

```java
class MyComparator implements Comparator<Point>{
    @Override
    public int compare(Point p1, Point p2){
        if (p1.x > p2.x){
            return 1;		// x에 대해 오름차순
        }
        else if (p1.x == p2.x){
            if (p1.y < p2.y){
                return 1;	// y에 대해 내림차순
            }
        }
        return -1;
    }
}
```



## Collections

```java
import java.util.Collections

Collections.shuffle( <Collection> )	 				// 리스트 섞어줌
Collections.min(<Collection>);
Collections.max(<Collection>);
Collections.swap(<Collection>, i , j )  				// 리스트에서 i랑 j 번째 요소 위치 바꿔줌
Collections.sort(ArrayList, Collections.reverseOrder());// 역순으로
Collections.copy(dList, sList)							// dList // destination list , sList // source list
														// shallow copy로 복사 진행.
														// dList의 size가 sList의 size보다 커야함

```



## Heap

```java
PriorityQueue<Integer> pq = new PriorityQueue<>();
PriorityQueue<Integer> maxPq = new PriorityQueue<>(Collections.reverseOrder());

pq.add(~);			// ~ 추가 , 성공시 true 반환
pq.offer(~) ; 		// ~ 추가 
pq.peek() ; 		// 첫번째 값 참조
pq.poll() ; 		// 첫번째 값 반환하고 제거
pq.remove() ; 		// 첫번째 값 제거
pq.clear() ; 		// queue 초기화
```





## Integer

```java
Integer.parseInt( <String> ) ;
Integer.toString( ~ );
```



## Map

```java
import java.util.HashMap;
Map<String, String> languages = new HashMap<>();

~.put( key , value )  					// 추가
~.get(key)  							// return 값 
~.containsKey(key) 						// 있으면 true nor false
~.keySet()  							// key 값들 반환
~.values()
~.remove(key) 
~.remove(key, value) 					// 해당 키의 value 내용을 지움
~.replace(key, value) 					// 키의 값을 value로 전환. 전환되면 true nor false
~.replace(key, value1 , value2) 		// 키의 value1을 value2로 전환.
~.getOrDefault( key , defaultvalue ) 	// ~에 키값이 있으면 키값의 value nor default value 반환

```



## Math

```java
import java.lang.Math

Math.floorDiv(26,10) 		//몫 2
Math.floorMod(26,10) 		//나머지 6
Math.pow( 밑 , 지수 )		  //제곱
Math.ceil(~)				//올림
Math.floor(~)				//내림
Math.round(~)				//반올림
```



## Queue

```java
Queue<Integer> queue = new LinkedList<>();

queue.add(~);		// ~ 추가 , 성공시 true 반환
queue.offer(~) ; 	// ~ 추가 
queue.peek() ; 		// 첫번째 값 참조
queue.poll() ; 		// 첫번째 값 반환하고 제거
queue.remove() ; 	// 첫번째 값 제거
queue.clear() ; 	// queue 초기화
```



## Set

```java
Set<Integer> squares = new HastSet<>( <Collections> );

set.add( ~ )
set.addAll( <Collections> ) 			// <Set>의 요소들을 다 넣음
set.retainAll( < Collections > ) 		// ~에서 < Collections >과 공통 부분만 남김
set.removeAll( < Collections >) 
set.containsAll(< Collections >) 		// 전부 포함하고 있으면 true
set.size()

```



## Stack

``` java
Stack<타입 > stack = new Stack<>();

~.push(Element item); 	// 데이터 추가
~.pop(); 				// 최근에 추가된(Top) 데이터 삭제
~.peek(); 				// 최근에 추가된(Top) 데이터 조회
~.empty(); 				// stack의 값이 비었는지 확인, 비었으면 true, 아니면 false
~.get( i ) ; 			// i 위치 값 반환
~.seach(Object o); 		// 인자값으로 받은 데이터의 위치 반환

```



## Stream

```java
import java.util.strea
coll객체.stream()
	.map( String::toUpperCase)		// s -> s.UpperCase 도 가능
	.filter( s -> s.startsWith("~") )  
	.sorted()
	.forEach( System.out::println ) ;

 Arrays.stream(dayOfend).filter(i -> i!=0).toArray();
```

`collect`

  - Stream의 데이터를 처리를 하고 원하는 자료형으로 바꿔줌
    ~.collect( Collectors.toList() )
 ```java
  Map< T1, T2 > 이름 = collect변수.
			stream().
			collect( Collectors.groupingBy( e -> e.getAge() ));
 ```
`reduce`
  - 스트림을 입력받아 반환값을 내보냄
    ~.reduce( (e1 , e2) -> e1 < e2 ? e1: e2)



## String

```java
str.length();
str.charAt();
str.substring(start) 
str.substring(start,end)  				// start위치 부터 end전까지 문자열 발췌
str.indexOf(target);
str.lastIndexOf("/")
str.replaceAll( a , b )  				// a -> b
String[] array = str.split("#");
str.startsWith( "~" ) ;
str.matches( "regex" ) ;   				// 정규표현식과 전부 일치하면 true nor false
str.equals( str2 )  					// str이 null이면 exception.
str.equalIgnoreCase( str2 ) 
str.compareTo( str2 ) 					// 같으면 0, str이 작으면 음수, 크면 양수
str.toCharArray()						// char array로 변환
```

- == 연산은 메모리 참조가 같은지 물어봄. .equals()을 통해 값이 같은지 비교



## StringBuilder

```java
StringBuilder sb = new StringBuilder() ; 

sb.append(~) ; 
sb.reverse() ;
sb.toString() ; 
sb.charAt( i ) ; 
sb.deleteCharAt( i ) ;
```



## 기타

- 객체 확인

```java
a instanceof A 	// a가 A의 instance면 true
```

