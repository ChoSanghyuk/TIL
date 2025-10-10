# SSH (Secure Shell)



## Server

### open ssh 기본 설정

1. open ssh 설치 및 자동 시작

  ```bash
  sudo apt install openssh-server
  sudo systemctl start ssh
  sudo systemctl enable ssh
  ```

  - `start ssh`는 ssh 서비스를 시작하는 명령
  - `enable ssh`는 서버 PC 부팅 시 자동으로 ssh 서비스를 실행하도록 하는 명령

2. 방화벽 세팅

  ```bash
  sudo ufw enable
  sudo ufw allow 22
  sudo ufw status # 방화벽 확인
  ```

  - 22번 포트를 방화벽에서 열어줌

3. 포트 포워딩

   https://generalcoder.tistory.com/29




### ssh key 설정

```bash
ssh-keygen [-t <keygen-algorithm>] [-C <comment>]
```

- 옵션
  - keygen-algorithm
    - Ed25519
    - RSA
  - comment
    -  자체로 인증에 어떤 역할도 하지 않음. user의 구분, 생성 날짜, 생성된 machine 등의 부가적인 정보 전달
- 진행
  1. 키를 저장할 장소: 기본값은 홈의 `.ssh` 디렉터리
  2. 암호(Passphrase): 기본값은 암호 없음 (**ssh key passphrase는 꼭 설정하는 것을 권장**)
- public 키 authorized_keys에 등록

  ```
  cat {pub key} >> ~/.ssh/authorized_keys
  ```

  :bulb: Private key는 pem 파일로 만들어 SSH 서버 접속 시에 사용하고, Public key는 서버의 authorized_keys 파일에 추가해 놓아야 SSH 접속






### SSH 서버 설정 바꾸기

```bash
sudo vim /etc/ssh/sshd_config
```

- `PasswordAuthentification`

  - 비밀번호 인증 설정

  - 비밀번호 인증을 막기 위해 주석 해제 후  `no`로 설정

- `PermitRootLogin`
  - root 로그인 허용 설정
  -  `no` 로 설정
- 기타
  - `MaxAuthTries`: 한 번에 가능한 인증 시도 횟수 입니다. 기본 값은 6번입니다. 4회 이하로 바꾸셔도 좋습니다.
  - `LoginGraceTime`: 이 시간 내 로그인을 하지 못하면 연결을 끊습니다. 기본 값은 120초입니다.
  - `ClientAliveInterval`: 연결한 클라이언트가 명시한 시간동안 아무것도 안 한다면 연결을 끊어줍니다. 기본 값은 없습니다.
  - `X11Forwarding`: 원격으로 GUI 사용이 가능해집니다. 보안을 위해 끄는 게 권장됩니다. 저는 편의성을 위해 켜줬습니다.

#### 서버 설정 변경 후 재시작

```bash
sudo service ssh restart
```

출처 : [:link:](https://chinensis.tistory.com/entry/SSH%EB%A1%9C-%EC%9B%90%EA%B2%A9-%EC%84%9C%EB%B2%84-%EC%95%88%EC%A0%84%ED%95%98%EA%B2%8C-%EC%A0%91%EC%86%8D-%EA%B4%80%EB%A6%AC%ED%95%98%EA%B8%B0)



:bulb: Permission denied (publickey) 문제 발생 시, 서버의 파일 권한이 너무 널널해서 발생하는 것. 아래처럼 조정 필요

```bash
# 홈 디렉토리 소유자 및 권한 확인 및 수정
sudo chown cho:cho /home/cho
chmod 755 /home/cho

# .ssh 디렉토리 및 파일 권한 수정
chmod 700 /home/cho/.ssh
chmod 600 /home/cho/.ssh/authorized_keys
```





## Client

### 접속

```bash
ssh -i {key file}.pem {user}@{instance ip}
```

- 접속 및 실행 : `ssh -i {key file}.pem {user}@{instance ip} "{command}"`
  - 포트 생략 (default 22 포트 사용 시)
  - `-p {port}` 



:bulb: `" "` vs `' '`  

- `" "`  : **Expandable**. Allow **variable expansion**, command substitution (`$(...)`), and escape sequences (`\n`, etc).
- `' '`  : **Literal**. Prevent **all variable expansion**, command substitution, and escape sequences.



:bulb: `$변수` vs `\$변수`

- `$변수` : local shell variables. `""` 안에 로컬에서 정의한 변수를 넣을 때 사용
- `\$변수` : remote shell syntax. When the variable is interpreted remotely, it is needed to be escaped



### 파일 넣기

```bash
scp -i {key file}.pem {local path} {user}@{instance ip}:{instance path}
```

### 파일 가져오기

```bash
scp -i {key file}.pem {user}@{instance ip}:{instance path} {local path}
```



### 편의성 설정

`~/.ssh/config` 파일 수정

```bash
HOST {name}
    HostName {instance ip}
    Port {instance port}
    User {user}
```



## 오류 케이스



### 기존 22 port 접속 서버와 2022 port 접속 서버의 포트 접속 정보 변경

```
It is also possible that a host key has just been changed.
The fingerprint for the ED25519 key sent by the remote host is
SHA256:......
Please contact your system administrator.
Add correct host key in /Users/84455/.ssh/known_hosts to get rid of this message.
```

- ssh client는 `/Users/84455/.ssh/known_hosts` 에 접속한 서버의 host key를 저장해두고, 재접속할 때마다 일치하는 지 비교를 함
- 기존에 접속한 이력이 있던 서버의 키 정보가 바뀌었다면, `known_hosts` 파일에도 똑같이 반영해줘야 함.
- 내 경우에는 기존 20022포트로 접속하던 서버를 22 포트로 접속하는 것으로 변경한 케이스라, 22 포트(default)로 되어있던 정보는 삭제하고 20022 포트를 22로 변경해줌
