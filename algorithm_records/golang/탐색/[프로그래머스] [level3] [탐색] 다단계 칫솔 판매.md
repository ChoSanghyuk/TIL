# [프로그래머스] [level3] [탐색] 다단계 칫솔 판매

:link: https://school.programmers.co.kr/learn/courses/30/lessons/77486

:memo: 문제 설명에서 진행되는 프로세스를 정확하게 이해하는 것이 중요. 판매를 다 한 다음 정산을 하는 것이 아니라, 판매 때마다 정산이 발생

```go
func solution(enroll []string, referral []string, seller []string, amount []int) []int {

	nodes := make(map[string]int)
	tree := make(map[int]int)

	rtn := make([]int, len(enroll))

	// enroll의 노드 번호 지정
	// root 노드 판별 및 자식 노드 식별
	for i := range enroll {
		nodes[enroll[i]] = i
		if referral[i] == "-" {
			tree[i] = -1
		} else {
			tree[i] = nodes[referral[i]] // 자식이 부모 노드 기억
		}
	}

	// 초기 수익 저장
	for i, s := range seller {
		bottomUp(nodes[s], 100*amount[i], rtn, tree)
	}
	// root 노드에서 시작해서 리프 노드로 이동. 리프 노드는 자신의 부모의 수익 갱신

	return rtn
}

func bottomUp(now int, gain int, profit []int, tree map[int]int) {

	off := gain / 10
	profit[now] += (gain - off)

	p := tree[now]
	if off != 0 && p != -1 {
		bottomUp(p, off, profit, tree)
	}
}
```

