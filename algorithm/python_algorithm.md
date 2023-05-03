# 파이썬 알고리즘



## 목차

- 기본
  - 변수 선언
  - 출력
  - 컨프리헨션
- Control Flow
  - 제어문
  - for loop
- 자료구조
  - 딕셔너리(dict)
  - 리스트
  - 문자열
  - 세트(Set)
  - 튜플
- 함수 및 연산자
  - 비트 연산자
- 클래스
  - Class

- 주요 Library
  - Collection





## 기본

### 변수 선언

  Type 명시 없이 자동으로 배정됨

```python
a ,b = 3 ,3
result = 800
```



### 출력

```python
print(f'{a} + {b} + {result:04.2f}')	# 3 + 3 + 800.00
```



### 컨프리헨션


```python
li = [i for i in range(5) if i >2] # [3, 4]

# 제너레이터
(i for i in range(5) if i >2) # 일회성
```



## Control Flow



### 제어문

```python
a = 5
if(a <= 3):
   print("a <= 3")
elif(a > 7):
   print("a > 7")
else:
   print(" 3< a <=7")
```



### for loop

```python
# for item in iterable  ex) list , tuple , range(n) , enumerate

myTuple = 1,2,3,4,5
for i in myTuple:
    print(i, end = ' ')
print('\n')
for i in enumerate(myTuple):
    print(i, end=' ')
```

    1 2 3 4 5 
    
    (0, 1) (1, 2) (2, 3) (3, 4) (4, 5) 





## 자료 구조

### 딕셔너리 (dict)

#### 특징

- 변경할 수 있고 (mutable)
- 순서가 없고
- 순회 가능

 

```python
# 딕셔너리
dic = {'a':1 , 'b':2 , 'c':3}

# 딕셔너리 update
dic['a'] = 0 					# 수정
dic['d'] = 4 					# 추가
del(dic['d'])					# 삭제
dic.update({'d' : 4 , 'e' : 5}) # dic 붙이기, key가 같다면 떺어씌우기

# 딕셔너리 메소드
dic.get('a')					# 0
dic.get(key [,default] )		# key 없어도 default 값 설정 o (기본 None)
dic.pop(key, [,default] )		# key 제거하면서 값 반환. key 없어도 default 값 설정 o

list(dic.keys())				#['a', 'b', 'c']
list(dic.values())				# [0, 2, 3]
list(dic.items()) 				# [('a', 0), ('b', 2), ('c', 3)]

```



#### Dictionary Comprehension

```python
{key : value for <변수> in <iterable> if <조건식> }
```



### 리스트

#### 특징

- 변경할 수 있고 (mutable)
- 순서가 있고
- 순회 가능



```python
# 리스트 선언
li = [1,2,3,4,5]

# 리스트 index , 슬라이스
li[0] 				# 1
li[0::2]			# [1,3,5]

# 리스트 update
li.append(6) 		# 뒤에 추가
li.insert(3, 3.5) 	# 중간 추가
li.extend(iterable)	# iterable을 뒤에 추가

li.pop() 			# 마지막 삭제
li.pop(3) 			# 해당 index 삭제
li.remove(5) 		# 해당 value 삭제
del(li[i])			# li 자체 삭제
li.clear() 			# 모든 항목 삭제

# 리스트 메소드
len(li) 			# 4
sum(li) 			# 10
li.index(2)			# 1
li.count(1) 		# 1
li.index(4) 		# 3
li.sort()
li.sort(reverse=True)
li.reverse() 		# 순서를 반대로
sorted(li) 			# [1,2,3,4]

# 리스트 언패킹
a ,*b = li			# a: 4 , b : [3, 2, 1]
```



#### 깊은 복사

-  slice 연산자
-  list() 함수
-  copy 모듈의 copy.deepcopy( list )



#### zip

- `zip(*iterables)`

  - 복수의 iterable을 모아 튜플을 원소로 하는 zip object를 반환

  :bulb: `*` : 여러 개의 인수를 받을 때 붙임. 발음 : asterisk

- 변환

  ```python
  last_names = ["Jack","Matt","Obama"]
  middle_names = ["D","S","O"]
  first_names = ["Cock", "Bal", "Care"]
  
  result_list = list(zip(last_names,middle_names,first_names))
  result_dict = dict(zip(last_names,first_names))
  
  print (result_list)		# [('Jack', 'D', 'Cock'), ('Matt', 'S', 'Bal'), ('Obama', 'O', 'Care')]
  print (result_dict)		# {'Jack': 'Cock', 'Matt': 'Bal', 'Obama': 'Care'}
  ```

  - **list**로 바꾸면 각 **원소가 tuple**로 생성
  - **dictionary를** 만드는 건 2개를 합친 zip 오브젝트만 가능

 

### 문자열

#### 특징

- 변경할 수 없고 (Immutable)
- 순서가 있고
- 순회 가능

#### 메소드

