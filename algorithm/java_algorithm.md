# 자바 알고리즘 주요 메소드 정리

## 목차

- Array
- ArrayList
- Character
- Comparable vs Comparator
- Collections
- Integer
- Map
- Math
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

  * arrayList -> array
```java
String[] array = arrayList.toArray(new String[arrayList.size()]);
```
  * array -> arrayList
```java
ArrayList<String> arrayList = new ArrayList<>(Arrays.asList(array))
```



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

Collections.shuffle( ( <Collection>)	 				// 리스트 섞어줌
Collections.min(( <Collection>);
Collections.max(( <Collection>);
Collections.swap(( <Collection>, i , j )  				// 리스트에서 i랑 j 번째 요소 위치 바꿔줌
Collections.sort(ArrayList, Collections.reverseOrder());// 역순으로
Collections.copy(dList, sList)							// dList // destination list , sList // source list
														// shallow copy로 복사 진행.
														// dList의 size가 sList의 size보다 커야함

```



## Integer

```java
Integer.parseInt( <String> )
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
~.replace(key value) 					// 키의 값을 value로 전환. 전환되면 true nor false
~.replace(key, value1 , value2) 		// 키의 value1을 value2로 전환.
~.getOrDefault( key , default value ) 	// ~에 키값이 있으면 키값의 value nor default value 반환

```



## Math

```java
import java.lang.Math

Math.floorDiv(26,10) 		//몫 2
Math.floorMod(26,10) 		//나머지 6
Math.pow( 밑 , 지수 )		  //제곱
Math.ceil(~)				//올림
Math.floor(~)				//내림
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



## Stream

```java
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
str.matches( "~~" ) ;   				// ~~와 전부 일치하면 true nor false
str.equals( str2 )  					// str이 null이면 exception
str.equalIgnoreCase( str2 ) 
str.compareTo( str2 ) 
```



## StringBuilder

```java
StringBuilder sb = new StringBuilder() ; 

sb.appned(~) ; 
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

