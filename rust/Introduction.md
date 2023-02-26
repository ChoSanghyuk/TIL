# Introduction



## 소개



### 분류

- systems programming language

:bulb: 특징 : able to reach low-level stuff, getting close to the real hardware world



### 강점

- safety

- concurrency

- speed



## Fundamentals



### cargo

- 역할
  - pacakge manager
  - build system
  - test runner
  - docs generator
- 명렁어
  - `cargo new hello`
    - `hello` 라는 package 생성
    - 생성 파일
      - `Cargo.toml` : config file
      - `main.rs` : rust source file
  - `cargo run`
    - 프로젝트 실행



### Cargo.toml

- name : 프로젝트 이름
- version
  - sementic versioning(.으로 구분된 3자리 수)로 버전 표기
- authors
  - 주로 연결된 git으로 자동 생성
- edition



### Cargo Run

- 진행 단계
  - Compiling
    - 코드 변경 X 재실행 => 생략
  - Finished
  - Running
- Debug 모드
  - default로 실행
-  Release 모드
  - Debug보다 빠른 Run 타임 but, 느린 Compile
  - `cargo run --release`



### Variables

- 특징

  - Strongly Typed Language

  - Default Immutable
    - Safety
      - Concurrency
      - Speed

- 기본 선언

    ```rust
    let var = 2;
    ```

    - let : declare

- Type 지정

    ```rust
    let var: i32 = 4;
    ```

- 다중 변수 선언

    ```rust
    let (var1, var2) = (8,50);
    ```

- Mutable 선언

    ```rust
    let mut var = 32;
    var - 2;
    ```

- Const 선언

    ```rust
    const CONST_VAR: f64 = 9.9;
    ```

    - 선언 방법
      - `const`로 선언
      - 변수명 : Screaming snake case로 선언
      - 변수 타입 필수
      - 값은 constant expression
        - compile 시, 알 수 있는 값
    - 사용 이점
      - Function Outside에서 선언되었어도, 어디서든 사용 O
      - compile 시 선언 => REALLY FAST




### Scope

- No Garge Collector => Block 종료 시, Block 내 선언된 모든 변수 삭제

- Shadowed

  1. 외부 블록 변수에 대한 Shadow

     - 외부 블록의 변수와 같은 이름으로 내부에서 선언 시, 블록 내 변수 사용

     - Variable is always local to the scope

  2. 같은 블록 내 변수에 대한 Shadow

     - 같은 블록 내에서도 선언된 변수에 대해서 재선언 가능

     ```rust
     let mut x = 5;
     let x = x; // now immutable
     ```

     - 다른 타입으로 재선언

     ```rust
     let var = "Sample String";
     let var = make_different(var);
     ```

     :bulb: Data Transform 후 이전 변수 제거 목적으로 자주 사용



### Memory Safety

- Complie 시, 확인

- 모든 변수들은 사용전 Initialize 되어야 함



### Functions

```rust
fn do_stuff(param1: f64, param2: f64) -> f64{
    return param1 * param2; // param1 * param2
}
```

- 선언
  - `fn` 키워드
- naming convention
  - snake_case
- 인자 값
  - `파라미터: 타입` 
  - 다중 인자 시, `,`로 구분
- Return 타입
  - `-> 타입`
- `return` 및 `;` 생략 가능
  - `{return true;}` = `{true}` 

:bulb: 호출 시 인자명으로 지정 X => 선언된 정확한 순서대로 인자 입력



### Module System

- 자체 Library 추가

  - project root/src/`lib.rs` 생성

  - Library 내 모든 항목은 Default 값으로 Private

    - `pub` : public 

    ```rust
    pub fn greet(){
    	println!("Hi!");
    }
    ```

