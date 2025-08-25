# Uniswap



## Version별 비교



### 요약 비교

| 버전 | 핵심 변화                               |
| ---- | --------------------------------------- |
| v1   | ETH 기반 단일 페어 AMM                  |
| v2   | ERC-20 ↔ ERC-20 직접 스왑 지원          |
| v3   | 집중 유동성으로 자본 효율성 증가        |
| v4   | Hooks로 풀 커스터마이징 + 수수료 최적화 |



|        Feature         | **v1**                                | **v2**                                  | **v3**                                                  | **v4**                                                       |
| :--------------------: | ------------------------------------- | --------------------------------------- | ------------------------------------------------------- | ------------------------------------------------------------ |
|   **Pool Structure**   | 1 token pair per contract             | 1 token pair per contract               | 1 token pair per contract (with range orders)           | All pools live inside a **Singleton** contract (gas efficient, composable) |
|      Swap Formula      | x * y = k                             | x * y = k                               | x * y = k                                               | x * y = k                                                    |
|   **Fee Management**   | Fixed 0.3% (protocol takes none)      | Fixed 0.3% (with protocol fee switch)   | Multiple fee tiers (0.05%, 0.3%, 1%, etc)               | Fully **dynamic fee** via hooks (programmatic logic per pool) |
| **Capital Efficiency** | Poor (full range only)                | Poor (full range only)                  | **Concentrated Liquidity** (custom price range per LP)  | Same as v3                                                   |
|  ERC20/ERC20 Support   | ETH-only routing (wrapped internally) | True ERC20/ERC20 pairs                  | ERC20/ERC20 pairs                                       | Native ETH support (no WETH wrapping needed)                 |
|     Price Oracles      | No built-in oracle                    | TWAP (Time-Weighted Avg Price) built in | TWAP, improved precision                                | Still available, extendable via hooks                        |
|     Composability      | Low                                   | Medium                                  | Medium                                                  | High (hooks allow full custom logic per pool)                |
|      Flash Swaps       | Not supported                         | Supported                               | Supported                                               | Supported                                                    |
|  **Flash Accounting**  | X                                     | X                                       | X                                                       | all balance updates deferred until end of transaction        |
|  Hooks (Custom Logic)  | X                                     | X                                       | X                                                       | custom code on actions like swap, mint, burn, collect        |
| Deployment Efficiency  | N/A                                   | N/A                                     | Multiple contracts per pool                             | CREATE2 + Singleton = **very cheap pool deployment**         |
| Tokenless Pool Support | X                                     | X                                       | X                                                       | pools can be deployed **before** the tokens are live         |
|    Fee Distribution    | All fees to LPs                       | Optional protocol fee split             | Optional protocol fee split                             | Optional protocol fee split                                  |
|        License         | MIT                                   | GPL                                     | Business Source License (BSL, time-delayed open source) | BSL (expected to become open source after ~4 years)          |



### V4 주요 특징

- Flash Accounting
  - 풀 내부에서 임시로 잔고 상태 추적 후, 트랜잭션 끝에서 정산 → 가스 절약 + 원자성 보장
- Tokenless Pools
  - 토큰 없이도 풀 생성 가능 => 새로운 자산이 유입될 때까지 대기 가능
- Singleton Pool
  - 모든 풀을 하나의 컨트랙트 (Singleton) 안에서 관리 → 가스 비용 절감, 상호작용 간소화



## Contract 분석

### Universal router

- 개요

  - ETH 및 ERC-20 자산을 대상으로 **Uniswap v2 / v3 / v4 전체를 아우르는 통합 스왑 라우터**
  - 컨트랙트는 **무소유(unowned), 업그레이드 불가(non-upgradeable)** 형태로 배포되어 신뢰 최소화를 지향
