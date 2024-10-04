# [프로그래머스] [level3] [일반] 가장 긴 팰린드롬

:link: https://school.programmers.co.kr/learn/courses/30/lessons/12904

```go

func solution(s string) int {
    
    p := 1
    m := make(map[byte][]int)

    for i := 0 ; i < len(s) ; i++ {    
        m[s[i]] = append(m[s[i]], i)
    }
    
    for _,v := range m {
        for i := 0 ; i <len(v)-1; i++ {
            for j := i+1 ; j < len(v) ; j++ {	// 팰린드론이 되려면 양끝 문자가 같아야 함
                start := v[i]
                end := v[j]
                
                if end-start < p {
                    continue
                }
                
                if ispalindrome(s[start:end+1])  { // 양끝 문자가 같은 경우 펠린드론 여부 확인
                    p = end-start+1
                }
            }
        }   
    }

    return p
}


func ispalindrome(s string) bool {

    l := len(s)
    if l %2 == 0 {
        for i :=0; i < l/2 ; i++ {
            if s[l/2-1-i] != s[l/2+i] {
                return false
            }
        }
    } else  {
        for i :=0; i < l/2 ; i++ {
            if s[l/2-1-i] != s[l/2+1+i] {
                return false
            }
        }
    }
    
    return true
}



```