- 모듈 사용

  - Absolute Path로 Library의 함수 사용 O
    - Absolute Path = Library Name = Name of project In Cargo.toml

      ```rust
      fn main(){
          hello::greet();
      }
      ```

  - `use` 키워드로 모듈 Import 시, Absolute Path 생략 O

      ```rust
      use hello::great;
      
      fn main(){
          greet();
      }
      ```

  - Rust Standard Library : `std`

    - Rust 자체 제공 기본 Library
    - ex) `use std::collections::HashMap;`
    - 해당 Library 관련 정보는 구글에 `rust std 찾는 이름` 검색
  
  - Rust Package Registry : `crates.io`
  
    - standard에서 미제공하는 Library 등록된 사이트
  
    - `Cargo.toml`에 dependency 추가
  
      - `패키지 이름 = "버전"`
  
      - ex) `rand = "0.6.5"`



## Primitive Types & Control Flow



### Scalar Types

- Integer Types

  | Unsigned(부호 없는) | Signed (부호있는) |
  | :-----------------: | :---------------: |
  |         u8          |        i8         |
  |         u16         |        i16        |
  |         u32         |   i32 (default)   |
  |         u64         |        i64        |
  |        u128         |       i128        |
  |        usize        |       isize       |

  - u/i size

    - 메모리 크기와 연관될 때 사용
      - 객체의 크기, 벡터의 인덱싱 등
    - 32bit 환경 => 4byte / 64bit 환경 => 8byte

  - 진법

    |    Decimal    | 1000000 |
    | :-----------: | :-----: |
    |      Hex      |  0x~~   |
    |     Octal     |  0o~~   |
    |    Binary     |  0b~~   |
    | Byte(u8 only) |  b'A'   |

  - 특징

    - 중간 _ 넣어도 무시됨

- Float

  - f32
  - f64 (default)
    - 64bit 미만 환경에서는 많이 느려짐

- Floating Point Literals

  - statndard :IEEE-754
  - suffix는 필요 없지만, `.` 앞에 숫자 필요

:bulb: Numerical Numbers는 suffix로 타입 명시 O. 

- 다음 두 식 중 선택 O
  - `let x: u16 = 5;`  
  - `let x = 5u16;` or `let x = 5_u16;`

- Boolean

  - type 명시 : `bool`
  - 값 : `true`, `false`

- Character

  - type 명시 이름 x

    - `' '`으로 initiate

  - 크기

    - 4 bytes (32 bits)

      => make array of characters `USC-4` or `UTF-32` string

      => String은 거의 `utf-8` 이기 때문에, character 타입 쓸 일 거의 X



### Compound Types

- 여러 타입의 값들을 하나의 타입으로 모임
- Tuple
  - stores multiple values of any type
  - 선언
    - `let info = (1, 3.3, 999);`
    - `let info: (u8, f64, i32) = (1, 3.3, 999);`
  - 값 접근
    - dot syntax
      - `let var1 = info.0;`
    - destructure
      - `let (var1, var2, var3) = info;`
  - 최대 크기
    - `12` 를 넘어가면, 사용은 가능하지만 제한된 기능 사용

- array

  - stores multiple values of same type
  - 선언
    - `let arr = [1,2,3];`
    - `let arr = [0;3];` 
      - 값;개수
  - 접근
    - `let var1 = arr[0];`

  - 최대 크기
    - `32` 를 넘어가면, 사용은 가능하지만 제한된 기능 사용

- Vec



### Control Flow

- 조건문

  - if문 사용시 `( )` 필요 X. `if`와 `{}` 사이가 조건식
  - 조건식에는 `boolean` 타입만 허용 (type coercion X)
  - if문은 `expression` 취급 (statement X)

      ```rust
      msg = if num == 5 {
          "five"
      } else if num == 4 {
          "four"
      } else {
          "other"
      };
      ```

      1. `;` 생략으로 return 표현
      2. return 사용 불가
         - return은 function body에만 사용되기에 사용 시 현재 function에서 return out
      3. 모든 block은 같은 type return
      4. 마지막에 `;`
        - if문의 값을 사용할 경우에만 `;` 필요
     
     :bulb: statement는 return 하지 X
     
  - `{ }` 는 필수

  - 삼항 연산자 없음


