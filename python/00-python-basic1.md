# Python의 변수타입과 자료구조

---
## 주요 내용

- 변수 선언
- 출력
- 리스트
- 튜플
- 딕셔너리
- set

---

## 변수 선언
  Type 명시 없이 자동으로 배정됨
```python
a ,b = 3 ,3
result = 800
```

## 출력
```python
print(f'{a} + {b} + {result:04.2f}')
```

    3 + 3 + 800.00



## 리스트

```python
# 리스트 선언
li = [1,2,3,4,5]

# 리스트 index , 슬라이스
li[0] 				# 1
li[0::2]			# [1,3,5]

# 리스트 update
li.append(6) 		# 뒤에 추가
li.insert(3, 3.5) 	# 중간 추가
li.pop() 			# 마지막 삭제
li.pop(3) 			# 해당 index 삭제
li.remove(5) 		# 해당 value 삭제
del(li[i])			# li 자체 삭제

# 리스트 메소드
len(li) 			# 4
sum(li) 			# 10
li.count(1) 		#1
li.index(4) 		#3
li.sort()
li.sort(reverse=True)
sorted(li) 			#[1,2,3,4]

# 리스트 언패킹
a ,*b = li			# a: 4 , b : [3, 2, 1]
```



## 튜플

```python
# 튜플 선언
tu = (1,2,3) # or 1,2,3

# 튜플 index
tu[1] 		# 2

# 변환
li = list(tu)
```

  :bulb: 튜플은 update 불가



## 딕셔너리

```python
# 딕셔너리
dic = {'a':1 , 'b':2 , 'c':3}

# 딕셔너리 update
dic['a'] = 0 					# 수정
dic['d'] = 4 					# 추가
del(dic['d'])					# 삭제
dic.update({'d' : 4 , 'e' : 5}) # dic 붙이기

# 딕셔너리 메소드
dic.get('a')					# 0
list(dic.keys())				#['a', 'b', 'c']
list(dic.values())				# [0, 2, 3]
list(dic.items()) 				# [('a', 0), ('b', 2), ('c', 3)]

```



## Set


```python
# set
mySet = {1,2,3, 1} 		# {1,2,3}

# 연산자
{1 ,2 ,3 } & {2, 3 ,4 }	#{2, 3}
{1 ,2 ,3 } | {2, 3 ,4} 	#{1, 2, 3, 4}
{1 ,2 ,3 } - {2, 3 ,4} 	#{1}
{1 ,2 ,3 } ^ {2, 3 ,4} 	#{1, 4}
```
