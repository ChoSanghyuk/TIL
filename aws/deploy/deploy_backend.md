## Backend Deploy



## 준비사항

### Django project

- 완성된 프로젝트
- 의존성 저장 - `pip freeze > requirements.txt`
- 원격저장소 업로드

### AWS (https://aws.amazon.com/ko/)

- AWS 계정 생성
- 기본정보입력
- 카드정보입력 (해외결제가 가능한 체크카드 or 신용카드)
- 휴대폰인증
- 완료후 로그인

### 참고

- vim 명령어
  - `vi filename` : 터미널에서 수정 접근
  - `i` 버튼으로 수정모드로 전환
  - 방향키를 이용하여 이동
  - 수정
  - `esc` 로 수정모드 빠져나오기
  - `:wq` 명령어로 저장 후 종료

------

## Deploy

### cloud9

- AWS Management Console 에서 Cloud9 검색 후 Create environment 클릭

  ![image-20211112095430728](deploy.assets/image-20211112095430728.png)

- 이름입력 후 Next step

![image-20211112100102885](deploy.assets/image-20211112100102885.png)

- 설정
  - Platform - Ubuntu Server 18.04 LTS
  - Cost-saving setting - 일정시간 후 꺼지도록 설정가능 (Never 설정시 과금주의)

![img](deploy.assets/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F0a2a4d00-1b40-484a-9b01-2d48a487814c%2FUntitled.png)

- 생성 완료 후 cloud9 화면 확인

![img](deploy.assets/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F311f75cc-0a66-4e64-b542-1212ee007259%2FUntitled.png)

- 파일트리 설정 (home directory기준으로 진행)
  - Show Environment Root 체크해제
  - Show Home in Favorites 체크

![img](deploy.assets/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fe8318462-63b6-402e-b642-5c8c82db3eef%2FUntitled.png)

- `cd ~` 로 홈 디렉토리에서 작업

### EC2

<aside> 💡 새로운 탭에서 진행 EC2는 cloud9 생성시 자동생성

</aside>

- 서비스 검색

![img](deploy.assets/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F21d80200-c920-4d7a-b437-06d13fdb5b21%2FUntitled.png)

- 보안그룹 탭 이동 후 생성된 보안 그룹 ID 클릭

![img](deploy.assets/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fe8a92138-4c2c-4fab-b617-1f75655677ef%2FUntitled.png)

- 하단 화면의 인바운드 규칙 편집

![img](deploy.assets/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F43df4d4d-4bca-49e9-93b5-8c2ff827f525%2FUntitled.png)

- 규칙 추가 후 저장
  - 포트 범위 - 80, 8000
    - 80 : HTTP 통신 포트
    - 8000 : 테스트용
  - 소스 - `0.0.0.0/0` , `::/0`

![img](deploy.assets/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F4f2b7644-1168-483f-b398-93c2d8859cf2%2FUntitled.png)

### 서버 설정

 💡 이후 내용은 cloud9 터미널에서 진행합니다.

- pyenv 설치 후 터미널 재시작
  - https://github.com/pyenv/pyenv

```bash
git clone <https://github.com/pyenv/pyenv.git> ~/.pyenv
sed -Ei -e '/^([^#]|$)/ {a \\
export PYENV_ROOT="$HOME/.pyenv"
a \\
export PATH="$PYENV_ROOT/bin:$PATH"
a \\
' -e ':a' -e '$!{n;ba};}' ~/.profile
echo 'eval "$(pyenv init --path)"' >>~/.profile

echo 'eval "$(pyenv init -)"' >> ~/.bashrc
```

- pyenv 설치 확인

```bash
pyenv -v

# 출력 확인 => pyenv 2.2.0-7-g663abeb5
```

- python 설치 (프로젝트에서 사용한 버전설치)
  - global 설정 후 버전확인

```bash
pyenv install 3.9.6
pyenv global 3.9.6
python -V
#=> Python 3.9.6
```

### project clone

<aside> 💡 루트폴더와 프로젝트폴더, 두개의 이름에 주의하며 진행해주세요. 두 폴더의 이름을 통일하면 조금더 편하게 설정할 수 있습니다.

