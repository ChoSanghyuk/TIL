# :books: Today I Learned



## 구조

- algorithm
- algorithm_records
    - golang
    - python_java
        - swea
        - 기타
        - 백준
        - 프로그래머스
    - 백준
- aws
    - deploy
- blockchain
    - bitcoin
    - go_ether
    - Hyperledger
    - smart_contract
- blog
    - linux_terminal
- book
- crawling
- CS
    - AIDD
    - network
    - TCT
        - Software Application
            - 실습
- DB
    - 강의
    - 실습
- django
- docker
- front-end
- git
- go
    - gorm
- grafana
- graphql
- java
- javascript
- jQuery
- jsp
- kafka
    - code
- kubernetes
- mac
- markdown
- maven
- oracle
- python
- python_project
- rust
- shortcut
- spring
    - JPA
    - spring 강의
        - AOP
        - cns_tech
        - mapping
        - Spring Security
        - URL Shortener
    - SSAFY
    - 추가 학습
- Spring_Cloud_MSA
- SQL
- terminal-command
- vue
- 용어정리
- 정규표현식
- 프로세스모델링





### :memo: 구조 생성 절차

1. 폴더 전체 조회 : ` find . -type d`
2. 트리 구조 생성

| Find                                       | Replace             |
| ------------------------------------------ | ------------------- |
| `^.*\.assets\n`<br />`^.*img\n`<br />`^./` |                     |
| `^[^/]+/`                                  | `\t ` (하나씩 추가) |
| `\t([A-Za-z가-힣])`                        | `\t- $1`            |
| `^([A-Za-z가-힣])`                         | `- $1`              |

3. 불필요 목록 삭제



