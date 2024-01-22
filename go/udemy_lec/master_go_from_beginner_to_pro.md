# Master Go (Golang) from Beginner to Pro



## Getting Started



### Main 패키지

- 실행가능한 모듈의 패키지 이름은 main
- **main 안에는 main func이 포함되어야 함**

:bulb: Convention

- `main.go`은 `package main`으로 명명하는것이 컨벤션
- 폴더 이름이 패키지 이름이 되는 것이 컨벤션



### Compile & Build & Run

- `run`
  - it compiles and runs the application. It **doesn’t produce an executable**
  - 코드
    - `go run file.go` 
    - `go run -x main.go` : with detail
- `build`
  - it just compiles the application. It **produces an executable**
  - 코드 
    - `go build file.go` : compiles a bunch of Go source files. It compiles packages and dependencies
    - `go build` :  compile the files in the current directory and will produce an executable file with the name of the current working directory
    - `go build -o app` : produce an executable file called app
      - `go build -o 이름.exe main.go`
  - 타 OS 파일로 생성하기
    - Compiling for Windows: `GOOS=windows GOARCH=amd64 go build -o winapp.exe`
    - Compiling for Linux: `GOOS=linux GOARCH=amd64 go build -o linuxapp`
    - Compiling for Mac: `GOOS=darwin GOARCH=amd64 go build -o macapp`
- `install`
  - it just compiles the application. It **produces an executable**
  - vs build
    - `go build`
      - place the resulting executable in the current directory
    - `go install`
      - move the executable to GOPATH/bin
      - you use paths relative to GOPATH/src



### formatting

- 개요

  - Go strongly suggests certain styles
  - `gofmt` formats a program's source code in an idiomatic way that is easy to read and understand.
  - VSCode 사용 시, 파일 저장 때 마다 자동 실행

- 코드

  - `gofmt -w main.go`
    - -w : overrite the source

  - `gofmt -w -l 폴더명`
    - 폴더명 내 파일 대상

  - `go fmt`
    - 현재 폴더 전체 대상



## Go Basics



### Variable

- 개요

  - created at runtime.
  - **A declared variable must be used** or we get an error
  - `_` is the **blank identifier** and mutes the compile-time error returned by unused variables

- 선언하기 (declare)

  1. var keyword
     - `var x int = 7`
     - `var s1 string; s1 = "Learning Go!"`
  2. Short Declaration Operator
     - `age := 30`

  :bulb: `Go`는 `C`와 기타 언어와 다르게 변수 선언을 왼쪽에서 오른쪽으로 읽을 수 있도록하여 가독성을 높임

- 주요 사용법 및 특징

  - `;`는 한 라인에 여러개 statement  작성용

  - `:=`를 통해 한번에 여러개 선언할 때 선언 변수 중 새거가 하나만 있으면 됨

  - Short declaration은 package scope에서 사용 X

  - Go는 statically typed language

    - complie 시 타입 체크

    - 타입이 명시되어 있거나 추론 가능해야 함

  - Strong type language
    - 다른 타입으로 입력 X



### Naming Convention

- Names start with a letter or an underscore (_)

- Case matters: quickSort and QuickSort are different variables

- Go keywords (25) can not be used as names

- Use the first letters of the words 

  - `var mv int //mv -> max value`

- Use fewer letters in smaller scopes and the complete word in larger scopes 

  ```go
  var packetsReceived int // NOT OK, too verbose 
  var n int //OK -> number of packets received 
  var taskDone bool //ok in larger scopes
  ```

- **Don’t use _** , it’s not idiomatic in GO

  - use camelCase rather than underscores to write multi word names
  - applicable to variables, constants or functions

  ```go
  const MAX_VALUE = 100 // NOT OK
  const N = 100 //OK, IDIOMATIC
  ```

- An **uppercase first letter** has special significance to go

  - it will be **exported**

- write acronyms(두문자어) in all caps

  ```go
  writeToDB // recommended
  writeToDb // not recommended
  ```

- packages are given lower case, single-word names

-  doesn't provide automatic support for getters and setters

  - If you have a field in a struct called owner, 

    => the **getter method** should be called **Owner** (upper case, exported)

    =>  A **setter function**, if needed, will likely be called **SetOwner**.

- one-method interfaces are named by the method name plus an -er suffix: Reader, Writer, Formatter, etc





### Package fmt

:link: https://pkg.go.dev/fmt#Printf

