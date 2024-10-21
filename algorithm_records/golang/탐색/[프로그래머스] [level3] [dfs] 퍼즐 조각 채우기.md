# [프로그래머스] [level3] [dfs] 퍼즐 조각 채우기

:link: https://school.programmers.co.kr/learn/courses/30/lessons/84021

:memo: 2차원 배열끼리 비교를 하기 위해, 각자 다른 위치에 있는 퍼즐을 (0,0)에서 시작하는 형태로 모두 바꿔주는 것이 필요함

:memo: 2차원 배열 회전 : **1.** 원본의 가로, 세로 길이 => 대상의 세로, 가로 길이 **2.** `new[j][i] = base[len(base)-(i+1)][j]`

```go
var directions = [][]int{{0, 1}, {0, -1}, {1, 0}, {-1, 0}}

func solution(game_board [][]int, table [][]int) int {

	cnt := 0

	emptys := makePuzzles(game_board, true)
	candidates := makePuzzles(table, false)

	for _, b := range emptys {
		for _, t := range candidates {
			if !t.matched && b.cnt == t.cnt && b.isMatched(t) {
				cnt += b.cnt
				t.matched = true
				break
			}
		}
	}
	return cnt
}

type puzzle struct {
	cnt     int
	shape   [][]bool
	matched bool
}

func makePuzzles(board [][]int, isBase bool) []*puzzle {

	var puzzleNumber int

	if isBase {
		puzzleNumber = 0
	} else {
		puzzleNumber = 1
	}
	rtn := make([]*puzzle, 0)

	visited := make([][]bool, len(board))
	for i := range visited {
		visited[i] = make([]bool, len(board[i]))
	}

	for i := range board {
		for j := range board[i] {
			if !visited[i][j] && board[i][j] == puzzleNumber {
				visited[i][j] = true
				seq := make([][]int, 0)
				dfs(i, j, puzzleNumber, board, visited, &seq)
				rtn = append(rtn, convert(seq))
			}
		}
	}

	return rtn
}

func dfs(i, j, t int, board [][]int, visited [][]bool, seq *[][]int) {

	if board[i][j] != t {
		return
	}
	*seq = append(*seq, []int{i, j})

	for _, d := range directions {
		if i+d[0] >= 0 && j+d[1] >= 0 && i+d[0] < len(board) && j+d[1] < len(board[0]) {
			if !visited[i+d[0]][j+d[1]] {
				visited[i+d[0]][j+d[1]] = true
				dfs(i+d[0], j+d[1], t, board, visited, seq)
			}
		}
	}
}

func convert(seq [][]int) *puzzle {

	p := puzzle{
		cnt: len(seq),
	}

	var maxI, maxJ, minI, minJ = 0, 0, 50, 50
	for _, cell := range seq {
		if maxI < cell[0] {
			maxI = cell[0]
		}
		if minI > cell[0] {
			minI = cell[0]
		}
		if maxJ < cell[1] {
			maxJ = cell[1]
		}
		if minJ > cell[1] {
			minJ = cell[1]
		}
	}
	shape := make([][]bool, maxI-minI+1)
	for i := range shape {
		shape[i] = make([]bool, maxJ-minJ+1)
	}

	for _, cell := range seq {
		i, j := cell[0]-minI, cell[1]-minJ
		shape[i][j] = true
	}

	p.shape = shape

	return &p
}

func (p *puzzle) isMatched(t *puzzle) bool {

	if !(len(p.shape) == len(t.shape) && len(p.shape[0]) == len(t.shape[0])) && !(len(p.shape) == len(t.shape[0]) && len(p.shape[0]) == len(t.shape)) {
		return false
	}

	for i := 0; i < 4; i++ {
		if !(len(p.shape) == len(t.shape) && len(p.shape[0]) == len(t.shape[0])) {
			goto outer
		}

		for i := range p.shape {
			for j := range p.shape[i] {
				if p.shape[i][j] != t.shape[i][j] {
					goto outer
				}
			}
		}
		return true
	outer:
		if i < 3 {
			t.rotate()
		}
	}

	return false
}

func (p *puzzle) rotate() {

	new := make([][]bool, len(p.shape[0]))
	for i := range new {
		new[i] = make([]bool, len(p.shape))
	}

	for i := range p.shape {
		for j := range p.shape[i] {
			new[j][i] = p.shape[len(p.shape)-(i+1)][j]
		}
	}
	p.shape = new
}

```

