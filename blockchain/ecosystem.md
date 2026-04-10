# 블록체인 Ecosystem

[toc]

## 네트워크



### 솔라나

- Pos + PoH 합의 알고리즘으로 이더리움보다 더 빠른 처리 속도

  ![image-20241110191630437](./assets/image-20241110191630437.png)

- based on the **Berkeley Packet Filter (BPF)**, an efficient virtual machine optimized for high-performance and **parallel execution**.

- BPF is compatible with Rust and C

:bulb: 하지만 이미 이더리움이 Defi와 NFT등의 영역에서 압도적인 시장 점유자



### tron

- TRON is a public blockchain dedicated to providing the underlying infrastructure that enables developers to **create smart contracts and decentralized applications**, freely publish, own, and store data & other content.
- TRON deploys a **Delegated Proof-of-Stake (DPoS) consensus mechanism** to avoid the issue of low transaction throughput times and high transaction fees amongst Proof-of-Work (PoW) public blockchains (e.g., Bitcoin).
- 거래소간 송금 시 사용됨
- why is it fast
  - DPoS
    - **27 Super Representatives (SRs)** are elected
    - prioritizes **speed over decentralization**.
  - TVM
    - Tron Virtual Machine (TVM) is optimized for speed.



### Cosmos

- 소개

  - described as "the internet of blockchains.
  - a decentralized network of independent, scalable, and interoperable blockchains

- Key Components

  - Cosmos SDK
    - A modular framework that allows developers to build custom blockchains easily.
  - IBC Protocol

- Cosmos Hub

  - the first blockchain network built within the cosmos ecosystem

  - coin : `ATOM token`

  - acts as a central hub that connects to other chains, but it's just one participant in the broader ecosystem



### Chainlink

- 소개
  - a decentralized oracle network (DON)
  - solves a core limitation of blockchain technology: blockchains are deterministic systems that can track their own ledger state but are inherently unable to access external data
- Chainlink Runtime Environment (CRE)
  - an orchestration layer that enables institutional-grade smart contracts
  - complex financial workflows that operate across blockchains and off-chain systems, addressing a key pain point of integrating existing systems with blockchains.

- 전통 금융과의 결합
  - Connecting legacy systems to blockchains (the Runtime Environment)






### Base

- Coinbase가 개발한 L2 블록체인 (Optimism 기반).
- Ethereum과 호환되며, Coinbase 생태계와 통합.



### Polygon

- L2 블록체인

- Ethereum의 확장 솔루션을 제공하는 프로젝트.
- PoS 체인, zkEVM, CDK 등 다양한 L2 기술 보유.



### Arbitrum

- L2 블록체인

- (Optimistic Rollup 방식).
- 빠르고 저렴한 트랜잭션, Ethereum 메인넷 보안 유지.



### BNB Chain

- **BNB Beacon Chain(관리 및 거버넌스)** + **BNB Smart Chain(BSC)**로 구성
  - **BSC (Binance Smart Chain)**
    - 바이낸스에서 개발한 EVM 호환 블록체인.
    - 빠른 트랜잭션 처리와 저렴한 수수료가 장점.

- 바이낸스 생태계 전반을 포괄하며, DeFi, GameFi, SocialFi 등 다양한 프로젝트 포함



### 카이아

- 카카오 + 라인 통합 네트워크
- SNS 통합된 dApp 서비스 제공



## 블록체인 서비스



### Alchemy

- a blockchain development platform
  - not a blockchain itself
  - think of it as a cloud service for blockchain, offering essential services that reduce the complexity of decentralized application (dApp) development. 
- provides the tools, infrastructure, and APIs that developers use to build, manage, and scale applications on various blockchains
  - simplifying complex processes like node access and data retrieval. 



### Cosmos

- 개념

  - **Cosmos** is actually an **ecosystem** or **framework** that enables many independent blockchain networks to exist and communicate with each other. 

  - Think of it less as a single network and more as a collection of tools and protocols that allow separate blockchains to connect.

- 체인

  - Cosmos Hub : the first blockchain network built within the cosmos ecosystem.
  - **Osmosis** (decentralized exchange), **Juno** (smart contract platform) , **Akash** (decentralized cloud computing) etc...
    - independent chains can communicate through **IBC (Inter-Blockchain Communication)** protocol. 
    - They're not all dependent on the Cosmos Hub - they can connect directly to each other peer-to-peer, creating a web of interconnected blockchains rather than a hub-and-spoke model.

:bulb: hub-and-spoke : 하나의 hub가 존재하고, spokes(개별 노드들)은 hub로만 연결되어, 중앙 hub를 통해서만 통신



### Euclid Protocol

- unifies liquidity across the web3 ecosystem without relying on traditional bridging

- 핵심 기능

  1. **Unified Liquidity Pools (Virtual Settlement Layer):**
     - Liquidity from all chains are available as a single resource, reducing slippage and maximizing asset utilization. 
     - The Virtual Settlement Layer (VSL) is the core of the Euclid system, responsible for for managing liquidity, optimizing asset utilization, and ensuring instant finality. 
     - Security measures guard against vulnerabilities like double-spending and 51% attacks.

  2. **Cross-Chain Messaging (EMP, Euclid Messaging Protocol):**
     - simplifies cross-chain asset access. 
     - EMP is a cross-chain system enabling communication between pools and their Virtual Pool counterparts, allowing the Euclid layer to track activity across chains. 
     - EMP is built on top of *IBC*(Inter Blockchain Communication), but goes beyond it — extending support to ecosystems such as Ethereum and Solana.