- `Printf`

  - 개요

      ```go
      func Printf(format string, a ...any) (n int, err error)
      ```

      - formats according to a format specifier and writes to standard output
      - 자동 new line X
      
  - format
  
      - `%+d` : 숫자 앞에 `+` 표기하며 출력
  
      - `%.3f` : 소수점 3자리까지만 표기
  
      - `%q` : quoted String
  
      - `%v` : any type
  
          - `%#v` : 타입 + 값
  
      - `%T` : print Type
  
      - `%t` : bool
  
      - `%p` : pointer (address in base 16, with leading 0x)
  
      - `%b` : base2 (2진수)
  
          - `%08b` : 8bit 형식으로 base2 출력 (앞을 0으로 채움)
  
      - `%x` : hexadecimal (16진수)
  
      - `%c` : char (rune) represented by the corresponding Unicode code point
  
          



### Constants

- 개요

  - represent fixed (unchanging) values

  - use constants to avoid possible errors (variables that change when they shouldn’t) or to replace a value only in one place instead of in many places

  - All basic literals (1, 3.4, “hello”, true) are in fact **unnamed constants**

  - **A constant belongs to compile time** and it’s created at compile time

    - Go can not detect runtime errors at compile-time but constants belong to compile time 

      => errors can be detected earlier

- Constant Rule

  1. 수정 불가
  2. runtime에 초기화 불가
  3. constant를 초기화 하는데 varaible 사용 불가
     - 단, string 값에  `len` 메소드를 사용하는 것과 같이 compile 시 값을 알 수 있는 경우는 메소드 사용 가능

- 주요 특징

  - 선언 후 사용 X => 에러  X
  - declare과 동시에 초기화 필요

  :bulb: const 로 선언 불가

- 다중 선언

  ```go
  // 1
  const n, m int = 4, 5
   
  // 2
  const (
      a         = 5   // untyped constant
      b float64 = 0.1 // typed constant
  )
  
  // 3
  const (
      min1 = -500
      max1 //gets its type and value form the previous constant. It's 500
      max2 //in a grouped constants, a constant repeats the previous one -> 500
  )
  ```

- Constant Expression : Typed vs Untyped

  ```go
  const x = 5				// untyped (typeless)
  const y float64 = 1.1	// typed
  
  fmt.Println(x * y) // No Error because x is untyped and gets its type when its used first time (float64).
  ```

  - untyped로 초기화 시, 첫번째 사용에 의해서 implicitly converted되어서 type 지정됨

    :bulb: 일반 variable의 경우, 초기화 시 default type으로 배정

- Iota

  - `const ( )` 내에서 0부터 증가하는 값으로 사용 O
  - 다른 `const ( )` 에서는 다시 처음부터 0부터 증가
  - 중간값을 스킵하고 싶을 때엔 `_` 활용

  ```go 
  // IOTA
  // iota is number generator for constants which starts from zero 
  // and is incremented by 1 automatically.
  
  const (
      c1 = iota
      c2 = iota
      c3 = iota
  )
  fmt.Println(c1, c2, c3) // => 0 1 2
  ```

  



### Data Type

- 개요

  - A type determines a set of values together with operations and methods specific to those values

- 종류

  - predeclared types
  - introduced types 
    - with type declarations 
  - composite types: 
    - ex) array, slice, map, struct, pointer, function, interface, channel types

- **predeclared types (Built-in types)**

  - Numeric types

    - int8, int16, int32, int64
    - uint8, uint16, uint32, uint64
      - used to represent unsigned (positive) integers
    - uint
      - alias for uint32 or uint64 based on platform
    - int
      - alias for int32 or int64 based on platform
    - float32, float64
      - zero before the decimal point separator can be omitted
        - ex)  -.5
    - complex64, complex128
    - byte
      - alias for uint8
      - used for character value
    - rune
      - alias for int32
      - used for character value

    :bulb: data type 범위 넘어가는 수 입력 시, compile error

  - Bool type

    - pre-defined constants true and false

  - String type

    - Unicode chars written enclosed by double-quotes
    - A string value is a (possibly empty) sequence of bytes

- **Introduced Types**

  - introduce new types using the `type` keyword

