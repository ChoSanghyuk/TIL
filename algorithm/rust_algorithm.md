# Rust 알고리즘 주요 메소드 정리



## 목차



## Array

:bulb:array는 compile 시에 size가 정해져 있어야 함으로, constant value가 아닌 값으로 생성 X.



## String

- 초기화

```rust
let mut string_line = String::with_capacity(3); // 문자열 크기 미리 지정
```

- 메소드

```rust
string_line.push_str("*"); // 문자열 이어 붙이기
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

  



## Vec

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

