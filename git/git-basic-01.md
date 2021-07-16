# Git Basic 01

## 개요

:green_apple: git bash를 이용하여 github 관리하기 

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
$ git remote add origin <URL>
```
  해당 폴더(로컬)를 원격 저장소에 연동시킴
  

```
$ git push origin master
```
  확정된 변경 사항을 원격 저장소에 밀어넣음
  

```
$ git log
```
   git 수행 history를 불러옴
   

```
$ git status
```

  현재 git의 상태를 알려줌