- **Composite Types:**

  - Array and Slice Type

    -  a numbered sequence of elements of a single type

    - array : a fixed length

      ```go
      var numbers = [4]int{4, 5, -9, 100}
      fmt.Printf("%T\n", numbers) // =>  [4]int
      ```

    - slice : dynamic length (can shrink or grow )

      ```go
      var cities = []string{"London", "Bucharest", "Tokyo", "New York"}
      fmt.Printf("%T\n", cities) // => []string
      ```

  - Map Type

    - unordered group of elements of one type, indexed by a set of unique keys of another type

    ```go
    balances := map[string]float64{
        "USD": 233.11,
        "EUR": 555.11,
    }
    ```

  - Struct Type (User defined type)

    - A struct is a sequence of named elements, called fields, each of which has a name and a type

    ```go
    type Car struct {
     brand string
     price int
    }
    ```

  - Pointer Type

    -  a variable that stores the memory address of another variable
    - The value of an uninitialized pointer is nil.

  - Function and Interface Type

  - Channel Type

    - provides a mechanism for concurrently executing functions to communicate by sending and receiving values of a specified element type



### Operations on Types

- Arithmetic and Bitwise Operators
  - +, -, *, /, %, &, |, ^, <<, >>
- Assignment Operators
  - +=, -=, *=, /=, %=
- Increment and Decrement Statements
  - ++, --
- Comparison Operators
  - ==, !=, <, >, <=, >=
- Logical Operators
  - &&, || , !
- Operators for Pointers
  - &
- Channels
  - <-

:bulb: `Println(x++)` 는 허용 X



### Overflows

- Go는 c, c++, java과 같이 overflow 체크하지 않음
  - 효율성 문제
- int 타입의 경우, overflow 발생 시, 다시 초기값부터 순환
- float 타입의 경우, `+Inf`로 넘어감 (Infinite)



### Converting Numeric Types

- Go에서는 Casting이 아닌 Converting이라 표현

- Go에서는 다른 이름의 type이면 다른 Type

  - int, int64 사이에도 converting이 필요
  - 단, alias 제외

- converting 예시

  - string(99) 

    - int to rune (Unicode code point)

      => the ascii code for symbol c

  - `fmt.Sprintf(format string, a ...any)`

    - 포맷에 변수들을 대입하여 string으로 반환

  - `strconv`

    - common numeric conversions

        ```go
        i, err := strconv.Atoi("-42") 	// string to int
        s := strconv.Itoa(-42)			// int to string
        ```
    
    - ParseBool, ParseFloat, ParseInt, and ParseUint convert strings to values
    
        ```go
        b, err := strconv.ParseBool("true")
        f, err := strconv.ParseFloat("3.1415", 64)
        i, err := strconv.ParseInt("-42", 10, 64)		// 값, 진수, bitSize. 진수가 0일 경우, 값을 보고 유추
        u, err := strconv.ParseUint("42", 10, 64)
        ```
    
        - The parse functions return the widest type (float64, int64, and uint64), but if the size argument specifies a narrower width the result can be converted to that narrower type without data loss:
    
    - FormatBool, FormatFloat, FormatInt, and FormatUint convert values to strings:
    
      ```go
      s := strconv.FormatBool(true)
      s := strconv.FormatFloat(3.1415, 'E', -1, 64)
      s := strconv.FormatInt(-42, 16)
      s := strconv.FormatUint(42, 16)
      ```



### Defined Types (named type)

- 개요

  - a new type created by the programmer from another existing type which is called the **underlying** or **source type**
  - A new defined type must have a new name and can have its new methods
  - The underlying type provides the representation, operations and size of the newly defined type
  - Even though the defined type and the source type share the same representation, operations and size, **they are different types**
  - a completely new type and a type can be **converted** to another type if they share the same underlying type

  - no type-hierarchy in Go

- 용도

  - **attach methods** to newly defined types

  - **Type safety**

    - We must convert one type into another to perform operations with them

  - **Readability**

    - `type usd float64` 

      =>  know that new type represents the US Dollar

- 예시

  ```go
  // new type speed (underlying type uint)
  type speed uint
  
  // s1, s2 of type speed
  var s1 speed = 10
  var s2 speed = 20
  
  // convert
  var x uint
  x = uint(s1)
  ```



### Aliases

- form
  - `type T1 = T2`
    - T2에 대해 새 이름 T1 소개

- 개요
  - **same type with a new name**
  - can be used together in operations without type conversions
  - **shoud use it with caution**, it's not for everyday use



## Program Flow Control in Go



### If, Else If, Else Statement

- 기본
    ```go
    if condition_that_evaluates_to_boolean{
         perform action1
    }else if condition_that_evaluates_to_boolean{
         perform action2
    }else{
         perform action3
    }
    ```
	- non bool value는 조건식에 사용 X

