# mac 터미널



## 실행 파일 설치



### 설치

```bash
brew install python3
```



### 환경변수 설정

1. 사용하는 터미널 타입에 따라서 다른 파일 편집

   - bash : `vi ~/.bash_profile` 

   - zsh : `vi ~/.zshrc`

2. 환경 변수 설정

  ```vi
  export PYTHON_PATH=/usr/local/bin/python
  export PATH=$PYTHON_PATH:$PATH
  ```

3. 반영

   - bash : `source ~/.bash_profile`

   - zsh : `source ~/.zshrc ` 