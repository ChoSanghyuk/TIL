# [프로그래머스] [level3] [트리] 모두 0으로 만들기

:link: https://school.programmers.co.kr/learn/courses/30/lessons/76503

:memo: **트리 구조**라는 것에 착안하여 문제를 접근하는 것이 가장 중요! 

- 트리 구조 : 노드가 항상 하나의 부모만을 가지고 자식은 여러 개 가질 수 있음

:memo: 간선 정보를 기록할 때, `[][]bool` 형식보다 `map[int][]int` 타입 iterate할 때 유리함



```go
package main

func solution(a []int, edges [][]int) int64 {

	nodes := make(map[int][]int)

	sum := 0
	for _, v := range a {
		sum += v
	}
	if sum != 0 {
		return -1
	}

	for _, line := range edges {
		i, j := line[0], line[1]
		nodes[i] = append(nodes[i], j)
		nodes[j] = append(nodes[j], i)
	}

	v := make([]bool, len(a))

	rtn := int64(dfs(0, a, nodes, v))
	if a[0] != 0 {
		return -1
	}
	return rtn
}

func dfs(i int, a []int, nodes map[int][]int, visited []bool) int {

	c := 0
	visited[i] = true
	var parent int = -1

	for _, j := range nodes[i] {
		if visited[j] { // 자신의 부모노드
			parent = j
		} else {
			c += dfs(j, a, nodes, visited)
		}
	}

	if parent != -1 {
		a[parent] += a[i]
	}
	return c + abs(a[i])
}

func abs(a int) int {
	if a < 0 {
		return -1 * a
	}
	return a
}
```

