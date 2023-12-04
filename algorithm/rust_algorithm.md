# Rust 알고리즘 주요 메소드 정리



## 목차



## Array

:bulb:array는 compile 시에 size가 정해져 있어야 함으로, constant value가 아닌 값으로 생성 X.



## Collection

### Vec

- 초기화

```rust
let mut vector: Vec<[i64;2]> = Vec::new(); // 2차원 선언
let vector: Vec<[i64;3]> = vec![[2, -1, 4], [-2, -1, 4], [0, -1, 1], [5, -8, -12], [5, 8, 12]]; // 값으로 선언
let vector:Vec<String> =  Vec::with_capacity(3); // 배열 크기 미리 지정
```

- 메소드

```rust
vector.push(element); // 요소 추가
```



### Deque

- import

```rust
use std::collections::VecDeque;
```

- 초기화

```rust
let mut deque: VecDeque<u32> = VecDeque::new();
```

- 메소드

```rust
deque.push_front(store[i]); // 좌측 push
deque.push_back(inputs[i]); // 우측 push

deque.pop_front().unwrap(); // 좌측 pop (Option 반환)
deque.pop_back().unwrap(); // 좌측 pop (Option 반환)
```





## IO

### Console

- 한 줄 읽어오기

```rust
let mut store= String::new();
io::stdin().read_line(&mut store).unwrap();
```

- buffer에 console 전체 읽어오기

```rust
let buf = io::read_to_string(io::stdin()).unwrap(); // console에 모두 입력 후 EOF(window는 crtl+z) 입력 필요
```

:bulb: buffer를 공백단위로 나누어, Integer / Vec로 변환하기

```rust
let buf = io::read_to_string(io::stdin()).unwrap(); // buffer에 저장
let mut input  = buf.split_ascii_whitespace();	// 공백 단위로 쪼갬. SplitAsciiWhitespace라는 struc 구조로 반환
let n:usize = input.next().unwrap().parse().unwrap(); // 개별 String parsing
let is_queue:Vec<u32> = (0..n).map(|_| input.next().unwrap().parse().unwrap()).collect(); // Vec으로 변환
```





## Debug

- 기본 사용 방법
  - struct / enum에 `#[derive(Debug)]` 적용 시 사용 O
- Primitivate Type & 대부분 Library 타입엔 적용되어 있음

```rust
println!("{:?}", puzzle)	// 기본 Debug
println!("{:#?}", puzzle)	// Pretty Debug
```



## Loop



### for loop

```rust
for i in 0..n{
// ..
}
```





## 문자열 조작

### String

- 초기화

```rust
let mut s = String::new();
let mut s = String::from("String Value");
let mut s = String::with_capacity(3); // 문자열 크기 미리 지정
```

- 메소드

```rust
s.push_str("문자열": &str ); // 문자열 이어 붙이기. &str 타입을 요구함
s.len() // 문자열 길이
s.replace("orgn", "dest") // 일치 문자열 전체 교환
s.chars().rev().collect::<String>() // 문자열 뒤집기
```

- 비교

```rust
s1 == s2 // PartialEq는 String 타입에는 정의되지 않았음.
```

- parse (String => int)

```rust
let n:usize = n.trim().parse().unwrap(); // n은 console에서 받아온 문자열
```

- String => Int Vec

```rust
// is_queue은 console에서 받아온 문자열
let is_queue:Vec<u32> = is_queue.trim().split_whitespace().map(|s| s.parse().unwrap()).collect();
```







## Trait

### TryInto

```rust
pub trait TryInto<T>: Sized {
    type Error;

    fn try_into(self) -> Result<T, Self::Error>;
}
```

- 형변환에 사용

- consumes self

- 예시

  ```rust
  let range_x:usize = (max_x - min_x +1).try_into().unwrap(); // i64 => usize
  ```

  

## 선언

```rust
// mutable 다중선언
let (mut max_x,mut min_x,mut max_y,mut min_y) = (1,2,3,4);
```



## 정수

```rust
i64::MAX // 최대 값
i64::MIN // 최소 값
```