- Simple If Statement

  ```go
  if initialization statement ; condition{
       perform action1
  }else if initialization statement ; condition{
       perform action2
  }else{
       perform action3
  }
  ```

  => 주요 error handling에 이용

  ```go
  // i and err are variables scoped to the if statement only
  if i, err := strconv.Atoi("34"); err == nil {
      fmt.Println("No error. i is ", i)
  } else {
      fmt.Println(err)
  }
  ```

  



### Command Line Arguments: os.Args

- 개요

  - 터미널에서 `go run main.go` 이후의 문자열을 string 배열에 담아서 출력시킴
  - space 단위로 separate
  - 첫번째 인자는 파일의 path, 두번째 인자부터 command argument

- 샘플

  ```go
  fmt.Println("os.Args:", os.Args) // os.Args is slice of strings ([]string)
   
  // accessing command line arguments using indexes
  fmt.Println("Path:", os.Args[0])
  fmt.Println("1st Argument:", os.Args[1])
  fmt.Println("2nd Argument:", os.Args[2])
  fmt.Println("No. of items inside os.Args:", len(os.Args))
  ```



### For Loops

- 개요

  - Go에는 while loop 없음. 오직 for 문

- 형태

  - 기본 형식

  ```go
  for initialization statement ; testing boolean expression ; post statement {
      perform action
  }
  ```

  - post statement은 내부로 이동 O


  ```go
  for initialization statement ; testing boolean expression {
      perform action
      post statement
  }
  ```

  - while 문처럼 사용


  ```go
  for testing boolean expression {
      perform action
      post statement
  }
  ```

  - iterate


  ```go
  for index, value := range arrayVar {
  
  }
  ```

-  기타 문법

  - continue, break문은 타 언어와 동일



### Label Statement

- 개요
  - 코드의 특정 부분을 label로 지정할 수 있음
  - Labels are used in break, continue, and goto statements.
  - It is illegal to define a label that is never used.
  - In contrast to other identifiers, labels are not block scoped and do not conflict with identifiers that are not labels. 
    - They live in another space.
    - label은 변수명과 충돌나지 않음 (ex. outer라는 변수가 존재하여도 outer라는 lable 사용 O)
  - The scope of a label is the body of the function in which it is declared and excludes the body of any nested function.
  - Most of the time labels are used to terminate outer enclosing loops
    - 다중 loop에서 loop에 label을 붙일 수 있음 => 특정 label break

- 코드

    ```go
    outer:	// label, it doesn't conflict with other names
        for index, name := range people {  
            for _, friend := range friends { 
                if name == friend {
                    fmt.Printf("FOUND A FRIEND: %q at index %d\n", friend, index)
                    break outer //breaking outside the outer loop which terminates
                }
            }
        }
    ```

    

### go to statement

- 개요

  - 같은 함수 내 label로 이동
  - `break`, `continue`는 for, switch에서만 사용 가능한 것에 비해 `go to`는 제약 X
  - for문 처럼 사용 O

  :bulb: 단, 새 변수가 선언된 곳 이후의 label로는 `go to` 불가

- 코드

  ```go
  i := 0
  loop: // label
      if i < 5 {
          fmt.Println(i)
          i++
          goto loop
      }
  ```

  

### switch

- 특징

  - case 문 안에 `break` 명시할 필요 X
  - `case A , B :` => A or B와 일치 시 실행

- 코드

  - 기본 형식

      ```go
      language := "golang"
      
      switch language {
          case "Python": //values must be comparable (compare string to string)
              fmt.Println("You are learning Python! You don't use { } but indentation !! ")
          case "Go", "golang": //compare language with "Go" OR "golang"
              fmt.Println("Good, Go for Go!. You are using {}!")
          default:
              // and gets executed if no testing condition is true.
              fmt.Println("Any other programming language is a good start!")
      }
      ```

  - comparing bool

      ```go
      n := 5
      // comparing the result of an expression which is bool to another bool value
      switch true {
          case n%2 == 0:
              fmt.Println("Even!")
          case n%2 != 0:
              fmt.Println("Odd!")
          default:
              fmt.Println("Never here!")
      }
      ```

      - 이때 swtich 옆 true를 생략해도 같은 구문

  - Switch simple statement

      ```go
      switch n := 10; true {
          case n > 0:
              fmt.Println("Positive")
          case n < 0:
              fmt.Println("Negative")
          default:
              fmt.Println("Zero")
      }
      ```




### Scopes in Go

