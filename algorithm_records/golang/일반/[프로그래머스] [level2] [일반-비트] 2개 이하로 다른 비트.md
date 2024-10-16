# [프로그래머스] [level2] [일반-비트] 2개 이하로 다른 비트

:link: https://school.programmers.co.kr/learn/courses/30/lessons/77885

:memo: 일반화된 규칙을 찾아. (앞에 0을 붙이고, 뒤에서부터 탐색하며 제일 먼저 나온 0을 1로 바꾸고, 그 바로 뒤를 1로 바꿈)

```go
import 	"strconv"



func solution(numbers []int64) []int64 {

	rtn := make([]int64, len(numbers))

	for i, n := range numbers {
		rtn[i] = f(n)
	}

	return rtn
}

func f(n int64) int64 {

	if n%2 == 0 {
		return n + 1
	} else {
		s := strconv.FormatInt(n, 2)
		b := []byte(s)
		b = append([]byte{'0'}, b...)

		for i := len(b) - 1; i >= 0; i-- {
			if b[i] == '0' {
				b[i] = '1' // 홀수일때는 끝자리가 0으로 시작하지 않음 길이 초과 걱정 X
				b[i+1] = '0'
				rtn, _ := strconv.ParseInt(string(b), 2, 64)
				return rtn
			}
		}
	}
	return -1
}

```

