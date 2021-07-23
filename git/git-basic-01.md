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

 서명에 사용할 이름 설정

```
$ git config --global user.email <email>
```

 서명에 사용할 이메일 설정



## command

```
$ git init
```

  해당 폴더를 초기화 하는 코드로, 현재 디렉토리를 기준으로 Git 저장소가 생성됨

  해당 폴더에  .git 이라는 하위 디렉토리를 생성하는데,  .git 디렉토리에는 저장소에 필요한 뼈대 파일들이 들어있음


```
$ git add .
```
   디렉토리 내의 모든 파일이 Git으로 관리되도록 추가해줌


```
$ git commit -m "<commit message>"
```
  추가된 내용을 message로 남기면서 commit 수행

  commit을 통해서 폴더 안에서 일어난 변경사항들을 확정


```
$ git remote add <name> <URL>
```
`<url>` 을 이름이 `<name>` 인 변수에 담아 관리함.   + 해당 폴더(로컬)를 원격 저장소에 연동시킴

 보통 이름에 origin 을 많이 사용


```
$ git push <name> <branch>
```
  확정된 변경 사항을 원격 저장소에 밀어넣음

 보통 `<branch>` 는 master 사용


```
$ git log 
```
   git 수행 history를 불러옴


```
$ git status
```

  현재 git의 상태를 알려줌

```
$ git clone <url>
```

  해당 url에 있는 내용들 모두다 받아옴
