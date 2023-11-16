# Python 기본 문법



## 주요 내용

- 제어문
- for loop
- 컴프리헨션
- 함수
- 람다식
- map & filter
- 예외처리
- Class
- Collection



## 제어문

```python
a = 5
if(a <= 3):
   print("a <= 3")
elif(a > 7):
   print("a > 7")
else:
   print(" 3< a <=7")
```



## for loop

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



## 컴프리헨션 


```python
li = [i for i in range(5) if i >2] # [3, 4]
# 제너레이터
(i for i in range(5) if i >2) # 일회성
```



## 함수


```python
def func(arg1 , *arg2 , **arg3): # arg1 : 변수 , arg2: tuple , arg3: dict
    print(f"{arg1} , {arg2} , {arg3}")
    
func(1,2,3,4,5, a = 1 , b = 2)
```

    1 , (2, 3, 4, 5) , {'a': 1, 'b': 2}



## 람다식

```python
# lambda 매개변수 : 리턴값
multiply = lambda a,b: a*b
multiply(2,5)				# 10
```



## map & filter


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



## 예외 처리


```python
try:
    raise NameError('HiThere')
except NameError:
    print('handled')
finally:
    print('end')
```

    handled
    end



# Class

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

## Collection

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
