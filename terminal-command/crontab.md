# 리눅스 크론탭



## 크론탭 기본



### 크론탭 이란

- 특정 시간에 특정 작업이 실행되도록 설정



### 기본 명령어

- `crontab -e`
  - 크론탭 편집 장소로 로딩
  - 편집 후 `:wq`로 갱신
- `crontab -l`
  - 크론탭 내용 출력
- `crontab -r`
  - 크론탭 삭제



## 주기 설정



### 주기 결정

```
*			*		*		*		*
분(0-59)	시간(0-23) 일(1-31) 월(1-12) 요일(0-7)
```

- 요일 
  - 0,7 :일요일
  - 1: 월요일



### 주기 설정 기호

- `*` : every
- `,` : and
- `-` : 범위
- `/` : 주기
  - ex) 분 자리에 */10 : 매 10분 마다





## 사용 팁



### 사용 방법

- 한 줄에 하나의 명령만 작성

```sh
# 잘못된 예
* * * 5 5
/home/script/test.sh
# 잘된 예
* * * 5 5 /home/script/test.sh
```

- 주석

```sh
# 주석 #
#--------------------#
# 이것은 주석입니다. #
#--------------------#
```





## 로깅

- `* * * * * /home/script/test.sh > /home/script/test.sh.log 2>&1`
  - 매분마다  test.sh.log 파일 갱신
- `* * * * * /home/script/test.sh >> /home/script/test.sh.log 2>&1`
  - 매분마다  test.sh.log 파일에 누적





## 백업

- `crontab -l > /home/bak/crontab_bak.txt`
  - 크론탭 내용을 txt 파일로 저장
- `50 23 * * * crontab -l > /home/bak/crontab_bak.txt`
  - 크론탭 백업을 자동화