- `~.find(x)` : x 의 첫 번째 위치 반환. 없으면 -1
- `~.index(x)` : x 의 첫 번째 위치 반환. 없으면 `오류`
- `~.replace(old , new [, count ] )` : count 지정하면, 해당 개수만큼 시행
- `~.strip( [chars] )` : 특정 문자들을 지정하면 양쪽에서 제거. 지정 x -> 공백 제거
  - `~.lstrip`
  - `~.rstrip`
- `~.split( [chars] ) `: 문자열을 특정한 단위로 나눠 리스트 반환
- `'separator'.join( [iterable] )` : 반복 가능한 컨테이너 요소들을 seperator로 합쳐 문자열 반환
- 대소문자 관련
  - `~.capiatalize()` : 앞글자를 대문자
  - `~.title()`  : 처음 등장 글자 대문자
  - `~.upper()`
  - `~.lower()`
  - `~.swapcase()` : 대 <-> 소문자로 변경
- 검증
  - `~.isalpha()` : 유니코드 상 Letter(한국어 포함)
  - `~.isupper()`
  - `~.islower()`
  - `~.istitle()`
  - `~.isdecimal()` <`isdigit()` < `isnumeric()`



### 세트 (Set)

#### 특징

- 변경할 수 있고 (mutable)
- 순서가 없고
- 순회 가능




```python
# set
mySet = {1,2,3, 1} 		# {1,2,3}

# 연산자
{1 ,2 ,3 } & {2, 3 ,4 }	#{2, 3}
{1 ,2 ,3 } | {2, 3 ,4} 	#{1, 2, 3, 4}
{1 ,2 ,3 } - {2, 3 ,4} 	#{1}
{1 ,2 ,3 } ^ {2, 3 ,4} 	#{1, 4}

# 메서드
mySet.add( elem )
mySet.update( *others ) 	# 여러 값을 추가
mySet.remove( elem )		# 없으면 KeyError
mySet.discard( elem )		# 없어도 No error
mySet.pop() 				# 임의 원소 제거해 반환
```



### 튜플

#### 특징

- 변경 할 수 없고
- 순서가 있고
- 순회 가능



```python
# 튜플 선언
tu = (1,2,3) # or 1,2,3

# 튜플 index
tu[1] 		# 2

# 변환 => list
li = list(tu)
```

 

## 함수



### 함수


```python
def func(arg1 , *arg2 , **arg3): # arg1 : 변수 , arg2: tuple , arg3: dict
    print(f"{arg1} , {arg2} , {arg3}")
    
func(1,2,3,4,5, a = 1 , b = 2)
```

    1 , (2, 3, 4, 5) , {'a': 1, 'b': 2}



### 람다식

```python
# lambda 매개변수 : 리턴값
multiply = lambda a,b: a*b
multiply(2,5)				# 10
```



### map & filter


```python
# map
# map(함수이름 , 리스트) : 리스트 값을 순차적으로 함수에 대입
list(map(lambda a:a+5 , [1,2,3]))		# [6, 7, 8]
```


```python
# filter
# filter(함수이름 , 리스트) : 리스트 값을 순차적으로 함수에 대입하여 true 인것만 반환
list(filter(lambda a:a<3 , [1,2,3]))	# [1, 2]
```



### 비트 연산자

- & : 비트 단위로 AND
- | : 비트 단위로 OR
- `<<` : 피연산자의 비트 열을 왼쪽으로
- `>>` : 피연산자의 비트 열을 오른쪽으로

### 비트 연산자를 이용해 부분 집합 전체 탐색

```python
arr = [3,6,7,1,5,4]

n = len(arr)
count = 0
for i in range(1<<n):			# 1<<n : 부분 집합의 개수
    for j in range(n):			# 원소의 수만큼 비트를 비교
        if i & (1<<j):			# i의 j번째 비트가 1이면 j번째 원소 출력
            print(arr[j], end=" ")
    print()
```



## 클래스

### Class

```python
# 클래스 선언
class Car:
    __speed = 100
    def __init__(self, speed):
        self.__speed = speed
    def print(self):
        print(self.__speed)

myCar = Car(500)
myCar.print()				# 500
```

```python
# 상속
class smallCar (Car):
    def __init__(self, size):
        self.size = size
    
    def print(self):
        super().print()
        print(self.size)

myCar = smallCar("small")	
myCar.print()				# 100 small
```



## 주요 Library

### Collection

```python
# OrderedDict : Dict의 순서 유지
from collections import OrderedDict

d ={'a' : 1 , 'c' : 3 , 'b' :2}
orderDict = OrderedDict(sorted(d.items(), key = lambda d:d[0]) )
orderDict.update({'d' : 4})
orderDict					# OrderedDict([('a', 1), ('b', 2), ('c', 3), ('d', 4)])
```


```python
# Counter
from collections import Counter

# Counter : list의 element별 개수를 dictionary로
li = [0,0,1,1,1,2,3,4,4,6,6,6,6]
li_C = Counter(li)
li_C						# Counter({0: 2, 1: 3, 2: 1, 3: 1, 4: 2, 6: 4})
```

