# GIT-MERGE



## MEGER LOCALLY



### git command

 1. Fetch and check out the branch for this merge request

    ```bash
    git fetch origin
    git checkout -b "develop" "origin/develop"
    ```

- develop이 merge  소스 브랜치

 2. Review the changes locally
 3. Merge the branch and fix any conflicts that come up

    ```bash
    git fetch origin
    git checkout "origin/master"
    git merge --no-ff "develop"
    ```

- master가 meger의 타겟 브랜치
- ff : fast-forward
 4. Push the result of the merge to Gitlab

    ```bash
    git push origin "master"
    ```



### 충돌 소스 해결-

- `<<<<<<HEAD`
  - 충돌이 시작된 부분
- feature 브랜치에서의 merge request 부분
- `======`
  - merge request한 변경 사항의 끝
- target 브랜치에서 충돌난 최신 변경사항
- `>>>>>>`
  - 충돌 종료 지점
