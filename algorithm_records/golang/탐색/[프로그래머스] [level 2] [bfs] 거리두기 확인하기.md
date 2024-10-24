# [프로그래머스] [level 2] [bfs] 거리두기 확인하기

:link: https://school.programmers.co.kr/learn/courses/30/lessons/81302#fn1

:memo: 테스트 케이스의 크기 확인하고 bfs 진행. visited는 하나의 bfs에서 무한 loop 방지용이며, 해당 bfs 끝난 후에는 해제해 주어야 다음 bfs 정상적 진행 가능

```go
var directions = [][]int{{1, 0}, {-1, 0}, {0, 1}, {0, -1}}

func solution(places [][]string) []int {

	rtn := make([]int, len(places))
	for i, p := range places {
		rtn[i] = isSafe(p)
	}
	return rtn
}

func isSafe(room []string) int {

	visited := make([][]bool, 5)
	for i := range visited {
		visited[i] = make([]bool, 5)
	}

	for i, _ := range room {
		for j, col := range room[i] {
			if col == 'P' {
				if !bfs(i, j, 0, visited, room) {
					return 0
				}
			}

		}
	}

	return 1
}

func bfs(i, j, breadth int, visited [][]bool, room []string) bool {

	if breadth == 3 {
		return true
	}

	visited[i][j] = true

	if room[i][j] == 'X' {
		return true
	} else if breadth != 0 && room[i][j] == 'P' {
		return false
	}

	for _, d := range directions {
		if i+d[0] >= 0 && j+d[1] >= 0 && i+d[0] < 5 && j+d[1] < 5 {
			if !visited[i+d[0]][j+d[1]] {
				if !bfs(i+d[0], j+d[1], breadth+1, visited, room) {
					return false
				}
			}
		}
	}
	visited[i][j] = false
	return true
}
```

