

## 형변환

strconv.ParseInt(info.BesuNetwork["chainId"], 10, 64)

strconv.ParseUint(info.BesuNetwork["gasLimit"], 10, 64)



## array

:memo: runtime시 정해지는 변수의 값으로는 array 길이 지정 불가능



## bitwise

- AND : `&`
- OR :`|`
- XOR : `^` (같으면 0, 달라야 1)
- NOT : `~`
- LeftShift : `num << shift`
- RightShift : `num >> shift`



## Number



### 큰 수 : big

- 큰 정수
  - 생성
    - `big.NewInt(0)`
    - 



## Slice



### 생성

```go
sl := make([]string, 0, len(s)) // 타입, 길이, capacity
```



### 합치기

- go 버전 1.22 이후

```go
import "slices"

arr = slices.Concat(arr[:lastZero+1], arr[i-2:i+1], arr[lastZero+1:i-2], arr[i+1:])
```

- go 버전 1.22 이전

```go
temp := make([]byte, 0, len(new)) 
temp = append(temp, new[:lastZero+1]...)
temp = append(temp, new[i-2:i+1]...)
// ...
```





### :memo: memo

-  `for i, s := range mySlice` 할 때, s를 직접적으로 수정해도 `mySlice`에는 반영 X





## Stack

**sample**

```go
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
```







## String

### `strings`

- 대소문자
  - 대문자 : `strings.ToUpper(str)`
  - 소문자 : `strings.ToLower(str)`
- slice <=> 문자열
  - slice로 쪼개기 : `strings.Split(s, " ")`
  - 문자열로 합치기 : `strings.Join(words, " ")`



### `strconv`

- 10진수 => 2진수 형태의 문자열
  - `s := strconv.FormatInt(number, 2)`
  - :memo: `b := []byte(s)` 로 룬 slice 형태로 변환 가능
- 2진수 형태 문자열 => 10진수
  - `strconv.ParseInt(string(b), 2, 64)`
