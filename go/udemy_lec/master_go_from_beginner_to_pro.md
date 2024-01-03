# Master Go (Golang) from Beginner to Pro



## Getting Started



### Main 패키지

- 실행가능한 모듈의 패키지 이름은 main
- **main 안에는 main func이 포함되어야 함**

:bulb: Convetion

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
  
          - `%#v` : representation of the value
  
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

  - untyped로 초기화 시, 첫번째 사용 전까지 strong type의 제약에서 어느 정도 자유로워짐

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

  





