</aside>

- clone
  - home을 기준으로 진행

```bash
cd ~
git clone {project_remote_url}
```

![img](deploy.assets/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F4f0d3cb7-da9b-410c-a6a8-dbcbeb4b97e3%2FUntitled.png)

- 폴더구조
  - 프로젝트 이름은 변수처럼 사용예정 이름을 기억해주세요!

```bash
home/
	ubuntu/
		{루트폴더}
			{프로젝트폴더}
			{앱}
			manage.py
			requirements.txt
```

![img](deploy.assets/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F703e4eab-bd98-4ef9-80de-7039388a30c9%2FUntitled.png)

편의를 위해 루트폴더와 프로젝트 폴더의 이름을 통일하였습니다.

- 루트폴더로 이동

```bash
cd ~/{루트폴더}
```

- 가상환경생성 (가상환경이름 기억)

```bash
python -m venv venv
```

- 가상환경 activate (window와 명령어가 다름)

```bash
source venv/bin/activate
```

![img](deploy.assets/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F23f1f0cd-717c-4a76-ba27-ea6897f7de56%2FUntitled.png)

- 라이브러리 설치

```bash
pip install -r requirements.txt
```

- 마이그레이션

```bash
python manage.py migrate
```

- createsuperuser

```bash
python manage.py createsuperuser
```

- loaddata (fixture가 있는경우)

```bash
python manage.py loaddata {data.json}
```

- collectstatic

  - `settings.py` 수정

    ```python
    # settings.py
    
    STATIC_ROOT = BASE_DIR / 'staticfiles'
    ```

  - collectstatic

    ```bash
    python manage.py collectstatic
    ```

### gunicorn

- 설치
  - https://docs.gunicorn.org/en/stable/install.html

```bash
pip install gunicorn
```

- 서버실행

```bash
gunicorn --bind 0.0.0.0:8000 {프로젝트폴더}.wsgi:application
```

- django 페이지 확인

  ![img](deploy.assets/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F98ddf507-a6e3-48c1-8fdc-005e1ffbee92%2FUntitled.png)

- `settings.py` 수정 후 서버 재시작

```python
# settings.py

ALLOWED_HOSTS = [
    '3.36.52.162' # 위의 IP 주소 입력
]
```

- 복사할 코드 작성
  - 아래의 코드에서 각자의 프로젝트이름에 맞게 수정 후 메모장에 입력
  - ExecStart : 해당 경로의 wsgi 실행하라 ( gunicorn 실행 설정 )

```
[Unit]
Description=gunicorn daemon
After=network.target

[Service]
User=ubuntu
Group=www-data
WorkingDirectory=/home/ubuntu/{루트폴더}
ExecStart=/home/ubuntu/{루트폴더}/venv/bin/gunicorn \\
        --workers 3 \\
        --bind 127.0.0.1:8000 \\
        {프로젝트폴더}.wsgi:application

[Install]
WantedBy=multi-user.target	
```

- 파일수정
  - c9는 ~ 보다 상단의 폴더에 대해서는 권한 x => GUI 미제공
  - 위의 복사한 코드를 붙여넣고 저장함

```bash
sudo vi /etc/systemd/system/gunicorn.service
```

- 시스템 데몬 재시작

```bash
sudo systemctl daemon-reload
```

- 서비스 실행 및 등록

```bash
sudo systemctl start gunicorn
sudo systemctl enable gunicorn 
sudo systemctl status gunicorn.service 

# 중지
# sudo systemctl stop gunicorn
# 재시작
# sudo systemctl restart gunicorn
```

- 이후 q 로 나오기

### nginx

<aside> 💡 vim을 사용하여 터미널에서 파일을 수정합니다. 사용법을 숙지하고 진행해주세요.

</aside>

- 설치

```bash
sudo apt-get update
sudo apt-get install -y nginx
```

- 복사할 코드 작성
  - 아래의 코드에서 각자의 프로젝트이름에 맞게 수정 후 메모장에 입력
  - staticfiles의 경우 다른 폴더를 썼다면 이름수정

