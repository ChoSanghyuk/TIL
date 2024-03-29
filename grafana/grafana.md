# Grafana



## 설치 및 실행



### 설치

1. ubuntu 실행
2. Grafana 설치

```cmd
sudo apt-get install -y adduser libfontconfig1
wget https://dl.grafana.com/oss/release/grafana_8.2.3_amd64.deb
sudo dpkg -i grafana_8.2.3_amd64.deb
```



### 실행

도커로 실행


```
$ docker run -d -p 3000:3000 grafana/grafana
```





## 개요



### 설명

Grafana is the analytics platform for all your metrics. Grafana allows you to query, visualize, alert on and understand your metrics no matter where they are stored.



### grafana snapshot

- grafana는 version 바뀌는 일이 잦으며, 버전이 바뀔 때 마다 기존의 설정되 datasource들이 깨질 수 있음

- 버전 upgrade 전에 snapshot을 찍어두면, 나중에 오류 발생 시 되돌릴 수 있음

- 버전 up은 한 단계 씩 하는 것이 안전함 (이 경우, 오류 내역 패치 설명에 들어있음)



### Point Doain Name

- ip 주소가 아닌, 개별 지정한 도메인 이름으로 접속 O



## dashboard 관리



### dashboard

- drag and drop 으로 그래프 이동 

- 그래프 크기 조절

- row 추가 하여, row 단위로 그래프 관리 O



### graph 관리

- view

- edit

- share

- explore
  - edit과 유사. panel과 dashboard에 영향을 주지 않으면서 쿼리 vary






### dashboard 버전 관리

- 저장해둔 dashboard들 : 버전으로 남겨짐

- version restore를 통해 이전 버전 이동 O



### data link

- data에 url을 연결할 수 있음
- url은 `${ }` 을 통해 변수와 동적 연결 O



## Edit panel



### Override

- 각각의 query 에 특성 덮어 씌우기

- 고유의 특성을 부여함으로써 query 구별에 효과적



### transform

- 두개 이상의 query의 상호 작용으로 새로운 query 생성
  =======

  db 연결 시, 최소한의 권한 (select)를 가진 grafana 계정을 만들어서 DB 서버와 연결 권장



### sample dashboard 사용

https://grafana.com/grafana/dashboards

연결된 DB 별로 추천 dashboard가 정리되어 있음.

grafana에서 dashboard 생성 시, Import from Granafa.com 에서 sample dashboard에 나와있는 숫자 입력하여 import 가능

>>>>>>> 1e222fbc7e26c05165eec7b0be066e2a5359ae60
