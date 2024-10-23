# [프로그래머스] [level 3] [이분탐색] 금과 은 운반하기

:link: https://school.programmers.co.kr/learn/courses/30/lessons/86053

:memo: 브루트 포스 or DP로 구하기에는 **너무 많은 탐색 대상** + 최소값 구하기 => **이분 탐색**

:memo: 문제에서 제시되는 **최악의 경우를 산정하여 max 시간**을 정함

:memo: 금,은의 옮길 수 있는지 여부를 개별적으로도 확인하고 모두 옮길 수 있는지 여부를 **단순히 총합 값으로 구분하여 계산**하는 로직이 추가로 필요함

```go
import "math"

func solution(a int, b int, g []int, s []int, w []int, t []int) int64 {

	// 일단 될만한 맥스 시간 구하기
	var start int64 = 1
	end := 4 * int64(math.Pow(10, 14))

	for start < end {
		mid := (start + end) / 2
		if search(int64(a), int64(b), g, s, w, t, mid) {
			end = mid
		} else {
			start = mid + 1
		}
	}

	return start
}

func search(a int64, b int64, g []int, s []int, w []int, t []int, l int64) bool {

	var goldMoved int64 = 0
	var silverMoved int64 = 0
	var totalMoved int64 = 0

	for i := 0 ; i < len(t) ; i++{

		gi := int64(g[i])
		si := int64(s[i])
		ti := int64(t[i])
		wi := int64(w[i])

		// 시간 내에 실어 옮길 수 있는 횟수
		mc := (l/ti + 1) / 2
		// 시간 내에 옮길 수 있는 양
		mw := mc * wi

		goldMoved += min(gi, mw)
		silverMoved += min(si, mw)
		totalMoved += min(gi+si, mw)

		if goldMoved >= a && silverMoved >= b && totalMoved >= (a+b) {
			return true
		}
	}

	return false
}


func min(a int64, b int64) int64{
    if a < b {
        return a
    }
    return b
}
```

