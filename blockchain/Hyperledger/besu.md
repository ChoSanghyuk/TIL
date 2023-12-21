# HYPERLEDGER BESU



## Announcement

:link: https://www.hyperledger.org/blog/2019/08/29/announcing-hyperledger-besu



### Intro

- Hyperledger Besu는 java 기반 Ethereun client로, public blockchain에서도 운영될 수 있는 첫번째 blockchain 프로젝트
- 기업이 그들의 어플리케이션에 permissioned network와 public network를 구축하고자 하는 관심사를 반영
- 모듈로써 설계되었으며, 합의 알고리즘과 다른 중요 blockchain 특징들 간에 관심사의 분리로 구성요소를 쉽게 발전/실행가능하게 함



### Hyperledger Besu

- 개념

  - open source Ethereum client

  - Ethereun의 public network , private permissioned network, test network (ex. Rinkeby, Ropsten, and Görli)에서 실행 가능

  - PoW, PoA, and IBFT와 같은 합의 알고리즘 포함
    - Proof of Work (mining)
      - 탈중앙화된 합의 메커니즘으로 network 구성원들에게 암호화된 16진수 숫자를 풀도록하게 함
        - allows for secure peer-to-peer transaction processing without needing a trusted third party.
        - Proof of work is one method that makes it too resource-intensive to overtake the network


- 특징

  - The Ethereum Virtual Machine

    - Ethereum 블록체인 내 트랜잭션을 통해서 스마트 컨트의 이용과 실행을 하게 해주는 가상머신

  - Consensus Algorithms

    - 트랜잭션 검증, 블록 검증, 블록 생성을 포함하는 다양한 합의 알고리즘을 구현함

      - Proof of Authority
        - permissioned 네트워크에서 참여자들이 서로를 알고 있고, 참여자들간 신뢰의 레벨이 존재할 때 사용됨
        - *IBFT 2.0*
          - 트랜잭션과 블록들은 승인된 검증자(validator)를 통해서 검증됨
          - 검증자들은 돌아가면서 다음 블록을 생성함. 
          - 기존 검증자들은 검증자들의 추가/삭제를 제안하고 투표로 결정
          - 즉각적인 불변성(finality)를 가지며, 분기없이 유효한 블록들만 메인 체인에 포함되어 있음
        - *Clique*
          - IBFT 2.0보다 더 장애-허용적(fault tolerant)임
            - Clique는 50%의 검정자들이 실패하는 것까지 참음
            - IBFT 2.0는 새로운 블록을 만듬에 있어 2/3 이상의 검증자들을 필요로 함
          - 즉각적인 불변성(finality)를 가지지 않으며, 분기 생성 및 체인 개편이 있을 수 있음을 유의

      - Proof of Work
        - mainnet Ethereum에서의 채굴 시스

  - Storage

    - 로컬에 체인 데이터를 저장하기 위해 RocksDBf라는 키-값 DB를 사용
    - 데이터들을 몇개의 카테고리로 분류
      - Blockchain
        - block header : 데이터의 체인을 형성하며, 블록체인의 상태를 암호화적으로 검증하는 데 사용
        - block bodies : 각 블록간 트랜잭션을 정렬하여 기
        - transaction receipts : 트랜잭션 로그와 같이 트랜잭션 실행의 메타 데이터를 포
      - World State
        - 주소(addresses)에서 계좌로의 매핑 
        - 모든 block header들은 stateRoot 해시를 통해 world state를 참조하고 있음
        - 외부적으로 소유된 계좌들은 ether 잔고를 포함하며, 스마트 컨트랙 계좌들은 추가적으로 실행 가능한 코드와 보관소를 가지고 있음



:bulb: PoW, PoA, and IBFT



:bulb: Finality(완결성, 불변성)

- **트랜잭션이 취소나 수정이 될 수 없다**는 것을 뜻함

### Ethereum Client

- Ethereum 프로토콜을 실행하는 소프트웨어
  - Hyperledger Besu도 Ethereum Client의 일종
- 하기 요소들을 포함함
  - Ethereum 블록체인 내의 트랜잭션을 처리하는 실행 환경
  - 트랜잭션 실행과 관련된 데이터를 유지하는 보관소
  - 상태를 동기화하기 위한 네트워크 상의 다른 Ethereum 노드들과 교류하는 P2P 네트워크
  - application 개발자가 블록체인과 교류하기 위한 API







Besu is designed to be as modular as possible, with a separation of concerns between consensus algorithms and other key blockchain features, making these components easy to upgrade or implement.

