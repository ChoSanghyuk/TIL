# git branch



## Tip

- local branch는 remote branch와 이름이 같은 경우에만 push가 가능하다.
  - pull은 달라도 상관 X
- :
  - <`source>:<destination>` 으로 구분됨



## branch 보기

```
$ git branch
```

- local branch 목록 확인



```
$ git branch -r
```

- remote branch 목록 확인



```
$ git branch -a
```

- list all remote branches



## branch 정보 갱신

```
$ git remote update
```

- 원격 branch 정보 갱신



## branch 이동

```
$ git checkout <브랜치이름>
```

- 해당 브랜치로 이동



```
$ git checkout -b <브랜치이름>
```

- 브랜치 생성 후 이동

```
$ git checkout -b "develop" "origin/develop"
```

- 브랜치 pull 받아 오면서 로컬 브랜치 생성 후 이동

```
$ git switch master (혹은 branch이름)
```

- 작업하려는 곳을 바꿈

- :bulb: checkout
  - switch ||  restore
  - 더 이상 사용 권장 X



## branch 생성

### 로컬 브랜치에서 원격으로 반영하기

```
$ git branch <브랜치이름>
```

- 로컬 브랜치 생성

```
$ git push origin master:브랜치이름
```

- 해당 이름의 브랜치에 올림 (없으면 생성되고 올려짐)

```
$ git switch -c <브랜치이름>
```

- 브랜치를 생성하면서 작업 공간을 옮김



### 원격 브랜치 로컬에 반영하기

```bash
$ git checkout -t [원격 브랜치명]
```

- 원격 레포지토리의 브랜치를 로컬 브랜치에 적용하기 (영구적 tracking)

```bash
$ git remote update
```

- 원격 레포지토리의 브랜치 들이 로컬 레포지토리에도 반영



## branch 삭제

### 로컬 branch 삭제

```
$ git branch -d <branchname>
```

- safe delete. unmerged된 상태라면 삭제 X



```
$ git branch -D <branchname>
```

- hard delete. 걍 지워버림



### 원격 branch 삭제

```
$ git push origin -d <remote branch>
```

- remote branch 삭제



## branch 수정

```
$ git branch -m <새이름>
```

- 현재 branch의 이름 변경



## branch 병합

```
$ git merge <branch>
```

- 브랜치를 master에 병합. 보통 현재 작업을 master에 두고 진행
- 현재 작업 공간이 전진하는 구조



### 병합 종류

- Fast Forward : master branch가 브랜치가 커밋해 둔 내용을 그대로 따라감.
- Auto Merge : master와 branch에 서로 다른 commit이 있을 경우, 두개를 합침
- Manual Merge : master와 branch의 변경 사항 사이에 충돌이 있는 경우, 문제 파일을 수정하고 병합함. 이때는 merge가 아닌 commit으로 합침.



## branch 강제 엎어치기

1. 백업

```bash
git branch develop_backup
```

2. 엎어쳐질 대상 branch로 이동

```bash
git checkout develop
```

3. master 브랜치로 엎어치기

```bash
git reset --hard master
```

4. 강제 push 하기

```bash
git push origin develop --force
```



## Commit간 변경된 파일 목록 조회



### Commit 간 비교

```bash
$ git diff [prev commit] [current commit]
$ git diff --name-status HEAD HEAD~1 # 파일명만 비교하고 싶을 경우
$ git diff HEAD HEAD~1 # 만약 내용 전체를 보고 싶을 경우
```

- Commit hash number를 통해서도 비교 가능

```bash
$ git diff [prev commit hash] [current commit hash]
```

  

### Branch 간 비교

- 원격 branch vs Local branch

```bash
$ git diff [remote]/[branch] [local branch]
$ git diff origin/master test1 --name-status
```

- Branch 간 비교

```bash
$ git diff [branch1] [branch2]
$ git diff master main
```

- 다른 파일 간 비교

```bash
$ git diff -- [path1] [path2] #  -- 옵션 뒤에 적은 내용은 항상 경로로 인식
```

- 특정 파일에 대한 변경점 비교

```bash
$ git diff [commit1] [commit2] -- [file]
$ git diff HEAD HEAD~5 -- src/aaa.cpp
```













