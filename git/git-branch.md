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
$ git -b checkout <브랜치이름>
```

- 브랜치 생성 후 이동



```
$ git switch master (혹은 branch이름)
```

- 작업하려는 곳을 바꿈



## branch 생성

```
$ git branch <브랜치이름>
```

- 로컬 브랜치 생성



```
$ git checkout -t <remote branch 이름>
```

- remote branch와 같은 이름 local branch를 생성하면서 내용 가져옴



```
$ git push origin master:브랜치이름
```

- 해당 이름의 브랜치에 올림 (없으면 생성되고 올려짐)



```
$ git switch -c <브랜치이름>
```

- 브랜치를 생성하면서 작업 공간을 옮김



## branch 삭제

```
$ git branch -d <branchname>
```

- safe delete. unmerged된 상태라면 삭제 X



```
$ git branch -D <branchname>
```

- hard delete. 걍 지워버림



```
$ git push origin :<remote branch>
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
