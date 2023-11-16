# Git Basic 01

## 개요

:green_apple: git bash를 이용하여 github 관리하기 



## 주의사항

1. Home folder에서 git init 하지 않는다.
2. repo 안에 repo를 만들지 않는다



## 닉네임 등록

```
$ git config --global user.name <이름>
```

- 서명에 사용할 이름 설정



```
$ git config --global user.email <email>
```

- 서명에 사용할 이메일 설정



## 연결

```
$ git init
```

- 해당 폴더를 초기화 하는 코드로, 현재 디렉토리를 기준으로 Git 저장소가 생성됨

- 해당 폴더에  .git 이라는 하위 디렉토리를 생성하는데,  .git 디렉토리에는 저장소에 필요한 뼈대 파일들이 들어있음




```
$ git remote add <name> <URL>
```

- `<url>` 을 이름이 `<name>` 인 변수에 담아 관리함.   + 해당 폴더(로컬)를 원격 저장소에 연동시킴
- 보통 이름에 origin 을 많이 사용



```
$ git remote remove <name>
```

-  remote 취소



```bash
$ git remote -v
```

- remote url 확인



## 업로드


```
$ git add .
```
- 디렉토리 내의 모든 파일이 Git으로 관리되도록 추가해줌




```
$ git commit -m "<commit message>"
```
- 추가된 내용을 message로 남기면서 commit 수행

- commit을 통해서 폴더 안에서 일어난 변경사항들을 확정



```
$ git commit -am "<commit message>"
```

- add와 commit 동시에
- 단, 이전에 commit 했었던 대상에 한해서




```
$ git push <name> <branch>
```
-  확정된 변경 사항을 원격 저장소에 밀어넣음
-  보통 `<branch>` 는 master 사용




```
$ git push -u <name> <branch>
```

- 디폴트 설정 : 앞으로 git push만 해도 해당 이름의 브랜치로 push(pull) 됨



## 다운로드

```
$ git clone <url>
```

- 해당 url에 있는 내용들 모두다 받아옴



```
$ git clone <url> <폴더이름>
```

- 해당 url에 있는 내용들을 해당 폴더를 생성하여 모두 다 받아옴



```
$ git pull origin master
```

- origin을 내 repository로 가지고 와 합침 (merge)



```
$ git fetch origin master
```

- merge 없이, origin을 일단 가지고만 옴



## 내역 취소

```
$ git reset --hard HEAD^
```

- 옵션
  - --hard HEAD^ : 수정한 내용까지 모두
  - --mixed HEAD^ : add한 내용까지 취소 (default)
  - --soft HEAD^ : commit한 내용까지 취소
- ^
  - 가장 최근 커밋으로부터 하나 전으로 되돌림
  - ex) ^^ : 두개 전으로 되돌림



```
$ git revert <commit hash>
```

- 해당 commit 내용으로 되돌림
- 다음 commit 내역이 과거의 commit이 됨
  - reset의 경우, 이력 삭제



### COMMIT 되지 않은 사항 폐기

```
$ git checkout .
```

- 커밋되지 않은 **모든 로컬 변경 사항을 되돌립니다**



### Local / Head Replace

#### Local <= Head 

```bash
git checkout origin/master file.md
```

#### Head <= Local

```
git checkout file.md
```





## 비교



```
$ git diff <비교대상 commit hash> <기준 commit hash>
```

- 비교대상 commit에 비해 기분 commit은 무엇이 달라졌는가



```
$ git diff HEAD HEAD^
```

- 최신 commit과 그 전 commit 비교
- ^개수만큼 이전 commit



```
$ git diff HEAD
```

- 이전 commit과 현재 수정된 내용 비교



```
$ git diff <비교대상 branch 이름> <기준 branch 이름>
```

- 브랜치간 비교  



## 상태


```
$ git log 
```
- git 수행 history를 불러옴




```
$ git status
```

-  현재 git의 상태를 알려줌



```
$ git log --oneline
```

