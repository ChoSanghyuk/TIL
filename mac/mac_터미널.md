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



### bash

- 개요

  - 기본적으로 mac에는 bash가 설치되어 있음
  - 다만, 버전 업데이트가 필요할 경우 재설치 후 경로 지정 필요

- 설치 및 등록

  1. homebrew 설치 (미설치 시)

     ```bash
     /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
     brew --version
     ```

  2. bash 재설치

     ```bash
     brew install bash
     ```

  3. bash 경로 재등록

     - `bash --version`해도 예전 버전일 경우 수행

     - `brew info bash` : bash 설치 경로 확인

     - `/opt/homebrew/bin/bash --version` : 설치한 버전 확인

     - `~/.zshrc` & `~/.bashrc`에 경로 등록

       ```
       export PATH="$HOME/.local/share/solana/install/active_release/bin:$PATH"
       ```

     - `source ~/.zshrc ` 로 반영

















