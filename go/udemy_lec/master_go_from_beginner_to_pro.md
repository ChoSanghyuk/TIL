# Master Go (Golang) from Beginner to Pro



## Getting Started



### Main 패키지

- 실행가능한 모듈의 패키지 이름은 main
- **main 안에는 main func이 포함되어야 함**



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

















