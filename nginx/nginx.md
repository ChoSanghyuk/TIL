# Nginx



## 개요

### 소개

- 웹 서버이자 리버스 프록시, 로드 밸런서, HTTP 캐싱 기능 등을 제공하는 오픈 소스 소프트웨어
- Apache와 함께 가장 널리 사용되는 웹 서버 중 하나로, 가벼우면서 높은 성능을 내는 것이 특징



### 주요 특징

- **고성능 및 고효율**: 이벤트 기반, 비동기 구조로 설계되어, 적은 리소스로 많은 동시 접속 처리가 가능
- **리버스 프록시(Reverse Proxy)**: 요청을 받아 내부 서버로 분산 전달하며, 보안을 강화하고 서버의 부하를 줄임
- **로드 밸런싱(Load Balancing)**: 여러 서버에 요청을 분산시켜, 서비스의 확장성과 안정성을 높임
- **캐싱(Caching)**: 정적인 콘텐츠(이미지, CSS, JavaScript 등)를 빠르게 전달할 수 있도록 캐싱하여 성능 향상에 기여
- **TLS/SSL 지원**: HTTPS 연결을 지원하여 보안성을 제공



:bulb: (포워드) 프록시 vs 리버스 프록시

- (포워드) 프록시
  - 클라이언트 바로 뒤에 놓여 있는 프록시
  - 클라이언트 보안, 캐싱, 암호화
- 리버스 프록시
  - 웹서버/WAS 앞에 놓여 있는 것
  - 서버 보안, 로드밸런싱, 캐싱, 암호화

<img src="./assets/image-20250417090230716.png" alt="image-20250417090230716" style="zoom: 25%;"/>



## Directive



### Directive

- 개요

  - configuration instruction that defines how NGINX behaves
  - nginx consists of modules which are controlled by directives specified in the configuration file

- syntax

  ```
  {directive_name} {directive_value}[...];
  ```

- 종류

  - Simple Directive
    - single line with semicolon
  - Block Directive (Context Blocks)
    - create contexts with curly braces (`{}`)
    - a block organizes directives into logical groups
    - directives outside of any contexts are considered to be in the main context (global context)



### main context

- 개요

  - top-level configuration context that exists outside of any blocks
  - affect the overall operation of NGINX and serves as the parent context for all other contexts

- 종류

  - `user` - Defines which user and group NGINX worker processes run as
    - nginx.conf 파일에 nginx의 계정 설정되어 있음. 주로 `user www-data;` 
  
  - `worker_processes` - Specifies the number of worker processes
  
  - `error_log` - Defines the global error log file
  
  - `pid` - Specifies the file where NGINX stores its process ID
  
  - `events` - A block directive that configures connection processing
  
  - `http` - A block directive that contains HTTP server configuration
  
  - `mail` - A block directive for mail proxy configuration
  
  - `stream` - A block directive for TCP/UDP proxy configuration



### context별 주요 directives

- Server

  - `listen` - Defines IP/port combination for the server

  - `server_name` - Sets names of virtual server (domain names)

  - `root`  vs `alias`

    ```nginx
    location /app/ {
      root /myfolder/; 	# /app/main.dart.js 요청=> /myfolder/app/main.dart.js
    	alias /myfolder/;	# /app/main.dart.js 요청 => /myfolder/main.dart.js
    }
    ```

    - `root` : Nginx가 웹 서버에서 정적 파일을 찾을때의 기본 디렉터리를 설정
    - `alias` : URI 경로를 변경하거나 재정의할 때 사용

  - `index` - Defines default index files (폴더 경로로 요청이 왔을 때 기본 파일 설정)

  - `return` - Returns a specific status code or redirects to URL

  - `rewrite` - Rewrites requested URIs to different URIs

  - `try_files` - 요청 경로와 일치하는 파일이 존재한다면 해당 파일 반환. 없으면 마지막 default 반환

    - `try_files A B ... C;` : 요청 경로가 A 파일과 일치하면 A 반환. 불일치 시 B 확인. 모두 안 맞으면 C 반환

    - `try_files $uri $uri/ /index.html;`
      - Try serving the file that matches the requested URI. If not found, try the directory version. 

      - If that also fails, serve `/index.html` as a fallback.

  - `include` - Includes other configuration files

  - `ssl_certificate` - Path to SSL certificate file

  - `ssl_certificate_key` - Path to SSL private key file

- Location

  - `location` - Defines configuration based on request URI
  - `proxy_pass` - Passes request to proxied server
  - `proxy_set_header` - Redefines or appends fields to the request header
  - `add_header` - Adds custom fields to response header
  - `deny`/`allow` - Controls access based on client IP address
  - `expires` - Sets caching rules for the client browser

- Events

  - `worker_connections` - Maximum connections per worker process

  - `multi_accept` - Allows a worker to accept multiple connections

- http

  - `server` - Defining a backend server
  - `upstream` - Defining groups of backend servers => load balancing



### proxy_pass 사용법

- case1 : `/` 로 끝나는 경우

  ```nginx
  location /pathA/ {
      proxy_pass http://serverA/;
  }
  ```

  - forwards only the **remaining part** to the upstream `serverA`
  - ex) `/pathA/user` ⇒ `http://serverA/user`

- case2 : `/`  없이 끝나는 경우

  ```nginx
  location /pathA/ {
      proxy_pass http://serverA;
  }
  ```

  - forwards the **full path** to the upstream `serverA`
  - ex) `/pathA/user` ⇒ `http://serverA/pathA/user`

:bulb: location 명시 path의 경우, `/pathA` vs `/pathA/` 

- `/pathA` : 정확히 pathA로는 오는 uri만
- `/pathA/` : pathA/로 시작하는 uri 전부



