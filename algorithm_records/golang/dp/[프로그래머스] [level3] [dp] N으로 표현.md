# [프로그래머스] [level3] [dp] N으로 표현

:link: https://school.programmers.co.kr/learn/courses/30/lessons/42895

:bulb: DP 문제에서는 DP를 작은 값을 기준으로 잡는 것이 유리

```go
func solution(N int, number int) int {

	v := make(map[int]bool) // 이전 단계에서 찾은 값은 별도로 저장 X
	v[0] = true
	dp := make([][]int, 9)	// dp는 1~8단계에서 찾을 수 있는 모든 수 기록
	var found bool = false // number 찾았다면 바로 종료

	for i := 1; i <= 8; i++ { // 1~8단계
		now := make([]int, 1)
		nn := 0
        for m := 1; m <= i; m++ { // 각 단계에선 N으로만 이루어진 숫자 생성 가능. ex) 55, 555
			nn = nn*10 + N
		}
		if nn == number {
			return i
		}
		now[0] = nn
		v[nn] = true

		for j := 1; j < i; j++ { // 이전 단계와의 조합을 통해서 숫자 생성 가능
			for _, a := range dp[i-j] {
				for _, b := range dp[j] {
					now, found = operationSave(a, b, now, v, number)
					if found {	// 찾았다면 종료
						return i
					}
				}
			}
		}
		dp[i] = now
	}

	return -1	// 8단계까지도 못 찾았다면 -1
}

// 입력된 두 값에 대해서 사칙연산 수행 후 결과 저장
func operationSave(a, b int, sl []int, v map[int]bool, t int) ([]int, bool) {

	var r int

	for _, o := range []int{1, 2, 3, 4} {
		switch o {
		case 1:
			r = a + b
		case 2:
			r = a * b
		case 3:
			if b-a >= 0 {
				r = b - a
			} else {
				r = a - b
			}
		case 4:
			if b-a >= 0 {
				r = b / a
			} else {
				r = a / b
			}
		}

		if r == t {
			return nil, true
		}

		if !v[r] {
			sl = append(sl, r)
			v[r] = true
		}
	}
	return sl, false
}
```



