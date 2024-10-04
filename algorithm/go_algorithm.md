

## 형변환

strconv.ParseInt(info.BesuNetwork["chainId"], 10, 64)

strconv.ParseUint(info.BesuNetwork["gasLimit"], 10, 64)



## array

:memo: runtime시 정해지는 변수의 값으로는 array 길이 지정 불가능





## Number



### 큰 수 : big

- 큰 정수
  - 생성
    - `big.NewInt(0)`
    - 



### Slice

- :memo: `for i, s := range mySlice` 할 때, s를 직접적으로 수정해도 `mySlice`에는 반영 X



## String

### `strings`

- 대소문자
  - 대문자 : `strings.ToUpper(str)`
  - 소문자 : `strings.ToLower(str)`
- slice <=> 문자열
  - slice로 쪼개기 : `strings.Split(s, " ")`
  - 문자열로 합치기 : `strings.Join(words, " ")`
