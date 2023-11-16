# swea 5174 subtree 문제 풀이(python)



## 문제 개요

주어진 이진 트리에서 노드 N을 루트로 하는 서브 트리에 속한 노드의 개수를 찾아라

https://swexpertacademy.com/main/learn/course/lectureProblemViewer.do



## 문제 풀이

풀이 순서

1. 간선 정보를 받아온다
2. N을 담은 큐를 생성한다.
3. 큐의 현재 위치에서 이동할 수 있는 노드들을 큐에 추가하고 위치를 다음 칸으로 옮긴다.
4. 3번을 반복하다 현재 위치가 큐의 마지막 위치이고, 해당 위치에서 더 추가할 노드가 없다면 반복 중지
5. 큐의 길이가 서브트리의 길이가 된다.

```python
import sys

sys.stdin = open("input.txt")

T = int(input())

for t in range(1, T+1):
    E, N = map(int, input().split())
    # 1. 간선 정보를 받아옴
    roads = [[] for _ in range(E+2)]
    inputs = tuple(map(int, input().split()))
    for i in range(0, E*2, 2):
        roads[inputs[i]].append(inputs[i+1])

    # 2.N을 담은 큐를 생성한다.
    queue = [N]
    i = 0
    # 4. 3번을 반복하다 현재 위치가 큐의 마지막 위치이고, 해당 위치에서 더 추가할 노드가 없다면 반복 중
    while not (i == len(queue)-1 and len(roads[queue[i]]) == 0) :
        # 3. 큐의 현재 위치에서 이동할 수 있는 노드들을 큐에 추가하고 위치를 다음 칸으로 옮긴다
        queue += roads[queue[i]]
        i += 1
	# 5. 큐의 길이가 서브트리의 길이가 된다.
    print(f"#{t} {len(queue)}")

```

