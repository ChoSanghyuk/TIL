# [Solana] 네트워크 동작 요소

## **POH**

### **배경**

- 분산환경에서 빠르게 트랜잭션들을 처리하기 위해서는, 트랜잭션의 정확한 발생 시점을 알고 순서를 세울 수 있어야 한다.
- 이더리움 방식의 비효율
    - 이더리움은 트랜잭션의 시간을 기록하기 위해서 각각의 validator가 가진 Local OS Time에 의존한다.
    - 이더리움은 validator가 기록한 시간이 합리적인 시간대인지 확인 (이전 블록의 기록 시간보다 늦어야 하며, gap이 크지 않아야 함)
    - 이러한 방식은 Timestamp에 대한 검증을 각각의 노드가 병렬적으로 처리할 수 없고, 타 트랜잭션과의 비교를 통해 직렬적으로 처리해야 하는 비효율 발생

### POH 원리

- 기본 원리
    - VDF(verifiable delay function)라는 timestamps 계산 도구가 모드 노드에 내장되게끔 설계
    - VDF는 매 tick마다 이전 POH hash값을 해시 처리하여 새로운 POH hash 결과를 만들어 냄 (SHA256)
        - 따라서 모든 노드는 네트워크에서 독립적으로 POH 연산을 수행하더라도, 모든 tick에 대해 같은 POH 해시 결과를 도출
        - 어떤 노드가 트랜잭션을 받더라도, 누구나 동의할 수 있는 시점을 기록할 수 있음
- 동작 순서
    1. 모든 노드들은 로컬에 동기화된 atomic한 시계(= VDF)를 가지고 있어, 각자가 tick 단위로 동기화된 POH 해시를 생성한다. 
    2. leader 노드는 트랜잭션을 처리한 이후 해당 트랜잭션이 들어온 tick의 POH 해시를 트랜잭션에 각인시킨다
      
        (when a transaction is processed, it is stamped with the current PoH hash)
        
    3. leader가 생성한 블록은 validator node들에게 전파되고, validator node들은 각자가 계산한 POH 해시를 가지고 블록을 검증한다.
- 특징
    - 정보를 작은 단위로 나누어 검증할 수 있으며, 검증을 병렬적으로 처리할 수 있다
    - 타임스탬핑 프로세스의 무결성과 정확성을 보장하여, 신뢰성과 신뢰도를 향상 시킨다
    - VDF는 상당한 연산을 필요로 하는 암호학적인 함수지만, 신속한 검증을 가능하게 한다
    - VDF는 해시 값을 생성할 뿐 정확한 시간(시/분/초 형식)을 알려주진 않는다.

📝 PoH는 합의 알고리즘이 아니다. 다만, 합의 메커니즘을 위한 동기화 로직이다.

### **비교**

- PoW or PoS
    - 노드들은 각자의 시계를 동기화해야 하며, 이같은 시간 소모적인 작업으로 트랜잭션 처리량이 제한된다
- PoH
    - 동기화 문제가 필요없어져, POH 계산의 복잡도에도 불구하고 트랜잭션 처리량을 향상시킨다.

