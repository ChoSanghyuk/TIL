# HYPERLEDGER BESU



## Announcement

:link: https://www.hyperledger.org/blog/2019/08/29/announcing-hyperledger-besu



### Intro

- Hyperledger Besu는 java 기반 Ethereun client로, public blockchain에서도 운영될 수 있는 첫번째 blockchain 프로젝트
- 기업이 그들의 어플리케이션에 permissioned network와 public network를 구축하고자 하는 관심사를 반영
- 모듈로써 설계되었으며, 합의 알고리즘과 다른 중요 blockchain 특징들 간에 관심사의 분리로 구성요소를 쉽게 발전/실행가능하게 함



### Hyperledger Besu 이란 무엇인가

- open source Ethereum client
- Ethereun의 public network , private permissioned network, test network (ex. Rinkeby, Ropsten, and Görli)에서 실행 가능
- PoW, PoA, and IBFT와 같은 합의 알고리즘 포함
  - Proof of Work (mining)
    - 탈중앙화된 합의 메커니즘으로 network 구성원들에게 암호화된 16진수 숫자를 풀도록하게 함
    - allows for secure peer-to-peer transaction processing without needing a trusted third party.
    -  Proof of work is one method that makes it too resource-intensive to overtake the network
  - Proof of Authority
    -  권한있는 노드들이 검증과 블록 생성을 담당함
  - IBFT
    - 이더리움이 PoW에서 PoS로의 네트워크 이전하기 위해 사용되는 합의 알고리즘
    - Proof of Staking
      - 탈중앙화된 합의 메커니즘으로 구성원들은 검증자가 되기 위해 배팅을 하고, 배팅에 따라 확률적으로 선출된 검증자가 검증을 수행
      - 검증을 옳게 수행하고 블록을 추가했을 시 보상을, 잘못된 검증을 행위를 했을 시, 패널티가 부여됨
- 





Besu is designed to be as modular as possible, with a separation of concerns between consensus algorithms and other key blockchain features, making these components easy to upgrade or implement.

