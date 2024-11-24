# Rust 알고리즘 주요 메소드 정리



[TOC]







## Array

:bulb:array는 compile 시에 size가 정해져 있어야 함으로, constant value가 아닌 값으로 생성 X.




## Compare

### PartialEq

- 동일성 확인을 실제로 수행하는 trait
- 구조

```rust
impl PartialEq for Puzzle {
    fn eq(&self, other: &Self) -> bool {
        (self.num_pieces == other.num_pieces) && (self.name == other.name)
    }
}
```

- 활용

```rust
is_queue[i].eq(&0) // is_queue는 Vec<u32> 타입의 변수
```

:bulb: `eq` 메소드는 `immutable reference` 타입의 self와 파라미터를 받음



## Collection

### Vec

- 초기화

```rust
let mut vector: Vec<[i64;2]> = Vec::new(); // 2차원 선언
let vector: Vec<[i64;3]> = vec![[2, -1, 4], [-2, -1, 4], [0, -1, 1], [5, -8, -12], [5, 8, 12]]; // 값으로 선언
let vector:Vec<String> =  Vec::with_capacity(3); // 배열 크기 미리 지정
```

- 메소드
  - 요소 추가 : `{vector}.push(element);`
  - 합산 : `{vector}.iter().sum();`
  - 길이 : `{vector}.len()`






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



## Debug

### Formatted Print

- 종류

  - `format!` : write formatted text to [`String`](https://doc.rust-lang.org/rust-by-example/std/str.html)

  - `print!` :  same as `format!` but the text is printed to the console (io::stdout).

  - `println!` : same as `print!` but a newline is appended.

  - `eprint!` : same as `print!` but the text is printed to the standard error (io::stderr).

  - `eprintln!` : same as `eprint!` but a newline is appended.

- 사용

  - `{}` will be automatically replaced with any arguments. These will be stringified.
  - Positional arguments can be used. 
    - Specifying an integer inside `{}` determines which additional argument will be replaced
    - Arguments start at 0 immediately after the format string.

  - specifying the format character. (10진수로 표현된 값을 해당 진법으로 변환해줌)
    - `{:b}` : binary
    - `{:o}` : octal
    - `{:x}` : hex
  - padding
    - `println!("{number:>5}", number=1);` => `"    1"`
      - right-justify text with a specified width (총 5칸을 만드는 것으로, 4칸 공백 추가됨)
    - `println!("{number:0>5}", number=1);` => `00001`
    - `println!("{number:0<5}", number=1);` => `10000`
  - named arguments
    - `println!("{number:0>width$}", number=1, width=5);` 



### Debugging

- 기본 사용 방법
  - struct / enum에 `#[derive(Debug)]` 적용 시 사용 O
- Primitivate Type & 대부분 Library 타입엔 적용되어 있음

```rust
println!("{:?}", puzzle)	// 기본 Debug
println!("{:#?}", puzzle)	// Pretty Debug
```





## IO

### Console 읽기

- 한 줄 읽어오기

```rust
let mut store= String::new();
// 1안
io::stdin().read_line(&mut store).unwrap();
// 2안
io::stdin().read_line(&mut store).ok().expect("read error");
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





### Buffer Write

```rust
use std::io::Write;
```

- `writeln!`
  - Writes formatted data into a buffer, with a newline appended
  - ex) `writeln!(&mut fptr, "{}", result).ok();`







## Loop



### for loop

```rust
for i in 0..n{
// ..
}
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



## 선언

```rust
// mutable 다중선언
let (mut max_x,mut min_x,mut max_y,mut min_y) = (1,2,3,4);
```



## 숫자

### 최대/최소

- 범위

  | 타입  | 최소                           | 최대 |
  | ----- | ------------------------------ | ----|
  | `i32` | -2_147_483_648  | 2_147_483_647 |
  | `i64` | -9_223_372_036_854_775_808    | 9_223_372_036_854_775_807 |

- 최대값/최소값

  ```rust
  i64::MAX // 최대 값
  i64::MIN // 최소 값
  ```



### 형변환

- 높은 범위로 이동

  ```rust
  i32::from(v) // v: i8
  ```

- 낮은 범위로 이동

  ```rust
  i8::try_from(v).ok() // v:i32
  ```

  





