# https



## 개요



HTTPS는 SSL 인증서를 통해 데이터를 암호화하여 안전하게 전송



### HTTPS 동작 원리

```
1. 유저가 브라우저에 웹 사이트를 접속
2. 브라우저가 CA 인증서를 검증
3. 서버에 요청을 보냄
4. 서버가 공개 키를 브라우저에 전달
5. 브라우저가 세션을 암호화하여 서버에 전달
6. 서버가 비밀 키로 세션을 복호화
7. 이후 대칭키 방식으로 통신
```

- client(브라우저)는 5번 단계에서 이후 통신에 사용할 대칭키를 생성하고, 이를 암호화하여 서버에 전달함



### SSL

- Secure Sockets Layer

- SSL 인증서는 CA(Certificate Authority)라는 신뢰할 수 있는 기관에서 발급

- SSL 인증서의 구조

  ```
  1. 공개 키
  2. 비밀 키
  3. 인증서 정보
  4. 서명
  ```
  
  - 공개 키 : 데이터를 암호화
  - 비밀 키 : 데이터를 복호화
  - 인증서 : 웹 서버의 신원 정보 포함
  - 서명 : CA가 인증서를 발급한 것을 증명



## 설정 방법

참고 : https://gngsn.tistory.com/82



### 주요 내용

```bash
sudo certbot certonly --manual -d "*.[domain]" -d "[domain]"
```

**`sudo`** - Runs the command with administrator privileges, which Certbot needs to access system directories and perform certificate operations.

**`certbot certonly`** - Tells Certbot to only obtain the certificate without automatically installing it. You'll need to manually configure your web server to use the certificate afterward.

**`--manual`** - Uses manual verification mode, meaning you'll need to manually prove domain ownership during the process. This typically involves:

- Adding specific DNS TXT records that Certbot provides
- Waiting for DNS propagation before continuing

**`-d "\*.[domain]"`** - Requests a wildcard certificate that covers all subdomains of your domain (like `www.example.com`, `mail.example.com`, `api.example.com`, etc.). The asterisk acts as a wildcard placeholder.

**`-d "[domain]"`** - Also includes the root domain itself in the certificate (like `example.com`).

:bulb: [domain]에는 내 도메인 입력.







