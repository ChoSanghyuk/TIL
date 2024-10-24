# [프로그래머스] [level 2] [일반] n^2 배열 자르기

:link: https://school.programmers.co.kr/learn/courses/30/lessons/87390

:memo: 인덱스와 n번째 사이의 보정은 마지막 값에 1 더하는 것으로 수행. 사전에 보정한 데이터로 수식을 만들려면 여러 예외 케이스들이 생김

:memo: 어차피 몫이랑 나머지들 다 조정하면, 마지막에 같이 하는걸로 수행

```go
func solution(n int, left int64, right int64) []int {
    
    rtn := make([]int, right-left+1)
    n2 := int64(n)
    
    for i := range rtn {
        rtn[i] = getNum(n2, left)
        left++
    }
    return rtn
}


func getNum(n int64, t int64) int {

    r := t / n
    c := t % n
    
    return max(int(r), int(c))+1
}

func max( a, b int) int {
    if a < b {
        return b
    }
    return a
}
```



### 실패 코드

```go
func solution(n int, left int64, right int64) []int {
    
    rtn := make([]int, right-left+1)
    n2 := int64(n)
    
    for i := range rtn {
        left++ // 보정
        rtn[i] = getNum(n2, left)
    }
    return rtn
}


func getNum(n int64, t int64) int {
    if n == 1 {
        return 1
    }
    
    r := t / n
    c := t % n
    
    if c == 0 {
        c = n
    }
    return max(int(r+1), int(c)) // 보정
}

func max( a, b int) int {
    if a < b {
        return b
    }
    return a
}
```

- :bulb: n이 2이고, t가 짝수 (left값이 홀수)일 때, 예외 케이스가 생겨버림