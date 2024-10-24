# [프로그래머스] [level2] [다익스트라] 배달

:link: https://school.programmers.co.kr/learn/courses/30/lessons/12978

:memo: 그래프 문제에선 노드 사이 간선의 양방향성 고려

:memo: 다익스트라에서 힙을 사용할 때에는 최소값이 갱신되는 노드는 다시 힙에 넣어줘야 함

```go
import (
	"container/heap"
)

const Max = 500001

func solution(N int, road [][]int, k int) int {

	m := make([][]int, N+1)
	for i := range m {
		m[i] = make([]int, N+1)
	}
	for i := 1 ; i <= N ; i ++{
        for j := 1 ; j <= N ; j++{
			m[i][j] = Max
		}
	}

	for _, r := range road {
		m[r[0]][r[1]] = min(m[r[0]][r[1]], r[2])
        m[r[1]][r[0]] = min(m[r[1]][r[0]], r[2]) //양방향
	}

	d := make([]int, N+1)
	for i := 1 ; i <= N ; i ++{
		d[i] = Max
	}
    m[1][1] = 0
    
	pq := make(PriorityQueue, 0)

	for i := 1; i <= N; i++ { // 시작 노드와 간선이 있는 노드들을 초기 세팅
        if m[1][i] != 500001 {
            heap.Push(&pq, &item{
			priority: m[1][i],
			n:        i,
		})
        }
	}

	for len(pq) > 0 {
		e := heap.Pop(&pq).(*item)
		d[e.n] = min(d[e.n], m[1][e.n])
        for i := 1; i <= N; i++ {	// 현재 노드와 연결되어 있는 모든 노드들에 대해서 최소값 업데이트 진행
            if d[i] > d[e.n]+m[e.n][i] {	
				d[i] = d[e.n]+m[e.n][i]
                heap.Push(&pq, &item{	// 최소값이 갱신되었다면 힙에 추가 (자기 주변 최소값 세팅을 책임져야함)
                    priority: d[i],
                    n: i,
                })
            }
		}
	}

	var cnt int = 0
	for i := 1; i <= N; i++ {
		if d[i] <= k {
			cnt++
		}
	}
	return cnt
}

func min (a,b int) int{
    if a <= b {
        return a
    }
    return b
}

type item struct {
	priority int // The priority of the item in the queue.
	index    int // The index of the item in the heap.
	n        int
}

type PriorityQueue []*item

func (pq PriorityQueue) Len() int {
	return len(pq)
}

func (pq PriorityQueue) Less(a, b int) bool {
	return pq[a].priority < pq[b].priority
}

func (pq *PriorityQueue) Pop() interface{} {
	old := *pq
	l := len(old)
	item := old[l-1] // [0]이 아니라 맨 뒤에껄 꺼내네..
	old[l-1] = nil
	item.index -= 1
	*pq = old[:l-1]
	return item
}

func (pq *PriorityQueue) Push(e interface{}) {
	item := e.(*item)
	item.index = len(*pq)
	*pq = append(*pq, item)
}

func (pq PriorityQueue) Swap(a, b int) {
	pq[a], pq[b] = pq[b], pq[a]
	pq[a].index = a
	pq[b].index = b

}

```

