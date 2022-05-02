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



## 주요 명령어



- `touch <filename>`  : 파일 생성
- `start <filename>` : 파일 실행
- `rm <filename>` : 파일 삭제
- `mkdir <dirname>` : 폴더 생성
- `mkdir -p <dirname1/dirname2>` : 중간 폴더도 같이 생성
- `rm -r <dirname>` : 지정한 폴더 및 파일 삭제
- `rm - rf <dirname>` : 지정판 폴더 및 파일 강제 삭제
- `ls` :  현재 위치한 폴더 내부의 파일/폴더 출력
- `ls -a` : 현재 위차한 홀더 내부의 모든 파일 / 폴더 출력
  - `ls -al ./폴더` : 대상 폴더 내부 확인

- `code . `  : 해당 디렉토리로 visual studio code 열기
- `vim <filename>` : 파일 오픈 (없으면 파일 생성)
- `vi <filename>` : 파일 오픈 



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
  - ctrl + d : 이전 화면
- 찾기
  - /글자 : 아래쪽으로 찾기
  - ?글자 : 위쪽으로 찾기
  - `*`: 현재 커서가 있는 단어와 동일 단어 찾기
  - n : 다음 단어
  - N : `*`시 위쪽으로 찾기