- unconditional loop

  ```rust
  loop {
      loop {
          break;
      }
  }
  ```

  - 특정 loop 지정으로 `break` 또는 `continue` 할 loop도 선택 O

  ```rust
  `bob: loop {
      loop {
          break `bob;
      }
  }
  ```

- conditional loop

  ```rust
  while 조건 {
      
  }
  ```

- for loop

  - iterate : `.iter()`
  	  ```rust
      for num in [7,8,9].iter() {
  	  
      }
      ```

    - collection이 ordered라면 순서대로, 아닐 시 랜덤

  - destructure 가능

    ```rust
    let array = [(1,2), (3,4)];
    for (x,y) in array.iter(){
        // do sth
    }
    ```

  - range : `..`

    ```rust
    for num in 0..50{
        // 0 ~ 49
    }
    ```

    - = 명시시, inclusive
      - `0..=50` 



### String

- at least 6 types in standard library, but 2 are most used
- str
  - called as string slice. In most case, called as borrowed string slice
  - 비교 (vs String)
    - cannot be modified`&str`
  - String type으로 변환
    - `str변수.to_string()` 메소드
    - `String::from(str변수)`
  - 구조
    - ptr (pointer)
    - len

- String

  - can be modified

  - 구조

    - ptr
    - len
    - capacity

    :bulb: str은 String의 subset구조

- 공통점

  - Valid UTF-8
  - can not be indexed by character position (like `my_string[3]`)
    - 서로 다른 언어에서 공통적으로 indexing 하는 것이 불가능
      - grapheme(자소)의 존재 정확하게 byte로 나누기 X

- 인덱싱

  - `~.bytes()` 

    - String을 byte array로 전환
    - ASC2 인 영어라면 사용성 O

  - `~.chars()`

    - Unicode scalars iterate

  - package : `unicode-segmentation`

    - `graphemes(문자열, true)`
      - return iterators that handle graphemes of various types

    :bulb: 인덱싱만 가능하다면, fase, constant-time operation O

  - iterator method : `.nth(숫자)`
    - String의 index 대신 사용



## Code Practice



### Varaibles

```rust
const STARTING_MISSILES: i32 = 8;
const READY_AMOUNT: i32 = 2;

fn main() {
    
    let (mut missiles, ready)  = (STARTING_MISSILES, READY_AMOUNT);
    // let ready = READY_AMOUNT;

    println!("Firing {} of my {} missiles...", ready, missiles);

    missiles = missiles - ready;
    println!("{} missiles left", missiles);
   
}
```



### Functions

```rust
// Silence some warnings so they don't distract from the exercise.
#![allow(unused_variables)]

fn main() {
    let width = 4;
    let height = 7;
    let depth = 10;
    // 1. Try running this code with `cargo run` and take a look at the error.
    //
    // See if you can fix the error. It is right around here, somewhere.  If you succeed, then
    // doing `cargo run` should succeed and print something out.
    
    let area = area_of(width, height);
    
    println!("Area is {}", area);

    // 2. The area that was calculated is not correct! Go fix the area_of() function below, then run
    //    the code again and make sure it worked (you should get an area of 28).

    // 3. Uncomment the line below.  It doesn't work yet because the `volume` function doesn't exist.
    //    Create the `volume` function!  It should:
    //    - Take three arguments of type i32
    //    - Multiply the three arguments together
    //    - Return the result (which should be 280 when you run the program).
    //
    // If you get stuck, remember that this is *very* similar to what `area_of` does.
    //
    println!("Volume is {}", volume(width, height, depth));
}

fn area_of(x: i32, y: i32) -> i32 {
    // 2a. Fix this function to correctly compute the area of a rectangle given
    // dimensions x and y by multiplying x and y and returning the result.
    //
    x*y
    // Challenge: It isn't idiomatic (the normal way a Rust programmer would do things) to use
    //            `return` on the last line of a function. Change the last line to be a
    //            "tail expression" that returns a value without using `return`.
    //            Hint: `cargo clippy` will warn you about this exact thing.
}

fn volume(width: i32, height: i32, depth: i32) -> i32{
    width*height*depth
}
```