## 폴더 및 설정파일



### 기본적인 폴더 구조

```
/etc/nginx/
├── nginx.conf (main config)
├── sites-available/ (all site configs)
├── sites-enabled/ (symlinks to active sites)
├── conf.d/ (additional configs)
└── snippets/ (reusable config snippets)
```



### main config

```
/etc/nginx/nginx.conf
```

- 해당 conf 파일 안에는 `include /etc/nginx/sites-enabled/*;` 가 설정되어 있어 추가 설정파일들을 반영함 

  ```nginx
  http {
      # Include all enabled sites
      include /etc/nginx/sites-enabled/*;

      # Include additional configs
      include /etc/nginx/conf.d/*.conf;
  }
  ```

- Create snippets for common configs (SSL settings, security headers)

  - `include /etc/nginx/snippets/ssl.conf;`

  :memo: snippet : 재사용 가능한 소스 코드, 기계어, 텍스트의 작은 부분



### 추가 설정 파일

1. `/etc/nginx/sites-available/` 
   - 추가 conf 파일 생성
   - for storing all of your vhost configurations, whether or not they're currently enabled.

2. `/etc/nginx/sites-enabled/`

   - contains symlinks to files in the sites-available folder
   - **allows you to selectively disable vhosts by removing the symlink**

   - symlink 추가 : `sudo ln -s /etc/nginx/sites-available/{file} /etc/nginx/sites-enabled/{file}`

   :memo: symlink : symbolic link. = 바로가기

3. `/etc/nginx/conf.d`
  
   - ~~sites-enabled와 같이 nginx에 반영되는 작업들이 있지만 비활성해야 하는 경우 삭제하거나 변경해야 함~~
   
   - 추가적인 설정 파일
4. `/etc/nginx/sys-available`
  
   - 서버 블록 구성을 저장하는 디렉토리입니다. Nginx는 서버 블록이 사이트 사용 디렉토리에 연결된 경우에만 사용
5. `/etc/nginx/http` 지원
   - 디렉터리에는 이미 활성화된 사이트별 Nginx 서버 블록이 포함되어 있음



### 로그 파일

- `/var/log/nginx/access.log` : 웹 서버에 대한 모든 요청이 기록

- `/var/log/nginx/error.log` : 오류 로그 파일이며 Nginx에서 발생하는 모든 오류를 기록





## 실행



### 명령어

- 실행 : `systemctl start nginx`
- 재실행 : `systemctl restart nginx` or `service nginx restart`
- 테스트 : `nginx -t`

- 중지 : `systemctl stop nginx`
- 부팅 혹은 재부팅 시 nginx 자동 시작 : `systemctl enable nginx`



## 예제



### 기본형

```nginx
server {
    listen 80; # HTTP 기본 포트
    server_name example.com www.example.com;

    location / {
        root /var/www/html;  # 정적 파일이 위치한 디렉토리
        index index.html index.htm;  # 기본 페이지 파일 설정
    }
}
```

- `server_name`
  - Nginx에서 들어오는 요청(request)의 **도메인 이름**(예: [example.com](http://example.com))과 웹 서버의 특정 설정을 연결하는 방법
  - `server_name _;`
    - 기본(default) 설정 (지정된 도메인이 없을 때 모든 요청 처리)



### Basic Authentication 

```nginx
server {
    listen 80;
    
    location / {
        auth_basic "Restricted Content";
        auth_basic_user_file /etc/nginx/.htpasswd;

        root /var/www/html;
        index index.html index.htm;
    }
}
```

```bash
sudo apt install apache2-utils  # if not already installed
sudo htpasswd -c /etc/nginx/.htpasswd usernameb
```



### CORS

- 개요

  - CORS(Cross-Origin Resource Sharing)는 **교차 출처 리소스 공유**를 의미하며, 서로 다른 출처의 웹 페이지나 서버가 자원에 접근할 수 있도록 허용하는 보안 메커니즘
  - HTTP 헤더 기반의 메커니즘으로, 브라우저가 자신의 출처가 아닌 다른 출처의 자원을 로딩할 수 있도록 서버가 허가해 준다. (결국 프론트와 백엔드가 다른 환경에서 떠있어서 발생한 문제)

  :bulb: 같은 기기에 있더라로 포트나 hostname, 프로토콜이 다르면 발생 

- 동작 원리

  - 단순 요청을 보내는것 **(Simple Request)**

  - 예비 요청을 보내서 확인하는것 

    (✈️ Preflight)

    - ex) 크기가 큰 데이터를 담아 보내야하는데 실패하면 안 되니 유효한지 사전에 체크

  - 인증된 요청을 사용하는 방식 **(**🔐 **Credential Request)**

```nginx
server {
    listen 80;
    server_name yourdomain.com;

    location /api/ {
        proxy_pass http://your-backend-ip:your-backend-port;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;

        # Enable CORS
        add_header 'Access-Control-Allow-Origin' '*' always;
        add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS' always;
        add_header 'Access-Control-Allow-Headers' 'Content-Type, Authorization' always;

        # Handle preflight (OPTIONS) requests
        if ($request_method = OPTIONS) {
            add_header 'Access-Control-Allow-Origin' '*';
            add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
            add_header 'Access-Control-Allow-Headers' 'Content-Type, Authorization';
            return 204;  # No Content
        }
    }
}
```





### web server

```nginx
server {
    listen 80;
    server_name your.domain.com;

    # Flutter app
    location /app/ {
        alias /full/path/to/build/web/;
        index index.html;
        try_files $uri $uri/ /index.html;
    }

    # Proxy API calls to backend (e.g., Go on port 8080)
    location /api/ {
        proxy_pass http://127.0.0.1:8080/;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```































