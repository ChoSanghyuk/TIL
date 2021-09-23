# git branch



## branch 보기

```
$ git branch
```



## branch 생성

```
$ git branch <브랜치이름>
```

브랜치 생성

```
$ git switch master (혹은 branch이름)
```

작업하려는 곳을 바꿈

```
$ git push origin master:브랜치이름
```

​	해당 이름의 브랜치에 올림 (없으면 생성되고 올려짐)

```
$ git switch -c <브랜치이름>
```

​	브랜치를 생성하면서 작업 공간을 옮김



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



## branch 삭제

```
$ git branch -d <branchname>
```