```
server {
        listen 80;
        server_name {서버IP주소};

        location /static/ {
                root /home/ubuntu/{루트폴더}/staticfiles/;
        }

        location / {
                include proxy_params;
                proxy_pass <http://127.0.0.1:8000>;
        }
}
```

- 파일수정

```bash
sudo vi /etc/nginx/sites-available/django_test
```

- 사이트 추가 (바로가기 만들기)

```bash
sudo ln -s /etc/nginx/sites-available/django_test /etc/nginx/sites-enabled
```

- nginx status, restart

```bash
sudo systemctl restart nginx
systemctl status nginx.service
```

- 80번 포트를 이미 사용중인경우 아래의 명령어 입력 후 

  ```
  nginx
  ```

   재시작

  - `sudo lsof -t -i tcp:80 -s tcp:listen | sudo xargs kill`

![img](deploy.assets/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fbd86c974-93c8-4beb-8737-0871f8eeacdf%2FUntitled.png)

### 배포확인

- EC2 대시보드에서 퍼블릭 IP로 접속

------

## DNS

<aside> 💡 도메인 결제 후 진행합니다.

</aside>

### Route53

- 레코드 생성
  - 레코드 유형 - A
  - 값 - EC2 퍼블릭IP

![img](deploy.assets/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F994006ac-ea06-46dc-8e12-7a5965607200%2FUntitled.png)

- nginx 설정 수정

```
server {
        listen 80;
        server_name {서버IP주소} **{도메인주소}**;

        location /static/ {
                root /home/ubuntu/{루트폴더}/staticfiles/;
        }

        location / {
                include proxy_params;
                proxy_pass <http://127.0.0.1:8000>;
        }
}
```

- settings.py 수정

```bash
ALLOWED_HOSTS = [
    '3.35.230.223',
    'changeo.click'
]
```

- 수정 후 `gunicorn`,  `nginx` 재시작

```bash
sudo systemctl restart gunicorn
sudo systemctl restart nginx
```

------

## HTTPS

📌 https://howhttps.works/ko/

### Let's Encrypt

💡 https://letsencrypt.org/ko/getting-started/
certbot 사용을 권장

### certbot

https://certbot.eff.org/

- Software(nginx), System(Ubuntu) 선택 후 가이드진행

![img](deploy.assets/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fd9fc25e0-f82c-4ad8-9a56-7e5ab8dc517f%2FUntitled.png)

- core 설치 (EC2에 설치되어있음)

```bash
sudo snap install core; sudo snap refresh core
```

- certbot 설치

```bash
sudo snap install --classic certbot
```

- 심볼릭 링크

```bash
sudo ln -s /snap/bin/certbot /usr/bin/certbot
```

- 자동 설정

```bash
sudo certbot --nginx
```

- 이메일 입력

![img](deploy.assets/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fa3666804-5d30-4319-b986-8be52a886fee%2FUntitled.png)

- 동의 (y입력)

![img](deploy.assets/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fe947f6f4-75a1-4a15-a914-0b38f7f22933%2FUntitled.png)

![img](deploy.assets/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F195890b8-92f3-4a9f-a7fc-588e1aa4067e%2FUntitled.png)

- 도메인 선택

  ![img](deploy.assets/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fc43cdcb3-d52f-40db-bfab-12b9f48e70bd%2FUntitled.png)

- EC2 보안그룹 탭 이동 후 생성된 보안 그룹 ID 클릭

![img](deploy.assets/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fe8a92138-4c2c-4fab-b617-1f75655677ef%2FUntitled-163668210364819.png)

- 하단 화면의 인바운드 규칙 편집

![img](deploy.assets/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Faf5b043d-a1e5-4d68-87e7-be89c77f660d%2FUntitled.png)

- 규칙 추가 후 저장
  - 포트 범위 - 443
    - 443 : HTTPS 기본 포트
  - 소스 - `0.0.0.0/0` , `::/0`

![img](deploy.assets/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F9378028a-b796-4726-97d9-dd19e1d9a120%2FUntitled.png)

- `https://` 주소로 요청 후 확인