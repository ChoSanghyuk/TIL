# Linux Terminal 명령어 모음



## 기본 개념

- `$` : prompt. 명령어를 받아들일 준비가 되었다는 기호
- `/` : Root directory. 
- `~` : Home directory.
- `.` : 현재 폴더
- `..` : 현재 폴더의 상위 폴더
- `tab key` : 자동 완성 기능
- `ctrl + c` : 실행중인 프로세스 취소
- `ctrl + l ` : 터미널 창 정리하기



### 명령 문자

- `;` : 명령의 끝을 나타냄
- `||` : 이전의 명령이 실패하면 실행하는 조건문 문자""
- `&&` : 이전의 명령이 성공하면 실행하는 조건문 문자
- `&` : 명령을 백그라운드에서 실행
- `$` : 변수에 접근할 수 있는 문자
- `>` : 출력 결과 저장
  - `find ./ -name "*filname" -and -path "*path*" > data.txt`
  - 파일 미존재 시 생성, 파일 존재 시 **덮어쓰기**
- `>>` : 출력 결과 저장
  - 파일 미존재 시 생성, 파일 존재 시 **이어쓰기**



### 변수 접근 기호

- `0` : stdin(표준 입력)
- `1` : stdout (표준 출력)
- `2` : stderr (에러 출력)



## 주요 명령어

- `touch <filename>`  : 파일 생성
- `start <filename>` : 파일 실행
- `rm <filename>` : 파일 삭제
- `mkdir <dirname>` : 폴더 생성
- `mkdir -p <dirname1/dirname2>` : 중간 폴더도 같이 생성
- `cp -r <폴더> <목적지>` : 하위 폴더 및 파일까지 같이 복사 이동
- `rm -r <dirname>` : 지정한 폴더 및 파일 삭제
- `rm - rf <dirname>` : 지정판 폴더 및 파일 강제 삭제
- `ls` :  현재 위치한 폴더 내부의 파일/폴더 출력
- `ls -a` : 현재 위차한 홀더 내부의 모든 파일 / 폴더 출력
  - `ls -al ./폴더` : 대상 폴더 내부 확인
- `code . `  : 해당 디렉토리로 visual studio code 열기
- `vim <filename>` : 파일 오픈 (없으면 파일 생성)
- `vi <filename>` : 파일 오픈 
- `lsof -i :<port_number>` : 해당 포트 사용 중인 프로세스 찾기
- `cat <filename>` : 파일 내용 출력



### ls

- `ls [*options*] [*directory name*]`

- 옵션

  - -a : 숨김파일을 포함한 모든파일을 리스트

  - -t : 파일이 마지막으로 수정된 시간 순으로 출력

  - -l : 해당 디렉터리에 존재하는 파일, 디렉터리의 접근권한 등 파일정보를 출력

  - -i : incode 를 함께 출력

  - -r : 파일 및 디렉터리의 순서를 역순(reverse) 출력

  - -R : 재귀적(recursive)으로 수행되는 서브디렉터리 내용도 함께 출력

  - -S : 파일의 크기순으로 출력

  - -F : 해당 파일의 종류도 우측에 함께 출력



### find

#### 기본 구조

`find [경로] [플래그] [표현식]`

- 플래그 : -name 등의 옵션
  - 앞에 -not 으로 부정 사용 가능

- 표현식 : 검색할 파일의 이름/패턴/표현식

#### 플래그 

- `-name` : 파일/폴더의 이름 형식 지정
  - `iname ` : 대소문자 구분 x

- `-type` : 검색 대상의 파일 지정. 디폴트가 f
  - d : directory
  - f : file
- `-regextype` : 정규표현식 타입
  - posix-extended 로 지정
- `-regex` : 정규표현식 사용
- `-path` : 경로 형식 지정
- 예제
  - `find / \(-name "*.pc" -or -name "*.cpp" -or -name "*.gc" \) -and \( -not -path "*backup*" -and -not -path "*PIF" \) | xargs grep -r -i -a "테이블" 2>/dev/null`


### grep

#### 기본 구조

`grep 옵션 [문자열] [파일명]`

#### 옵션

- -a : binary 형식이 아닌 파일 형식으로 인식
- -b : 문자와 일치하는 줄의 시작점 출력
- -c : 문자와 일치하는 줄의 수 출력
- -h : 여러 파일에서 문자열을 찾을 때, 파일이름이 붙는 것을 방지
- -i : 대소문자 구분 X
- -n : 줄의 번호와 내용을 같이 출력
- -v : 문자가 포함되지 않는 행 출력
- -w : 문자와 한 단어로 일치해야 출력
- -l (소문자 엘) : 문자가 들어간 파일 이름만 출력
- -r : 하위 디렉토리에서도 문자를 찾음
- -A : 특정문자 아래 추가로 여러 행 출력
- -B : 특정 문자 위 추가로 여러 행 출력
- -E : use extended regular expressions
- -P : Perl-compatible regular expressions

#### 예제

- **grep "john" | grep -v "[[:graph:]]john\|john[[:graph:]]"**
  - john 이라는 정확히 단어가 일치하는 것만 찾음
  - john이 포함된 행을 찾고, john 앞뒤로 공백을 제외한 문자가 있을 때 제외
