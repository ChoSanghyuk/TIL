# [프로그래머스] [level3] [stack] 110 옮기기

:link: https://school.programmers.co.kr/learn/courses/30/lessons/77886

:memo: 110을 계속 없을 때, 다시 생성되는 110을 없애는 구조에서 stack을 활용. 이후 일반화된 규칙 (제일 마지막 0 뒤에 110들 쭉 이어서 붙이기) 찾기

```go
import "strings"

func solution(s []string) []string {
	rtn := make([]string, len(s))
	for i, str := range s {
		rtn[i] = dic(str)
	}
	return rtn
}

func dic(s string) string {

	stk := make(Stack, 0, len(s))
	idx := 0
	cnt := 0

	for _, c := range s {
		if c == '0' {
			if stk.isCondition() {
				stk.Pop()
				stk.Pop()
				cnt++
			} else {
				stk.Push(c)
				idx = len(stk)
			}
		} else {
			stk.Push(c)
		}
	}
	return string(stk[:idx]) + strings.Repeat("110", cnt) + string(stk[idx:])
}

type Stack []rune

func (s Stack) isEmpty() bool {
	return len(s) == 0
}

func (s *Stack) Push(x rune) {
	*s = append(*s, x)
}

func (s *Stack) Pop() rune {
	old := *s
	item := old[len(old)-1]
	// old[len(old)-1] = 0
	*s = old[:len(old)-1]
	return item
}

func (s Stack) isCondition() bool {
	l := len(s)
	return l > 1 && s[l-1] == '1' && s[l-2] == '1'
}
```

