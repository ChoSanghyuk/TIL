# ZK Sync



## 개요

zkSync is a **Layer 2 scaling solution** for Ethereum that uses **Zero-Knowledge (ZK) proofs** to increase transaction throughput while inheriting Ethereum's security.



### How it works in the EVM context

**The core idea:** Instead of executing every transaction on Ethereum mainnet (Layer 1), zkSync batches thousands of transactions off-chain, generates a cryptographic proof that they were all valid, and posts only that proof + compressed data to Ethereum.

**Two key components:**

1. **ZK Rollup** — transactions are bundled together and a validity proof (ZK-SNARK or ZK-STARK) is submitted to L1. Ethereum doesn't re-execute the transactions; it just verifies the proof.
2. **zkEVM** — zkSync Era introduced an EVM-compatible virtual machine. This means Solidity smart contracts can be deployed with little to no modification, unlike earlier ZK systems that required custom languages. (but not 100% EVM equivalent)

:bulb: ZK-SNARK : smaller and faster to verify, but they require a "trusted setup" phase where you have to trust that the initial parameters weren't compromised.

:bulb: ZK-STARK : larger and a bit slower to verify, but they're transparent. they don't need trusted setup (decentralized and secure)

:bulb: zkSync Era : the name of ZKSync's mainnet Layer 2 solution



### Optimistic Rollup vs Zk Rollup 

- Optimistic Rollup 
  - assumes transactions are valid by default and only checks if someone disputes them
  - easier to develop 
- Zk Rollup 
  - uses zero-knowledge proofs to verify every batch upfront. 
  - theoretically more secure



## cross-chain interoperability

### The Core Idea: ZK Proofs as a Trust Layer

Instead of relying on multisig bridges (the hack-prone approach), ZK proofs let chains **cryptographically verify** what happened on another chain — no trust required, just math.



### ZK Stack & Hyperchains

zkSync's ZK Stack lets anyone launch their own **sovereign ZK-powered chain** (a Hyperchain). These chains can:

- Share the same ZK proof system
- Settle to the same or different L1s (Ethereum, or other EVM L1s)
- Communicate natively via **Hyperbridges** without third-party bridges

```
Chain A (Hyperchain) ──┐
                       ├──► ZK Proof Layer ──► Ethereum L1
Chain B (Hyperchain) ──┘
         │
         └──► Native messaging to Chain A (trustless)
```

#### hyperchains

- custom ZK-powered blockchains that run on top of ZKSync.
  -  Instead of everything funneling through one Layer 2, you can spin up your own hyperchain that operates independently but still gets security from Ethereum. 
- Multiple hyperchains can run in parallel and communicate with each other through bridges, which gives you near-infinite scaling without sacrificing decentralization or security.



### Interoperability Patterns

#### 1. Native ZK Bridges (Trustless)

- A ZK proof proves "this transaction happened on Chain A"
- Chain B verifies the proof and executes the corresponding action
- No validators, no multisigs — just cryptographic verification
- **Much safer** than traditional bridges like Ronin or Wormhole (which were hacked)
- 예) Chain B는 Chain A에서 자금이 lock됨을 zk proof를 통해 확인하고, 상응하는 금액을 mint 또는 unlock하는 방식으로 자금 이전

#### 2. Shared Sequencer / Proof Aggregation

- Multiple Hyperchains share a **proof aggregator**
- One combined proof is submitted to L1 for all chains
- Enables **atomic cross-chain transactions** — either both sides execute or neither does

#### 3. Interop with Non-ZK EVM L1s (e.g., BNB Chain, Avalanche, Polygon)

zkSync can interop with other EVM L1s via:

| Method                | How                                         | Trust Level  |
| --------------------- | ------------------------------------------- | ------------ |
| ZK light clients      | Deploy a verifier contract on the target L1 | Trustless    |
| Canonical bridges     | Official bridge contracts on each L1        | Semi-trusted |
| Third-party protocols | LayerZero, Axelar, Wormhole                 | Varies       |
| Shared ZK Stack       | Both chains use ZK Stack                    | Trustless    |



### The Big Picture

```
Ethereum L1
    │
    ├── zkSync Era (ZK Rollup)
    │       ├── Hyperchain A (Gaming)
    │       ├── Hyperchain B (DeFi)
    │       └── Hyperchain C (Enterprise)
    │
BNB Chain ──► ZK Light Client Verifier ──► can verify zkSync state
Avalanche ──► ZK Light Client Verifier ──► can verify zkSync state
```

#### ZK Light Client

- wraps the light client verification logic inside a **ZK circuit**. 
- Instead of verifying raw signatures on-chain, it verifies a cheap ZK proof, saying "1000 block headers are verified and the resulting state root is X" 

:bulb: L1 chain가 다른 체인에서의 일을 알 수 있도록 the ZK proof, stateRoot, blockNumber, claimedEvent 전달