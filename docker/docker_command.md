

- `docker ps -a`
  - Docker에서 현재 실행 중인 모든 컨테이너
  - `-a` or `--all` 옵션으로 정지된 컨테이너 포함 모든 컨테이너 보여줌

- `docker-compose down -v`
  - 정의된 서비스를 중지하고 제거
  - `-v` 옵션은 볼륨 제거

- `docker stop {id}`
  - 지정된 id의 컨테이너 중지

- `docker rm {id}`
  - 지정된 id의 컨테이너 제거

- `docker-compose -f docker-compose.yml up -d`
  - docker compose를 사용하여 서비스 시작
  - 옵션
    - `-f` : compose 파일의 이름을 지정
    - `-d` or `--detach` : 백그라운드 실행

- `docker images`
  - docker에서 현재 사용 가능한 모든 이미지를 보여줌

- `docker logs -f {id}`
  - 지정된 ID의 docker 컨테이너의 로그를 보여줌
  - 옵션
    - `-f` or `--follow` : 로그를 실시간으로 계속 추적하도록 함

- docker save & load
  - **도커 이미지 저장하고 로드, 해당 이미지는 오리지널 이미지와 동일**
  - `docker save -o {file_name.tar} {image_name}`
  - `docker load -i {file_name.tar}`

- docker export & import
  - 도커 컨테이너 저장하고 로드, 오리지널 이미지를 아카이빙 하여 하나의 레이어로 저장된 이미지로 추출
  - 이미지를 작동시키기 위해서는 Dockerfile을 작성하거나, import change 옵션 사용하여 필요한 구문 추가
  - `docker export -o {file_name.tar} {image_name}`
  - `docker import {file_name.tar} {image_name}`