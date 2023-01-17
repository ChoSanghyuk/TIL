# GIT-MERGE



## MEGER LOCALLY



### 1. Fetch and check out the branch for this merge request

```bash
git fetch origin
git checkout -b "develop" "origin/develop"
```

- develop이 merge  소스 브랜치



### 2. Review the changes locally



### 3. Merge the branch and fix any conflicts that come up

```bash
git fetch origin
git checkout "origin/master"
git merge --no-ff "develop"
```

- master가 meger의 타겟 브랜치
- ff : fast-forward



### 4. Push the result of the merge to Gitlab

```bash
git push origin "master"
```

