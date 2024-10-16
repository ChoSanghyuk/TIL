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

