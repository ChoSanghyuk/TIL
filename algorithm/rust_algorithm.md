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

- 기본 메소드
  - 요소 추가 : `{vector}.push(element);`
  - 길이 : `{vector}.len()`
  - 합산 : `{vector}.iter().sum();`
  - 최대 : `{vector}.iter().max().unwrap();`
- 정렬
  - `{vector}.sort()`
    - 요소 간 값이 같을 경우, 원래 자리 유지
  
  - `{vector}.sort_unstable()`
    - 요소 간 값이 같을 경우, 원래 자리 보장 X
  






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
  - 소수점 반올림
    - `println!("{:.2}", 0.66666);` => `0.67`
  - named arguments
    - `println!("{number:0>width$}", number=1, width=5);` 
  - numbered arguments
    - `println!("{number:0>1$}", number=1, 5);` 
  
  



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



## Map



### HashMap

```rust
use std::collections::HashMap;
```

- 생성 : `let mut h: HashMap<u8, bool> = HashMap::new();`

- 메소드
  - 추가 : `{map}.insert({k}, {v});`
  - 삭제 : `{map}.remove(&{k});`
    - 삭제 후 값 반환 : `{map}.remove(&{k}).unwrap();`







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
  - 문자열 이어 붙이기 : `{s}.push_str("문자열": &str );`
  - 길이 : `{s}.len()`
  - 치환 : `{s}.replace("orgn", "dest")` (일치 문자열 전체 교환)
  - 뒤집기 : `{s}.chars().rev().collect::<String>()` 
  - 반복 : `{s}.repeat({n})`
  - 분리 : `{s}.split("{separator}")`
    - gives an iterator


- 비교

```rust
s1 == s2 // PartialEq는 String 타입에는 정의되지 않았음.
```

- 형변환
    - parse (String => int)

    ```rust
    let n:usize = n.trim().parse().unwrap(); // n은 console에서 받아온 문자열
    ```

    - String => Int Vec

    ```rust
    // is_queue은 console에서 받아온 문자열
    let is_queue:Vec<u32> = is_queue.trim().split_whitespace().map(|s| s.parse().unwrap()).collect();
    ```

    - `Vec<String>` => `String`

    ```rust
    ~.join(":") // 사이사이 : 넣으면서 합치기
    ```



### str

- 개요
  - String Slice
  - *string slice* is a reference to part of a `String`
    - :bulb: `&` 사용 필수
    - `str`은 참조만 가능하기에 소유권을 가져올 수 있는 `str` 타입 자체로 사용 X. `&str` 타입으로 사용
- 사용
  - `&s[starting_index..ending_index]`
      - starting_index 생략 시, 0
      - ending_index 생략 시, `s.len()`


:bulb: 비고

```rust
let s = "07:05:45PM"; // s : &str
let s = "07:05:45PM"[..]; // s : str 타입으로 사용 불가
```



### Regex

```rust
use regex::Regex;
```

- 치환

  ```rust
  let re = Regex::new(r"{regexp}").unwrap();
  let result = re.replace_all("Hello World!", "x");
  ```

- raw string literal : `r#"..."#`

  ```rust
  r#"..."#
  ```

  - 안에서 `"` escape 필요없이 자유롭게 쓸 수 있음

- 캡처

  - regexp안에 `()`로 그룹을 나누고, 각 그룹에 접근할 수 있음

  - `replace`

    ```rust
    let row = ele_re.replace(input, "$1").to_string();
    ```

    - input에서 ele_re의 첫번째 그룹과 매칭되는 영역을 반환
    - `$0`은 정규표현식과 매칭되는 전체 영역을 의미

  - `captures`

    ```rust
    let class_selector = class_re
                          .captures(&row)
                          .map(|cap| cap[1].split_whitespace().collect::<Vec<_>>().join(".") );
    ```

    - `captures`는 `Option<Captures<'h>>`으로 반환하기 때문에 `map`을 사용해서 값이 있을때만 받아옴
    - `[0]`은 정규표현식과 매칭되는 전체 영역을 의미하며, 이후 숫자부터 해당 번째의 영역을 반환



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



### 주요 메소드

- 절대값
  - `{i32}.abs()`
    - `i32`, `i64`, `f32`, `f64` O



### 비교

```rust
use std::cmp;
```

- 큰 값
  - `cmp::max(a, b);`
- 작은 값
  - `cmp::min(a, b);`



## 형변환



### associated function 사용

- 높은 범위로 이동

  ```rust
  i32::from(v) // v: i8
  ```

- 낮은 범위로 이동

  ```rust
  i8::try_from(v).ok() // v:i32
  ```

  

### as

- 개요
  - `as` is most commonly used to turn primitive types into other primitive types
  - has other uses that include turning pointers into addresses, addresses into pointers, and pointers into other pointers
- 예시
  - `let thing1: u8 = 89.0 as u8;`
  - `true as u8`