🔗 [https://solana.com/news/proof-of-history](https://solana.com/news/proof-of-history)

🔗 [https://unchainedcrypto.com/solana-proof-of-history/](https://unchainedcrypto.com/solana-proof-of-history/)

## Transaction & Block

### Block

- 생성
    - slot 주기로 생성
        - 1 slot = 64 tick = 400ms (1 tick = 6250μs)
    - slot별 사전에 선출된 leader node가 혼자서 생성
- 블록 크기
    - 128MB
    - 48 million compute units 포함

### Transaction

- 구조
    - `header`: 읽기/쓰기를 구분하는데 사용되는 정보 및 `accountKeys`에 기재된 서명자의 권한(signer privileges)
    - `accountKeys`: 트랜잭션 내 모든 instructions들에 대한 account address 배열
    - `recentBlockhash`: 최근 블록의 해시
        - 트랜잭션에 대한 시간 표시(timestamp)로, 트랜직션이 충분히 최근에 만들어졌는지를 판단하고 중복을 방지
        - 최근 150개 이내의 block의 해시값만 유효함 (트랜잭션은 요청 이후 1분 19초가 지나면 만료)
    - `instructions`: 트랜잭션에 담긴 instruction 배열
        - instruction 안의 `account` 와 `programIdIndex` 필드는 `accountKeys` 를 인덱스로 참조함
    - `signatures`: 서명을 요구하는 instruction에 대한 서명 배열
        - instruction들이 각기 다른 서명자를 요구할 경우, 여러개의 서명이 포함될 수 있음
- Instruction
    - 처리되어야 할 구체적인 하나의 작업으로, Program에 작성된 실행 로직이다. (이더리움의 트랜잭션)
    - 트랜잭션은 여러 개의 instruction을 담을 수 있으며, 트랜잭션은 insturction들의 실행 순서와 원자성을 보장한다.
    - 주요 필드
        - **Program address**: 실행시킬 Program
        - **Accounts**: instruction이 읽거나 쓰는 대상 account들 전부
        - **Instruction Data**: byte array로 포현되는 Program에서 실행시킬 instruction handler 및 파라미터 정보
- 크기
    - 1232 bytes
        - 솔라나 네트워크 내 최대 전송 단위 (MTU, maximum transmission unit)1280 bytes
            - 40 bytes : IPv6 header
            - 8 bytes : fragment header
            - **1232 bytes : transaction packet data**
    - 여기서 각각의 서명이 64 bytes, 주소값이 32 bytes를 소요하기에 사용되는 서명과 계정 수에 따라 잔여 용량 계산 필요
- Gulf Stream 방식의 트랜잭션 전파
    - 트랜잭션은 미리 정해진 leader validator에게 전송되며, 이후 다른 validator들에게 전파된다
    - 모든 노드들은 leader schedule을 알고 있기 때문에 트랜잭션들을 미리 leader validator에게 전송해둘 수 있다
    - mempool-less한 방식으로 대기 시간(latency)을 줄이고 트랜잭션 처리를 최적화한다.

📝 **트랜잭션 예시**

![sol-transfer.svg](./assets/sol-transfer.svg)

### 확정 상태(c**ommitment status)**

- 트랜잭션에 대한 확정 상태를 3가지로 나타낸다
    - Processed
    - Confirmed
    - Finalized

| **Property** | **Processed** | **Confirmed** | **Finalized** |
| --- | --- | --- | --- |
| Received block | X | X | X |
| Block on majority fork | X | X | X |
| Block contains target tx | X | X | X |
| 66%+ stake voted on block | - | X | X |
| 31+ confirmed blocks built atop block | - | - | X |
- confirmed와 finalized 상태 의미 해석
    - confirmed
        - 정식 체인(canonical chain)에 포함된 것을 아직 보장하지 않는다
            - revert 될 수 있음
        - validator들은 fork choice rule에 따라 해당 블록을 빼고 체인을 재구성할 수 있다
    - finalized (rooted)
        - 정식 체인(canonical chain)에 포함된 것을 보장하며, revert 될 수 없다
        - forked 네트워크 또한 finalized된 블록은 포함해야 한다.
- 31-block rule
    - 즉각적인 최종성(Immediate finality)은 실현불가능하기에 solana에서 둔 유예시간이다
    - 네트워크 지연, Byzantine faults 또는 악의적인 validator에 의한 잘못된 블록 생성을 바로 잡을 시간을 validator들에게 부여한다.
    - 대략 12.4초가 소요되며, 이는 타 블록체인에 비해 상당히 빠르게 최종성을 확정짓는다. (이더리움의 경우 수 분 소요)

💡 금융 거래 등 높은 수준의 보장이 필요한 거래의 경우 "confirmed" 수준이 권장된다.

🔗 [https://solana.com/docs/core/transactions](https://solana.com/docs/core/transactions#recent-blockhash)

🔗 [https://docs.anza.xyz/consensus/commitments](https://docs.anza.xyz/consensus/commitments)

🔗[https://chainstack.com/solana-transaction-commitment-levels/](https://chainstack.com/solana-transaction-commitment-levels/)

## Node

### **Validator Node**

- 합의 노드들로 알려진 노드들이다.
- 다음의 역할들을 수행한다
    1. 블록의 유효성 검사
    2. 트랜잭션 최종성(finality) 투표
    3. 리더로 선출
- 위임지분증명 (delegated Proof-of-Stake protocol)
    - Solana는  delegated Proof-of-Stake protocol를 사용한다
    - SOL를 가진 노드는 누구나 validator에게 위임할 수 있다
    - validator은 네트워크에 대한 영향력을 획득하여 더 많은 슬롯의 리더로 지정되고 투표에 대한 더 많은 보상을 얻을 수 있다

💡 **DPoS**

토큰을 가진 사람들의 ‘투표’를 통해 득표순으로 정해진 숫자(주로 20~100개)의 검증자를 선출하고, 선출된 검증자가 토큰 홀더들을 대변해 블록을 생성하고 검증할 수 있도록 권한을 위임

검증인 숫자를 줄여 속도와 효율성을 높였지만 공격에 취약해지고 탈중앙성이 약해짐

### **Leader Node**

- 개요
    - validator node 중에서 블록을 생성하도록 당선된 노드
    - 리더는 연속적인 4개 블록에 대대한 생성을 맡는다
    - 모든 노드들은 리더 스케쥴을 사전에 알고 있다
- 선출
    - epoch라는 주기별로 leader들을 미리 선출한다.
        - 1 epoch = 432,000 slots (대략 2–3 days)
    - 리더는 지분 가중치(stake-weight)에 따라 랜덤하게 선출된다

### **RPC node**

- 블록 투표에 참여하지 않는다 (클러스터 합의에 참여하지 않음)
- RPC 제공자는 트랜잭션을 현재 및 다음 리더(검증자 노드) 모두에게 전송함
- 사용자가 Solana 네트워크의 노드에 요청을 보내고 데이터를 받을 수 있도록 쉽게 접근할 수 있도록 지원한다

🔗 [https://www.alchemy.com/overviews/solana-nodes](https://www.alchemy.com/overviews/solana-nodes)

🔗 [https://m.upbitcare.com/academy/advice/386](https://m.upbitcare.com/academy/advice/386)

🔗 [https://medium.com/@dattgoswami/understanding-transaction-inclusion-in-solana-from-wallets-to-validators-9e412ae792b3](https://medium.com/@dattgoswami/understanding-transaction-inclusion-in-solana-from-wallets-to-validators-9e412ae792b3)

🔗 [https://www.helius.dev/blog/consensus-on-solana](https://www.helius.dev/blog/consensus-on-solana)

## **SVM - Sealevel Virtual Machine**

### SVM

- 개요
    - Solana의 병렬처리 smart contracts runtime
    - Solana의 트랜잭션은 해당 트랜잭션이 실행중에 읽거나 혹은 쓸 것인지에 대한 모든 상태를 가지고 있다
    - 서로 덮어쓰지 않는 트랜잭션들과 읽기 트랜잭션은 동시에 병렬적으로 처리된다
- 비교 (vs EVM)
    - EVM (& EOS’s WASM-based runtimes) : single threaded
        - 한번에 하나의 컨트랙트만이 블록체인 상태를 변경한다
    - SVM
        - 한 번에 병렬적으로 트랜잭션을 처리한다
- Transaction 최적화
    1. 각각의 instruction VM에게 읽거나 쓰고자 할 account를 말한다
    2. 읽거나 쓸 모든 메모리 정보가 미리 kernel에 전달된다
    3. 운영 체제가 사전 로드(prefetch)하고, 장비를 준비시키며, 동시에 작업을 실행할 수 있도록 한다

### SVM multi thread

- 1 Thread : validator은 **Prio-Graph**라는 알고리즘을 돌리는 스케줄러 사용
    - 트랜잭션의 우선순위 수수료(priority fee)를 기준으로 우선순위를 결정한다
    - 트랜잭션간 충돌 여부를 확인한다
- 2 Threads : vote transactions 처리
    - 블록 내 트랜잭션의 약 절반이 투표 트랜잭션이며, 네트워크 동기화를 유지하기 위해 우선적으로 처리됨
- 4 Threads : non-vote transations 처리

🔗 [https://solana.com/ko/news/sealevel---parallel-processing-thousands-of-smart-contracts](https://solana.com/ko/news/sealevel---parallel-processing-thousands-of-smart-contracts)

## [추가] account와 program

### **Cloudbreak 구조**

- 개요
    - = accounts database
    - = mapping of Public Keys to Accounts
- Accounts
    - maintain balances and data
        - data is a vector of bytes
    - have an “owner” field = ProgramId
- Program
    - governs the state transitions for the account
    - are code and have no state
    - rely on the data vector in the Accounts

### **Program Authority**

- Program
    1. Programs can only change the data of accounts they own.
    2. Programs can only debit accounts they own.
    3. Any program can credit any account.
    4. Any program can read any account.
- System Program
    1. System Program is the only program that can assign account ownership.
        - 📝 By default, all accounts start as owned by the System Program
    2. System Program is the only program that can allocate zero-initialized data.
    3. Assignment of account ownership can only occur once in the lifetime of an account.
- Loader Program
    - 개요
        - A user-defined program is loaded by the loader program
        - able to mark the data in the accounts as executable
    - 동작 순서
        1. Create a new public key.
        2. Transfer coin to the key.
        3. Tell System Program to allocate memory.
        4. Tell System Program to assign the account to the Loader.
        5. Upload the bytecode into the memory in pieces.
        6. Tell Loader program to mark the memory as executable.

💡 The key insight here is that programs are code, and within our key-value store, there **exists some subset of keys that the program and only that program has write access.**

## Fee 종류

Transaction Fees : 트랜잭션을 전송하는데 드는 기본 값으로, 서명당 5K lamports로 값이 고정됨

Prioritization Fees : 트랜잭션을 우선적으로 처리하기 위한 추가적인 비용으로 optional함. 

Rent : data를 Solana Blockchian 상에 유지하기 위해서는 쓰는 비용

[https://solana.com/docs/core/fees](https://solana.com/docs/core/fees)