- **find ./ -name "*.pc" -or -name ".cpp" 2>/dev/null | xargs grep -r -i -a "문자열" 2>/dev/null**
  - find
    - -name : 파일명 지정
    - -or : 조건 추가
    - 2>/dev/null : 오류는 출력 안함

  - grep
    - -a : 
- `find / \(-name "*.pc" -or -name "*.cpp" -or "*.gc" \) -and \(-not -path "*.backup*" -and -not -path "*PIF*" \) 2>/dev/null |xargs grep -l -r -i -a "테이블" 2>/dev/null |xargs grep -i -a "컬럼" `




### 압축

- zip
  - `zip 파일명.zip 대상파일`
  - flag
    - `-e` : 비밀번호 설정
    - `-r` : 하위 폴더까지 전부 대상
  
- gz
  - `gzip [파일명]` : 파일 압축하기
  - `gzip -d [파일명].gz` : 압축풀기
  - `apt-get install gzip` : gzip 설치
- tar
  - 주요 명령어
    - `tar -cvf [처리후파일명.tar] [대상폴더명]` : tar 압축하기
    - `tar -zcvf [처리후파일명.tar.gz] [대상폴더명]` : tar.gz 압축하기
    - `tar -xvf [파일명.tar]` : tar 압축 풀기
    - `tar -zxvf [파일명.tar.gz]` : tar.gz 압축풀기
  - 옵션
    - -c : 파일을 tar로 묶음
    - -C : 경로 지정
    - -p : 파일 권한 저장
    - -v : 묶거나 풀 때 과정을 화면으로 출력
    - -f : 파일 이름을 저장
    - -x : tar 압축을 풂
    - -z : gzip으로 압축 / 해제



### 파일 시스템 용량 확인

- `df [옵션]`
  - -h : 보기 편한 용량 크기로 출력



### process 확인

- `lsof -i :<port>` 
  - 해당 port에서 동작 중인 프로세스 확인



### crontab

- `crontab -e`
  - crontab 설정 파일 열기/수정
- `@reboot /bin/bash {실행 파일}.sh >> /tmp/startup.log 2>&1`
  - 재부팅 시 실행 파일 자동 실행 및 에러 로그 저장



### maven 실행

- mvn 실행
  - `mvn --version` : 버전 확인
  - `mvn spring-boot:run -Dspring-boot.run.jvmArguments='-Dserver.port=9003'` : maven 프로젝트를 해당 port로 실행함
- jar 파일 사용
  - `mvn clean` : 기존 빌드 정보 삭제 => target 폴더 없어짐
  - `mvn compile package` : compile 후 packaging => target 다시 생성됨 (jar 생성)
  - `java -jar -Dserver.port=9004 ./target/user-service-0.0.1-SNAPSHOT.jar`
- 종료
  - crtl + c





## Command Mode

### vi/vim

- :    명령 시작
- 문서 편집
  - i    수정
  - w  저장
  - q   종료
  - wq, x  저장 & 종료
  - q!  강제 종료 (저장x 종료)
- undo/redo
  - u : 가장 마지막 명령 취소
  - U : 해당 라인 전체 수정사항 취소
  - ctrl + r : 원하는 만큼 redo
- 이동
  - ctrl + f : 다음 화면
  - ctrl + b : 이전 화면 
  - gg : 첫 화면
  - shift + g : 마지막 화면
- 찾기
  - /글자 : 아래쪽으로 찾기
    - \c 추가 시, 대소문자 구분 X
  - ?글자 : 위쪽으로 찾기
  - `*`: 현재 커서가 있는 단어와 동일 단어 찾기
  - n : 다음 단어
  - N : `*`시 위쪽으로 찾기

유용 :link: https://iamfreeman.tistory.com/entry/vi-vim-%ED%8E%B8%EC%A7%91%EA%B8%B0-%EB%AA%85%EB%A0%B9%EC%96%B4-%EC%A0%95%EB%A6%AC-%EB%8B%A8%EC%B6%95%ED%82%A4-%EB%AA%A8%EC%9D%8C-%EB%AA%A9%EB%A1%9D



## 시스템 환경 변수 등록하기

### PATH 등록

- 일시 등록

  ```SH
  export PATH=$PATH:/mnt/c/Users/chosh/lecture_code/besu-24.1.0/bin
  ```

- 영구적 등록

  - .bashrc 파일 open

    - `vim ~/.bashrc`

  - Path 등록 및 저장

    1. i로 수정모드 시작
    2. 입력 : `export PATH=$PATH:/mnt/c/Users/chosh/lecture_code/besu-24.1.0/bin`

    3. `:w`로 저장

:bulb: `$PATH` 

- $PATH: 를 앞에 추가해 준 것은 기존 PATH 환경변수 뒤에 내용을 추가한다는 의미
- **이부분을 빼먹으면 기존 설정은 삭제됨**



## 사용 기능



### 터미널 이전 메시지 완전히 삭제

```sh
clear && printf '\e[3J'
```