- 개요
  - Scope means visibility.
  - The scope or the lifetime of a variable is the interval of time during which it exists as the program executes.
  - **A name cannot be declared again in the same scope** (for example a function in the package scope), but it can be declared in another scope.
    - scope가 다른면 덮어쓰기 가능
- 종류
  - File Scope
    - import statements
  - Package Scope
    - 블록 밖 선언 (함수 포함)
    - Package Scope에서 선언된 변수는 사용 X해도 에러 나지 않음
  - Block (local) Scope
    - 함수 블록 or 여타 블록 내



## Arrays & Slice



### Arrays

- 개요

  - An array is a composite, indexable type that stores a collection of elements of same type

  - An array has a fixed length

  - Every element in an array (or slice) must be of same type

  - Go stores the elements of the array in contiguous memory locations and this way it’s very efficient

  - The length and the elements type determine the type of an array. 
    - The length belongs to array type and it’s determined at compile time
    - `accounts := [3]int{50, 60, 70 }` 
      - The array called accounts that consists of 3 integers has it’s type `[3]int`.

- 코드

  ```go
  var a1 = [4]float64{}                           //initialized with defaults (0)
  var a2 = [3]int{5, -3, 5}                       //initialized with the given values
  a3 := [4]string{"Dan", "Diana", "Paul", "John"} //short declaration syntax
  a4 := [4]string{"x", "y"}                       //initializing only the first 2 elements
  a5 := [...]int{1, 4, 5}							//initializing by using the ellipsis operator (...) 
  a6 := [...]int{1,								// declare an array on multiple lines
      2,
      3,
  }
  balances := [2][3]int{							// declaring a multi-dimensional arrays
      [3]int{5, 6, 7}, 							//[3]int is optional
      {8, 9, 10},
  }
  ```

  - ellipsis operator(...)
    - array 길이 지정을 compiler에게 맡김 => 자동 지정
  - multiple lines declaration
    - for better readability
    - **the ending comma is mandatory

- Arrays with keyed elements

  ```go
  // each key corresponds to an index of the array
  grades := [3]int{ //the keyed elements can be in any order
      1: 10,
      0: 5,
      2: 7,
  }
  
  // un unkeyed element gets its index from the last keyed element
  cities := [...]string{
      5:        "Paris",
      "London", // this is at index 6
      1:        "NYC",
  }
  ```
  
  - 특정 인덱스의 값을 지정할 수 있음
  
  - 키 미지정 시 default 값
  
    => 대부분이 default값이고 몇몇 index만 값을 가질 때 유용
  
  - ellipsis로 길이 지정 시, 최대 key index값을 기준으로 길이 확정
  - key 미지정하고 값 추가시, 마지막 key index 다음부터의 값으로 추가

- 특징

  - array는 `=`로 복사 시, 깊은 복사
  
    <=> slice는 얕은 복사



### array vs slice

- 차이점

  | Array                                                        | Slice                                                        |
  | ------------------------------------------------------------ | ------------------------------------------------------------ |
  | Has a fixed length defined at compile time                   | Has a dynamic length (it can shrink or grow)                 |
  | The length of an array is part of its type, defined at compile time and cannot be changed | The length of a slice is not part of its type and it belongs to runtime |
  | By default an uninitialized array has all elements equal to zero | An uninitialized slice is equal to nil (its zero value is nil) |

- 공통점

  - Both a slice and an array can contain only the same type of elements
  - We can create a keyed slice like a keyed array



:bulb: nil은 value의 부재를 의미하는 것이 아닌, value가 initialize 되지 않았음을 의미

​	=> slice가 nil인 것은 0 capacity slice인 것을 의미



### slice

- 초기화

  ```go
  numbers := []int{2, 3, 4, 5} // on the right hand-side of the equal sign is a slice literal
  nums := make([]int, 2)		 // creating a slice with 2 int elements initialized with zero.
  ```

- index

  - slice에서 index는 길이보다 작은 값에 한해서 사용 O

    => 아닐 시 에러

- 비교

  - - `==` 는 오직 `nil`하고만 가능
      - slice 끼리 `==`를 통한 비교 X

  - loop를 통한 비교

    ```go
    var eq bool = true
    if len(a) != len(b) {
        eq = false
    }
    
    for i, valueA := range a {
        if valueA != b[i] {
            eq = false // don't check further, break!
            break
        }
    }
    ```

- 복사
  - `dst := src`
    - 얕은 복사
  - `copy(dst,src)`
    - src의 값을 dst로 깊은 복사
    - src와 dst 중 최소 길이 만큼만 복사됨
  - `변수 := slice[i]` 
    - 깊은 복사



