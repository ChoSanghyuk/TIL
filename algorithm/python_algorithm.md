# 파이썬 알고리즘



## 목차

- 리스트의 부분 집합 탐색
- 정렬 비교





## 리스트의 부분 집합 탐색

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



## 정렬

|      알고리즘      | 평균 수행 시간 | 최악 수행시간  | 알고리즘 기법 |                         비고                          |
| :----------------: | :------------: | :------------: | :-----------: | :---------------------------------------------------: |
|     버블 정렬      |   *O*(*n*2)    |   *O*(*n*2)    |  비교와 교환  |                   코딩이 가장 쉬움                    |
|    카운팅 정렬     |   *O*(*n*+k)   |   *O*(*n*+k)   |  비교환 방식  |               n이 비교적 작을 때만 가능               |
|     선택 정렬      |   *O*(*n*2)    |   *O*(*n*2)    |  비교와 교환  |           교환의 회수가 버블, 삽입보다 적음           |
|      퀵 정렬       | *O*(*n* log n) |   *O*(*n*2)    |   분할 정복   | 최악의 경우 *O*(*n*2)이지만, 평균적으로는 가장 빠르다 |
| 삽입 정렬*O*(*n*2) | *O*(*n* log n) |   *O*(*n*2)    |  비교와 교환  |               n의 개수가 작을 때 효과적               |
|     병합 정렬      | *O*(*n* log n) | *O*(*n* log n) |   분할 정복   |         연결리스트의 경우 가장 효율적인 방식          |

