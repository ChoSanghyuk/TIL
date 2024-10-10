# [프로그래머스] [level2] [stack] 괄호 회전하기

:link: https://school.programmers.co.kr/learn/courses/30/lessons/76502

:memo: 마지막에 stack길이가 0인지 확인

```go
package main

func solution(s string) int {
	var cnt int
	for i := 0; i < len(s); i++ {
		new := s[i:] + s[:i]
		if isCorrect(new) {
			cnt++
		}
	}
	return cnt
}

// (), [], {}
func isCorrect(s string) bool {
	st := make(stack, 0)
	for _, a := range s {
		if a == '(' || a == '[' || a == '{' {
			st.push(a)
		} else {
			t := st.pop()
			switch t {
			case '(':
				if a != ')' {
					return false
				}
			case '[':
				if a != ']' {
					return false
				}
			case '{':
				if a != '}' {
					return false
				}
			default:
				return false
			}
		}
	}
	return len(st) == 0
}

type stack []rune

func (s *stack) push(a rune) { // pointer가 아닌 경우, 기존 s에 반영되지 않음
	*s = append(*s, a)
}

func (s *stack) pop() rune {
	l := len(*s)
	if l == 0 {
		return 0
	}
	rtn := (*s)[l-1]
	*s = (*s)[:l-1]
	return rtn
}

```

