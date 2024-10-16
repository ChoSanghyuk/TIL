# [프로그래머스] [level3] [일반-heap] 스타 수열

:link: https://school.programmers.co.kr/learn/courses/30/lessons/70130

:memo: 제일 긴 스타 수열이 가지는 요소들을 도출하여, 일반화된 수식을 만드는 것이 중요

:memo: heap을 사용했을 때가 counterMap 단순 iterate하는 것에 비해 더 느린 성능을 가짐. 단순 iterate해도 될 정도의 크기라면 iterage가 효율적으로 보임

```go
import "container/heap"


func solution(a []int) int {

	m := make(map[int]int)
	for _, n := range a {
		m[n] = m[n] + 1
	}

	maxCnt := 0
	counterHeap := make(myHeap, 0)

	for k, v := range m { 

		heap.Push(&counterHeap, &counter{
			value: k,
			count: v,
		})
	}

	for len(counterHeap) > 0 {
		item := heap.Pop(&counterHeap)
		k := item.(*counter).value
		v := item.(*counter).count

		if maxCnt >= 2*v {
			break
		}

		cnt := 0
		isBegin := true
		isCommon := false
		for _, n := range a {
			if isBegin {
				isCommon = k == n
				isBegin = false
			} else if (isCommon && k != n) || (!isCommon && k == n) { // bitwise bool에 사용 불가
				cnt += 2
				isBegin = true
			}
		}
		maxCnt = max(maxCnt, cnt)
	}

	return maxCnt
}

type counter struct {
	value int
	count int
	index int
}

type myHeap []*counter

func (h myHeap) Len() int {
	return len(h)
}

func (h myHeap) Less(i, j int) bool {
	return h[i].count > h[j].count
}

func (h myHeap) Swap(i, j int) {
	h[i], h[j] = h[j], h[i]
	h[i].index = i
	h[j].index = j
}

func (h *myHeap) Pop() interface{} {
	old := *h
	l := len(old)
	item := old[l-1]
	old[l-1] = nil
	item.index = -1
	*h = old[:l-1]
	return item
}

func (h *myHeap) Push(x interface{}) {
	item := x.(*counter)
	item.index = h.Len()
	*h = append(*h, item)
}

func max (a,b int) int {
    if a >= b{
        return a
    }
    return b
}

```