- 기능
  - 멀티 프로토콜 지원
    - Uniswap **v2 / v3 / v4** 스왑을 단일 트랜잭션 내에서 분할·조합 가능
  - 스왑 분할 & 인터리빙
    - 여러 풀/버전 사이에 주문을 쪼개서 체결하거나 순서를 섞어 실행
  - 부분 체결(Partial fill)
    - 유동성이 부족할 때 가능한 만큼만 체결 후 나머지 처리 로직 구성 가능
  - ETH ↔ WETH 처리
    - 트랜잭션 내에서 자동 **래핑/언래핑** 지원
  - Permit2 통합
    - 서명 기반, **기간 제한 토큰 승인** → 사전 approve 없이 트랜잭션에서 사용 가능
  - 포지션 매니저 연동
    - v3 / v4 포지션 초기화, 유동성 증감, permit 등 고급 관리 동작 실행
  - 서브 플랜 · 잔액 점검
    - 복잡한 실행 흐름을 하위 플랜으로 구성하고 중간 잔액 확인 가능 
- 명령(Command) 기반 아키텍처
  - 여러 커맨드를 **체인으로 연결**해 한 번의 `execute()` 호출에서 복잡한 워크플로우를 표현

- constant

  - MSG_SENDER = `0x0000000000000000000000000000000000000001`

  - ADDRESS_THIS = `0x0000000000000000000000000000000000000002`




### commands (`Commands.sol`)

- `PERMIT2_PERMIT` (`0x0a`)
  - **목적**: `Permit2`를 통해 토큰 사용 권한을 사전 승인 없이 부여
  
  - **동작**: 사용자 서명으로 전달된 `Permit2` 정보를 사용하여 UniversalRouter가 사용자의 토큰을 사용할 수 있게 함
  
  - **장점**: 별도 approve 트랜잭션 없이 **1회성 서명만으로 스왑** 가능
  
-  `V3_SWAP_EXACT_IN` (`0x02`)

  - **목적**: Uniswap V3에서 **정확한 입력 수량을 사용해 최대 출력 수량을 받는 swap** 수행

  - **동작**: 입력 토큰 수량(예: 1 ETH)을 정확히 사용하여 가능한 최대의 출력 토큰(예: DAI)을 받음

  - **경로**: 여러 풀을 거치는 multi-hop 경로 지원

- `PAY_PORTION` (`0x0e`)

  - **목적**: `amountIn` 중 **일부만 특정 주소로 전송** (예: 수수료 또는 제3자 지급)

  - **동작**: 스왑 직전/후 특정 비율만큼을 미리 정한 주소로 송금

  - **활용 예**: affiliate 보상, 수수료 분배 등

- `SWEEP` (`0x09`)

  - **목적**: UniversalRouter가 받은 토큰을 **사용자 지갑으로 옮김**

  - **동작**: 스왑 결과로 받은 토큰이 라우터에 남아 있으므로, 이를 최종 목적지(`msg.sender` 또는 recipient)로 이동시킴

  - **중요**: 이 명령이 없으면 받은 토큰이 라우터에 그대로 남아 있을 수 있음



### methods

- `{bytes 변수}.offset`
  - 기본 메소드
  - The **calldata byte offset** where the actual byte contents of `inputs` begin.

- `{bytes 변수}.toBytes(3);`
  - Uniswap에서 정의한 메소드
  - `_arg`(3) 만큼 이동한 부분에서 데이터를 가져오는 로직



### Permit2

- 개요
  - Uniswap UniversalRouter에서 사용하는 Permit. 일반적 표준 X
- 주요 특징
  - 기간 기반 승인 (timestamp 기반)
    - 특정 날짜까지만 승인하는 구조
  - one transaction으로 approve + transfer까지 지원



## Transaction 분석



### tx meta

- platform : Avalanche
- hash : `0x0ee26793629f51b044c9c95771b240f48a75d823429969e349c740f30401e69a`
- description : 0.5 WAVAX => 8.89 USDC 

- 주요 주소
  - 내 지갑 : `0xb4dd4fb3D4bCED984cce972991fB100488b59223`
  - universal router : `0x94b75331AE8d42C1b61065089B7d48FE14aA73b7`
  - WAVAX : `0xb31f66aa3c1e785363f0875a1b74e27b85fd66c7`
  - USDC : `0xb97ef9ef8734c71904d8002f8b6bc66dd9c48a6e`



### tx data

:bulb: 여기서 나오는 길이는 모두 bytes 길이로 hex에선 *2 해줘야 함

