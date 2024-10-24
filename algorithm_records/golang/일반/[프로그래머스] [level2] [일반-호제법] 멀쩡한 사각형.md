# [프로그래머스] [level2] [일반-호제법] 멀쩡한 사각형

:link: https://school.programmers.co.kr/learn/courses/30/lessons/62048?language=go

:memo: 호제법 :  2개의 자연수(또는 정식) a, b에 대해서 a를 b로 나눈 [나머지](https://ko.wikipedia.org/wiki/나머지)를 r이라 하면(단, a>b), a와 b의 최대공약수는 b와 r의 최대공약수와 같다

```go
func solution(w int, h int) int64 {

	c := gcd(w, h)
	w1, h1 := w/c, h/c
	ip := (w1 + h1 - 1) * c

	return int64(w*h - ip)
}

func gcd(a, b int) int {
	if b == 0 {
		return a
	}
	return gcd(b, a%b) // a < b인 경우, 여기서  b,a로 조정
}
```

