# PBFT



## 배경



### BFT

- Byzantine Fault Tolerance (비잔틴 장애 허용)
- 비잔틴 장군 문제(Byzantine General Problem)를 해결하는 방안
  - 비잔틴 장군 문제 : 네트워크 내에 배신자가 있더라도 합의 내용에 문제가 없으려면 어떻게 해야 하는가 



###FLP Impossibility

- 합의 알고리즘 특성

  - **Safety** : 노드들은 **같은 체인의 상태**를 공유한다. (노드들이 **동일한 블록 또는 상태**에 대해 **서로 충돌하지 않는 결정을 내리는 것**)
  - **Liveness** : 합의 대상(Transaction 또는 블록체인에서 블록)에 문제가 없다면, 네트워크 내에서 반드시 합의가 이루어진다

  => 어떤 합의 알고리즘이 네트워크에서 통용되기 위해선 **Safety**와 **Liveness**라는 특성을 가지고 있어야 함

- 비동기 네트워크 내에서는 Safety와 Liveness를 모두 완벽히 만족하는 합의 알고리즘을 설계하는 것이 불가능하다는 것이 증명

  - 일반적 합의 알고리즘의 경우, safety를 Liveness보다 우선
    - 설령 네트워크 내에 있는 모든 거래를 전부 합의하지는 못하더라도, 일단 합의된 거래는 네트워크 내에 있는 모두에게 동일한 값으로 제공되는 것이 더 중요
  - 비트코인의 경우, Liveness를 safety보다 우선
    - 모든 거래를 가감없이 수용하는 구조를 채택함으로써 Liveness를 극대화
    - 하드포크가 발생할 경우 “최장 길이 체인 선택 알고리즘”을 활용해 Safety 문제를 보완





## PBFT



### PBFT

- 개요

  - Practical BFT
  - Safety를 확보하고 Liveness를 일부 희생하면서, 비동기 네트워크에서도 합의를 이룰 수 있는 알고리즘

  - 네트워크에 배신자 노드가 어느 정도 있다고 해도 네트워크 내에서 이루어지는 합의의 신뢰를 보장하는 알고리즘

- 절차
  1. 클라이언트가 상태 변환을 요청하는 Request 메시지 m을 Primary Node(제일 먼저 요청을 받은 노드)에 전송
  2. Primary가 request 요청을 받으면 네트워크의 나머지 모든 노드에게 Pre-prepare 메시지를 전송
     - `<Pre-prepare, V, N, D(m)>` 
       - V : 메시지가 전송되는 View
       - N : request에 대응하는 Sequential number
       - D(m) : 요청 메시지 m의 요약본
  3. Backup 노드 i가 Pre-prepare 메시지를 받고  검증 결과가 참이라면, Prepare 메시지를 생성해 네트워크의 나머지 모든 노드에게 전송
  4. 각각의 노드는 Pre-prepare 메시지와 Prepare 메시지를 수집합니다. 수집한 Pre-prepare 메시지 개수가 2f+1개이고 Prepare 메시지가 2f개 이상인 경우 `prepared certificate`라 하며, 노드는 `prepared the request` 상태가 됨
  5. prepared certificate 조건을 만족한 노드는 네트워크의 모든 노드에게 Commit 메시지를 전송
  6. 각각의 노드는 Commit 메시지를 수집합니다. Commit 메시지가 2f+1개 모이면 해당 노드는 `commit certificate` 상태

- 특징
  - prepared certificate와 commit certificate 라는 두 번의 절차를 활용하여 배신자 노드가 존재하는 상황에서도 네트워크의 합의를 도출
  - 블록체인으로 이해한다면, 내용상 아무런 문제가 없는 블록이라 해도 2f+1 노드가 인정하지 않으면 체인에 올라갈 수 없다는 점에서 Liveness를 희생한 것입니다.



### Tower BFT

- Solana’s customized version of PBFT

- Solana **adds a cryptographic clock** (using PoH) so nodes already know roughly what order things happened

  => **they don’t need to flood the network with constant votes**.

  



https://steemit.com/consensus/@kblock/48-pbft-consensus-algorithm
