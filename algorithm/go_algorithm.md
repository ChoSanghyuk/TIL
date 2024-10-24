# Go algorithm







## 입출력



### 입력

- `bufio.Scanner`

  - Best for: Reading input line by line or token by token

  ```go
  scanner := bufio.NewScanner(os.Stdin)
  for scanner.Scan() {
      line := scanner.Text() // read one line at a time
      fmt.Println(line)
  }
  
  if err := scanner.Err(); err != nil {
      fmt.Fprintln(os.Stderr, "Error reading input:", err)
  }
  ```

- `bufio.Reader`

  - Best for: Reading input in larger chunks or when you need more control over how input is processed

  ```go
  reader := bufio.NewReader(os.Stdin)
  for {
      line, err := reader.ReadString('\n')
      if err != nil {
          if err.Error() == "EOF" {
              break
          }
          fmt.Fprintln(os.Stderr, "Error reading input:", err)
      }
      fmt.Print(line)
  }
  ```



### 출력

- `bufio.Writer`

  - Best for: Writing large amounts of data or when minimizing system calls is crucial

  ```go
  writer := bufio.NewWriter(os.Stdout)
  defer writer.Flush() // ensure all data is written at the end
  
  for i := 0; i < 1000; i++ {
      fmt.Fprintf(writer, "Line %d\n", i)
  }
  ```

  



## 형변환



### 문자열 => 숫자

- string => int
  - `strconv.ParseInt("문자열", 10, 64)`
  - `strconv.Atoi("-42")`
- string => uint
  - `strconv.ParseUint("문자열", 10, 64)`



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





## 자료구조



### Heap

```go
mport (
    "container/heap"
)

type Item struct {
    value    string // The value of the item; arbitrary data
    priority int    // The priority of the item in the queue
    index    int    // The index of the item in the heap (required by `container/heap`)
}

type PriorityQueue []*Item

// Len returns the number of items in the queue.
func (pq PriorityQueue) Len() int { return len(pq) }

// Less compares the priority of two items (higher priority comes first).
func (pq PriorityQueue) Less(i, j int) bool {
    return pq[i].priority > pq[j].priority // For max-heap (higher priority first)
    // return pq[i].priority < pq[j].priority // For min-heap (lower priority first)
}

// Swap swaps the position of two items in the queue.
func (pq PriorityQueue) Swap(i, j int) {
    pq[i], pq[j] = pq[j], pq[i]
    pq[i].index = i
    pq[j].index = j
}

// Push adds a new item to the queue.
func (pq *PriorityQueue) Push(x interface{}) {
    n := len(*pq)
    item := x.(*Item)
    item.index = n
    *pq = append(*pq, item)
}

// Pop removes and returns the item with the highest priority.
func (pq *PriorityQueue) Pop() interface{} {
    old := *pq
    n := len(old)
    item := old[n-1]
	  old[n-1] = nil
    item.index = -1 // For safety
    *pq = old[0 : n-1]
    return item
}

func main() {
	  pq := make(PriorityQueue, 0)
    heap.Init(&pq)
  
    heap.Push(&pq, &Item{
          value:    "task 1",
          priority: 3,
      })

    item := heap.Pop(&pq).(*Item)
}
```



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



### 정렬

```go
import "sort"

sort.Slice(mySlice, func(i, j int) bool {
    return rtn[i] < rtn[j]
})
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