### Slice’s Backing (Underlying) Array

- 개요
  - When creating a slice, behind the scenes Go creates a hidden array called Backing Array.
  - The backing array stores the elements, not the slice.
  - Go implements a slice as a data structure called slice header.
- Slice Header 구성
  - **the address** of the backing array (pointer)
  - **the length** of the slice. 
    - `len()` returns it.
  - **the capacity** of the slice. 
    - 해당 slice의 시작 위치부터 backing array의 끝 부분까지의 길이
    - 즉, 얼마큼까지 재할당이 필요없는지를 나타냄
    - `cap()` returns it
- 특징
  - Slice Header is the runtime representation of a slice.
  - A nil slice doesn't a have backing array.
  - slice 변수 자체는 header로 구성되어 있기에, array보다 사이즈가 작을 수 있음



### slice expression

- 개요

  - a slice expression is formed by specifying a start or a low bound and a stop or high bound 
    - `s1[start:stop]`
  - arrays, slices and strings are sliceable
  - slicing doesn't modify the array or the slice, it returns a new one

- 특징

  - **새로운 backing array 생성 X**, backing array 공유

    => 하나만 바뀌어도, backing array를 공유하는 모든 변수들 같이 변경

  - 최대 자신의 capacity만큼 slicing으로 접근 O

    ```go
    s1 := []int{1, 2, 3, 4, 5}
    s2 := s1[:3]
    fmt.Println(s2[:5]) // backing array의 기준 최대치까지 slice 가능
    ```

    :bulb: 단, index로는 자신의 최대 길이까지만 접근 O. `s2[4]` 불가



### append

- 개요

  - slice 마지막 부분에 요소 추가

- 코드

  - `append(s1,e1)` 
    - 단일 요소 추가
  - `append(s1,e1,e2)` 
    - 복수 요소 추가
  - `append(s1,s2...)` 
    - 복수 요소 추가 with ellipsis operator

- 특징

  - capacity를 벗어나지 않는다면, backing array를 수정

    ```go
    s1 := []int{1, 2, 3, 4, 5}
    s2 := s1[0:3]
    s3 := append(s2, 6) // s1 : {1,2,3,6,5}
    ```

  - capicity를 벗어나는 `append`에 대해서는 **새로운 backing array 생성**





## String



### String

- 개요

  - Golang doesn’t have a char data type. It uses byte and rune to represent character values.
    - **byte** and **rune** data types are used to distinguish characters from integer values
  - Characters or rune literals are expressed in Go by enclosing them in single quotes, as in 'x' or '\n' . 
    - Rune literals such as ‘a’ , ‘b’, ‘c’, ‘x’ or ‘\n’ are represented using **Unicode Code** Points. 
    - A code point is a numeric value that represents a rune literal.
  - The character encoding scheme ASCII which is a Unicode subset, comprises 128 code points.
  - A string is a series of bytes values. 
    - A string is a slice of bytes and any byte slice can be encoded in a string value.

- 주요 사용법

  - raw string

      ```go
      s2 := `Hi 
      there Go!` // => `` 안에 있는 형태 그대로 출력
      ```

  - concatenating strings
  
      - `s1 + s2`
  
- 특징

  - **string is immutable** in Go
    - concatenate 시, 완전 새로운 문자열 생성
    - 수정 불가. `s3[5] = 'x' // => error`
  
  - Unicode byte의 series
    - indexing 시, 실제 값이 아닌 Unioce 값 조회
    - `len(s1)` 시, 실제 문자열 길이가 아닌, bytes의 길이 조회
  

- `utf8` package

  - `utf8.RuneCountInString(str)` 

    - 실제 문자열 길이 반환

  - `r, size := utf8.DecodeRuneInString(str[i:])`

    - i부터 시작하는 하나의 문자열 반환. 이때 사용된 byte 개수 반환

      ```go
      for i := 0; i < len(str); {
          // it returns the rune in string in variable r and its size in bytes in variable size
          r, size := utf8.DecodeRuneInString(str[i:]) 
          fmt.Printf("%c", r) 		       // printing out each rune
          i += size           			   // incrementing i by the size of the rune in bytes
      }
      ```

    :bulb: `range`로 bytes => string

    ```go
    str := "안녕하세요"
    
    for i, r := range str { //the first value returned by range is the index of the byte in string where rune starts
        fmt.Printf("%d -> %c", i, r) // => ţară
    }
    ```

    

















