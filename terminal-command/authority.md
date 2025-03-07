# 리눅스 권한



## 권한 체계



### 권한 구분

|    -    |  User   |    -    |    -    |  Group  |    -    |    -    |  Other  |    -    |
| :-----: | :-----: | :-----: | :-----: | :-----: | :-----: | :-----: | :-----: | :-----: |
| 읽기(r) | 쓰기(w) | 실행(x) | 읽기(r) | 쓰기(w) | 실행(x) | 읽기(r) | 쓰기(w) | 실행(x) |
|    4    |    2    |    1    |    4    |    2    |    1    |    4    |    2    |    1    |

#### 심볼 표현

- `u` : User(owner)
- `g` : Group
- `o` : Others
- `a` : All
- `+` : Add permission
- `-` : Remove permission
- `=` : set exact permission



### 권한 명령어

- 권한 변경 : `chmod`

  - `chmod [OPTIONS] MODE FILE`
    - `MODE` → Defines permission changes (numeric or symbolic).
    - `FILE` → Target file or directory.

- 소유자 변경 : `chown` 

  - `chown [OPTIONS] USER[:GROUP] FILE`
    - `USER` → New owner of the file.
    - `GROUP` → New group (optional).
    - `FILE` → Target file or directory

  - OPTIONS
    - `-R` : Changes ownership of all files & subdirectories inside `{folder}`
      - `sudo chown -R {user1} {folder}/`

- 권한 확인 : `ls -l {file}`



### 예시

- Give execute permission to everyone

  ```bash
  chmod +x myscript.sh
  ```

- Remove write permission from others

  ```bash
  chmod o-w myfile.txt
  ```

- Give read/write to owner and group, but no access to others

  ```bash
  chmod ug+rw,o-rwx myfile.txt
  ```





## 계정 관리



### 사용자 관리

- 생성 

  - `sudo useradd [options] {user}`

  - options
    - `-m` : Creates a home directory for the user

- 비밀번호 부여

  - `sudo passwd {user}`

- 사용자 변경
  - `su - {user}`
  - 

### 그룹 관리

- 그룹 생성

  - `sudo groupadd {group}`

- 그룹에 사용자 추가

  - `sudo usermod -aG {group} {user}`

  



### sudo 권한 관리

- sudo 사용 금지
  - `sudo deluser {user} sudo`
- 권한 확인
  - `groups {user}`







```
sudo -u username code-server
```







---



1.. user 생성

sudo useradd developer



2.. config 파일 타 계정 권한 제거

chmod o-rx config.yaml























