# [프로그래머스] [level2] [bfs] 빛의 경로 사이클

:link: https://school.programmers.co.kr/learn/courses/30/lessons/86052

:memo: 마지막에 결과 정렬하라고 되어있음

```go
import "sort"

var dir = [][]int{{0, 1}, {0, -1}, {1, 0}, {-1, 0}}

func solution(grid []string) []int {

	rtn := make([]int, 0)
	visited := make([][][]bool, len(grid))
	for i := range visited {
		visited[i] = make([][]bool, len(grid[0]))
		for j := range visited[i] {
			visited[i][j] = make([]bool, 4)
		}
	}

	for i := range visited {
		for j := range visited[i] {
			for d := range visited[i][j] {
				if !visited[i][j][d] {
					rtn = append(rtn, bfs(i, j, d, grid, visited))
				}
			}
		}
	}

	sort.Slice(rtn, func(i, j int) bool {
		return rtn[i] < rtn[j]
	})

	return rtn
}

func bfs(i, j, d int, grid []string, visited [][][]bool) int {

	if visited[i][j][d] {
		return 0
	}

	visited[i][j][d] = true

	var nd int
	switch grid[i][j] {
	case 'S':
		nd = d
	case 'R':
		switch d {
		case 0:
			nd = 2
		case 1:
			nd = 3
		case 2:
			nd = 1
		case 3:
			nd = 0
		}
	case 'L':
		switch d {
		case 0:
			nd = 3
		case 1:
			nd = 2
		case 2:
			nd = 0
		case 3:
			nd = 1
		}
	}
	ni, nj := i+dir[nd][0], j+dir[nd][1]

	if ni == -1 {
		ni = len(grid) - 1
	} else if ni == len(grid) {
		ni = 0
	}

	if nj == -1 {
		nj = len(grid[0]) - 1
	} else if nj == len(grid[0]) {
		nj = 0
	}

	return bfs(ni, nj, nd, grid, visited) + 1
}

```