```bash
0x3593564c # execute
0000000000000000000000000000000000000000000000000000000000000060 # bytes   calldata 시작 위치 = 96
00000000000000000000000000000000000000000000000000000000000000a0 # bytes[] inputs 시작 위치 = 160 
00000000000000000000000000000000000000000000000000000000686cbb08 # deadline = 1751956232 (20250708 15:30:32)
# calldata 시작
0000000000000000000000000000000000000000000000000000000000000004 # length of commands = 4
0a00060400000000000000000000000000000000000000000000000000000000 # commands content, padded to 32 bytes
# 0a(PERMIT2_PERMIT) + 00(V3_SWAP_EXACT_IN) + 06(PAY_PORTION) + 04(SWEEP)

# inputs 시작
0000000000000000000000000000000000000000000000000000000000000004 # length of inputs = 4

# inputs들 시작 위치
0000000000000000000000000000000000000000000000000000000000000080 # input 1 위치. 128
0000000000000000000000000000000000000000000000000000000000000200 # input 2 위치. 512
0000000000000000000000000000000000000000000000000000000000000320 # input 3 위치. 800
00000000000000000000000000000000000000000000000000000000000003a0 # input 4 위치. 928

# input 1 - PERMIT2_PERMIT
0000000000000000000000000000000000000000000000000000000000000160 # input 1 길이. 352 (32*11)
000000000000000000000000b31f66aa3c1e785363f0875a1b74e27b85fd66c7 # PermitSingle.PermitDetails.token = WAVAX
000000000000000000000000ffffffffffffffffffffffffffffffffffffffff # PermitSingle.PermitDetails.amount
00000000000000000000000000000000000000000000000000000000689440c6 # PermitSingle.PermitDetails.expiration
0000000000000000000000000000000000000000000000000000000000000000 # PermitSingle.PermitDetails.nonce
00000000000000000000000094b75331ae8d42c1b61065089b7d48fe14aa73b7 # PermitSingle.spender = UniversalRouter
00000000000000000000000000000000000000000000000000000000686cbace # PermitSingle.sigDeadline = 1751956174
# signature The owner's signature over the permit data
00000000000000000000000000000000000000000000000000000000000000e0 
0000000000000000000000000000000000000000000000000000000000000041
efdda88d0d53eae44b4e6eb2aafb98cfa52ca2657741d620b91f3f951f6a1b99
7ea2ebf023289c059ce1b5480b40538a5222c5766d091624d97be396ee4c4d11
1c00000000000000000000000000000000000000000000000000000000000000

# input 2 - V3_SWAP_EXACT_IN
0000000000000000000000000000000000000000000000000000000000000100 # input 2 길이. 256 (32*8)
0000000000000000000000000000000000000000000000000000000000000002 # recipient(address) = address(this)
00000000000000000000000000000000000000000000000006f05b59d3b20000 # amount = 0.5 * 10^18
0000000000000000000000000000000000000000000000000000000000000000 # amountOutMin = 0 
00000000000000000000000000000000000000000000000000000000000000a0 # path 시작 위치 160. (단, 동적부분에서 160)
0000000000000000000000000000000000000000000000000000000000000001 # payerIsUser
000000000000000000000000000000000000000000000000000000000000002b # path 길이 = 43
b31f66aa3c1e785363f0875a1b74e27b85fd66c7 # WAVAX(address)
0001f4 # 500
b97ef9ef8734c71904d8002f8b6bc66dd9c48a6e # USDC(address)
000000000000000000000000000000000000000000

# input 3 - PAY_PORTION
0000000000000000000000000000000000000000000000000000000000000060 # 길이
000000000000000000000000b97ef9ef8734c71904d8002f8b6bc66dd9c48a6e # token(address)
0000000000000000000000007ffc3dbf3b2b50ff3a1d5523bc24bb5043837b14 # recipient(address)
0000000000000000000000000000000000000000000000000000000000000019 # bips(uint256) // fees 0.25%

# input 4 - SWEEP
0000000000000000000000000000000000000000000000000000000000000060
000000000000000000000000b97ef9ef8734c71904d8002f8b6bc66dd9c48a6e # token(address)
000000000000000000000000b4dd4fb3d4bced984cce972991fb100488b59223 # recipient
000000000000000000000000000000000000000000000000000000000086fdea # amountMin
0c # 꼬투리. 없어도 됨
```

