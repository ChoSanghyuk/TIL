# How Optimistic Rollup Logic Works

## Overview

This document provides a comprehensive explanation of **Optimistic Rollup** technology, including its core mechanisms, transaction flow, fraud proof system, and security model. Optimistic rollups are a Layer 2 scaling solution for Ethereum that enable fast and cheap transactions while maintaining the security guarantees of the base layer.

---

## Table of Contents

1. :star:[Core Philosophy: The "Optimistic" Assumption](#core-philosophy-the-optimistic-assumption)
2. :star: [Complete Transaction Flow](#complete-transaction-flow)
3. :star: [The Three Transaction States](#the-three-transaction-states)
4. [Fraud Proof Mechanism](#fraud-proof-mechanism)
5. [Economic Security Model](#economic-security-model)
6. [Complete Architecture](#complete-architecture)
7. :star:[Optimistic vs ZK Rollups](#optimistic-vs-zk-rollups)
8. [Advantages and Disadvantages](#advantages-and-disadvantages)
9. [Real-World Implementations](#real-world-implementations)
10. [Security Assumptions](#security-assumptions)

---

## Core Philosophy: The "Optimistic" Assumption

### What Does "Optimistic" Mean?

Optimistic rollups are called **"optimistic"** because they assume all transactions are **valid by default** and don't immediately prove their validity. Instead, they rely on a **fraud-proving scheme** to detect and correct invalid transactions after the fact.

**Key Principle**: *"Trust, but verify"*
- Transactions are accepted as valid unless someone proves they're fraudulent
- No need to prove correctness upfront
- Fraud detection happens reactively, not proactively

### Why This Approach?

**Efficiency Gains:**
- Don't waste resources proving valid transactions (99.9%+ of cases)
- Only compute proofs when fraud is suspected
- Significantly cheaper than generating validity proofs for every batch

**Trade-off:**
- Speed and cost (optimistic) vs Immediate certainty (ZK rollups)
- Delayed finality vs Instant finality
- Economic security vs Cryptographic security

---

## Complete Transaction Flow

### Overview Diagram

```
┌─────────┐
│  User   │
└────┬────┘
     │ ① Submit Transaction
     ↓
┌────────────┐
│ Sequencer  │ ② Orders & Executes
└─────┬──────┘
      │
      ↓
┌────────────┐
│  Batcher   │ ③ Compresses & Submits to L1
└─────┬──────┘
      │
      ↓
┌────────────┐
│  Proposer  │ ④ Posts State Root
└─────┬──────┘
      │
      ↓
┌──────────────────┐
│ Ethereum L1      │ ⑤ Challenge Period (7 days)
└──────────────────┘
```

---

### Step 1: User Submits Transaction

**User Action:**
```
User → Signs Transaction → Submits to Sequencer
```

**Transaction Details:**
- User creates a transaction (e.g., token transfer, smart contract call, NFT mint)
- Transaction is signed with user's private key
- Transaction is sent to the **Sequencer** (L2 node operator)

**Example Transaction:**

```json
{
  "from": "0xAlice",
  "to": "0xBob",
  "value": "1.5 ETH",
  "nonce": 42,
  "gasLimit": 21000,
  "signature": "0x..."
}
```

---

### Step 2: Sequencer Processes Transactions

**Sequencer's Role:**

The sequencer is the **central coordinator** of the L2 network (though moving toward decentralization).

**Responsibilities:**
1. **Receives** L2 transactions from users
2. **Orders** transactions (determines execution order)
3. **Executes** transactions using the execution layer (EVM)
4. **Creates** L2 blocks containing these transactions
5. **Updates** the local L2 state

**Sequencer Batches:**

The sequencer groups multiple transactions into batches:

```
Sequencer Batch = {
  epoch_number: 12345,      // Reference to L1 block
  l2_timestamp: 1640000000, // L2 block timestamp
  transactions: [
    tx1, tx2, tx3, ..., txN
  ]
}
```

**Why Batching?**
- **Efficiency**: Amortize L1 costs across many transactions
- **Performance**: Process hundreds of transactions in one L2 block
- **Cost Reduction**: One L1 submission for many L2 transactions

**Processing Flow:**
```
Tx Pool → Sequencer → Execution Engine (EVM) → State Update
```

**Example:**
- Input: 1000 user transactions
- Output: 1 sequencer batch with updated state
- L2 Block Time: ~2 seconds (post-Bedrock)

---

### Step 3: Batcher Submits Data to L1

**Batcher's Role:**

The batcher is responsible for **data availability** - ensuring all transaction data is on Ethereum L1.

**Process:**

1. **Aggregates** multiple sequencer batches into channels
2. **Compresses** data for efficient L1 storage (save gas costs)
3. **Submits** transaction data to Ethereum L1

**Channel Structure:**

```
Channel = Compressed(
  Sequencer Batch 1 +
  Sequencer Batch 2 +
  ... +
  Sequencer Batch N
)
```

**Benefits of Channels:**
- **Better compression ratio**: More data per compression frame
- **Cost optimization**: Larger chunks compress more efficiently
- **Flexibility**: Can split into multiple L1 transactions if needed

**Submission to L1:**

```
Batcher → Compress Batches → Post to Ethereum as Calldata/Blobs
```

- Posted as one or more transactions to Ethereum
- Transaction data becomes part of Ethereum's data availability layer
- Anyone can download this data and reconstruct L2 state
- Critical for security: enables fraud proof verification

**Data Availability:**
- All transaction data is publicly available on L1
- Any verifier can download and replay transactions
- Ensures censorship resistance and verifiability



**:bulb: State Root Proposals → Smart Contract Storage (MUTABLE)**

Rollup 되는 state는 묵여서 L1의 블록에 저장되는 것이 아닌, 전용 스마트 컨트랙트에 저장됨

- Stored in L2OutputOracle contract state
- Can be DELETED during challenge period
- Temporary until finalized



---

### Step 4: Proposer Commits State Roots

**Proposer's Role:**

The proposer creates **checkpoints** of the L2 state on L1.

**Process:**

1. **Executes** the L2 blocks locally (re-execution)
2. **Computes** new state root (cryptographic commitment to L2 state)
3. **Submits** state root to L2 Output Oracle contract on L1

**State Root Explanation:**

A **state root** is a Merkle root that represents the entire L2 state:

```
State Root = MerkleRoot(
  All Account Balances +
  All Contract Storage +
  All Code +
  All Nonces
)
```

**Properties:**
- **Compact**: Single 32-byte hash represents entire state
- **Cryptographic**: Any change produces completely different hash
- **Verifiable**: Can prove any account/storage value with Merkle proof

**Submission to L1:**

```solidity
// Simplified L2OutputOracle contract
contract L2OutputOracle {
    struct OutputProposal {
        bytes32 outputRoot;      // State root
        uint128 timestamp;       // L2 timestamp
        uint128 l2BlockNumber;   // L2 block height
    }

    function proposeL2Output(
        bytes32 _outputRoot,
        uint256 _l2BlockNumber,
        bytes32 _l1BlockHash,
        uint256 _l1BlockNumber
    ) external {
        // Proposer submits state root
        // Starts challenge period
    }
}
```

**Why State Roots?**
- Enable verifiable withdrawals from L2 to L1
- Provide checkpoints for fraud proof challenges
- Allow users to prove their L2 balance on L1

---

## The Three Transaction States

L2 transactions progress through three states as they move toward finality:

### State Progression

```
Unsafe → Safe → Finalized
  ↓       ↓         ↓
 L2     L1 DA    Challenge
 Only   Posted    Period
                  Passed
```

### Detailed States

| State | Description | Reversible? | Time |
|-------|-------------|-------------|------|
| **Unsafe** | Processed by sequencer but not on L1 yet | ✅ Yes | Immediate |
| **Safe** | Transaction data posted to L1 | ⚠️ Theoretically (via fraud proof) | ~Minutes |
| **Finalized** | Challenge period passed, state root accepted | ❌ No (irreversible) | ~7 days |

### 1. Unsafe State

**Characteristics:**
- Sequencer has processed the transaction
- Transaction is in sequencer's local database
- NOT yet submitted to L1
- Fastest confirmation (2 seconds)

**Risks:**
- Sequencer could crash (data loss)
- Sequencer could censor (transaction dropped)
- Sequencer could reorder (different execution)

**Use Cases:**
- UI updates (show pending transaction)
- Low-value transactions where speed matters
- Accepting risk for better UX

### 2. Safe State

**Characteristics:**
- Transaction data posted to Ethereum L1
- Data is now publicly available and censorship-resistant
- Can reconstruct L2 state from L1 data
- Higher confidence in finality

**Risks:**
- State root could be challenged and proven fraudulent
- Unlikely but theoretically possible
- Would require successful fraud proof

**Use Cases:**
- Medium-value transactions
- Most DeFi operations
- Standard user expectations

### 3. Finalized State

**Characteristics:**
- Challenge period (7 days) has passed
- State root is accepted as valid
- No further challenges possible
- Equivalent to L1 finality

**Security:**
- Same security as Ethereum L1
- Cryptographically secured
- Irreversible

**Use Cases:**
- Large withdrawals to L1
- High-value settlements
- Bridge operations
- Critical financial operations

---

## Fraud Proof Mechanism

This is the **core security mechanism** of optimistic rollups.

### Challenge Period

**Duration**: Typically **7 days** (1 week)

**During this time:**
- Anyone can challenge a proposed state root
- State root is NOT considered final
- Withdrawals to L1 are blocked (waiting for finality)
- Verifiers monitor for fraudulent transactions

**Why 7 Days?**
- Give verifiers time to detect fraud
- Allow for dispute resolution process
- Balance security vs capital efficiency
- Ensure at least one honest verifier is online

---

### How Fraud Proofs Work

#### Phase 1: Detection

**Verifier (Challenger) Monitoring:**

```
┌──────────────────────────────────────┐
│ Verifier Node                        │
│                                      │
│ 1. Download tx data from L1          │
│ 2. Re-execute all transactions       │
│ 3. Compute expected state root       │
│ 4. Compare with proposed state root  │
│                                      │
│ IF roots don't match → FRAUD!       │
└──────────────────────────────────────┘
```

**Detection Process:**
```
Verifier monitors L2 state roots posted to L1
  ↓
Verifier independently re-executes transactions
  ↓
Verifier computes: Expected State Root
  ↓
Verifier compares: Expected vs Proposed
  ↓
IF Expected ≠ Proposed → Fraud detected!
  ↓
Verifier identifies the fraudulent transaction
```

---

#### Phase 2: Challenge Submission

**Challenger Initiates Dispute:**

```solidity
// Simplified fraud proof contract
contract FraudProofSystem {
    function challengeStateRoot(
        bytes32 _outputRoot,      // Disputed state root
        uint256 _outputIndex,     // Which checkpoint
        uint256 _l2BlockNumber,   // L2 block with fraud
        bytes calldata _proof     // Initial evidence
    ) external payable {
        require(msg.value >= CHALLENGE_BOND, "Insufficient bond");
        // Initiate dispute game
    }
}
```

**Challenge Components:**
- **Target**: Specific state root being challenged
- **Evidence**: Points to fraudulent transaction
- **Bond**: Challenger stakes ETH (economic commitment)
- **Claim**: "This state root is incorrect"

---

#### Phase 3: Interactive Proving Game (Binary Search)

This is the most sophisticated part of the fraud proof system.

**Goal**: Narrow down the dispute to a **single computational step** that Ethereum L1 can verify.

**Binary Search Process:**

```
Initial: Full batch with 1024 transactions
         [Tx1, Tx2, Tx3, ... Tx1024]

         Challenger: "State root is wrong"
         Sequencer: "No, it's correct"

         Contract: "Which half is disputed?"

Round 1: Split in half
         Left:  [Tx1...Tx512]
         Right: [Tx513...Tx1024]

         Challenger: "Right half is wrong"
         Sequencer: "Right half is correct"

         → Continue with [Tx513...Tx1024]

Round 2: Split disputed half
         Left:  [Tx513...Tx768]
         Right: [Tx769...Tx1024]

         Challenger: "Left half is wrong"

         → Continue with [Tx513...Tx768]

Round 3: Continue splitting
         [Tx641...Tx704]

Round 4: [Tx673...Tx688]

Round 5: [Tx681...Tx684]

...

Final Round: Single transaction, single opcode
         Transaction #682, opcode #47

         → Ethereum L1 executes this ONE step
         → Determines who was correct
```

**Why Binary Search?**
- **Efficiency**: O(log n) rounds instead of O(n)
- **Gas Cost**: Only verify one step on L1
- **Scalability**: Works for arbitrarily large batches

**Interactive Game:**
```
Round   | Disputed Range | Actions
--------|----------------|---------------------------
Start   | 1024 txs       | Both parties commit
1       | 512 txs        | Narrow to half
2       | 256 txs        | Narrow to half
3       | 128 txs        | Narrow to half
4       | 64 txs         | Narrow to half
5       | 32 txs         | Narrow to half
6       | 16 txs         | Narrow to half
7       | 8 txs          | Narrow to half
8       | 4 txs          | Narrow to half
9       | 2 txs          | Narrow to half
10      | 1 tx           | Identify specific tx
11-15   | Opcodes        | Narrow to single opcode
Final   | 1 opcode       | L1 executes and judges
```

**Number of Rounds:**
- For 2^20 (~1 million) operations: ~20 rounds
- For 2^30 (~1 billion) operations: ~30 rounds
- Logarithmic scaling is key to efficiency

---

#### Phase 4: On-Chain Execution

**Final Verification:**

Once dispute is narrowed to a single computational step:

```
Ethereum L1 Contract:
  ↓
Executes disputed opcode with exact state
  ↓
Computes result
  ↓
Compares: Challenger's claim vs Sequencer's claim
  ↓
Determines winner
```

**Example:**

```
Disputed Operation: SSTORE (storage write)

Initial State: storage[0x123] = 0x456
Opcode: SSTORE(0x123, 0x789)
Expected Result: storage[0x123] = 0x789

Sequencer claimed: storage[0x123] = 0xABC ← WRONG
Challenger claimed: storage[0x123] = 0x789 ← CORRECT

L1 executes: SSTORE(0x123, 0x789)
Result: storage[0x123] = 0x789

Winner: Challenger ✓
```

---

#### Phase 5: Resolution & Penalties

**If Fraud Proof Succeeds (Challenger Wins):**

```
✅ Invalid state root is REJECTED
✅ Rollup protocol RE-EXECUTES transactions correctly
✅ Rollup state is UPDATED to correct value
✅ Dishonest sequencer's BOND IS SLASHED (penalty)
✅ Challenger receives REWARD from slashed bond
✅ User funds are SAFE
```

**Economic Outcome:**
```
Sequencer Bond: -100 ETH (slashed)
Challenger Bond: +100 ETH (returned)
Challenger Reward: +50 ETH (from slashed bond)
Protocol Penalty Fund: +50 ETH (remaining slashed)

Net: Honest party profits, dishonest party loses
```

**If Fraud Proof Fails (Sequencer Wins):**

```
❌ Challenge is DISMISSED
❌ Original state root STANDS
❌ Challenger's bond is SLASHED
❌ Sequencer receives REWARD from slashed bond
```

**Economic Outcome:**
```
Challenger Bond: -50 ETH (slashed)
Sequencer Reward: +25 ETH (from slashed bond)
Protocol Fund: +25 ETH (remaining slashed)

Net: Frivolous challenges are punished
```

---

### Fraud Proof Security Properties

**Key Properties:**

1. **Single Honest Verifier Assumption**
   - Only need ONE honest verifier monitoring
   - If fraud exists, honest verifier will catch it
   - Economic incentives ensure participation

2. **Permissionless Challenges**
   - Anyone can become a verifier
   - No special permissions required
   - Open participation increases security

3. **Cryptographic Correctness**
   - Single step execution is deterministic
   - L1 arbitration is trustless
   - No room for ambiguity

4. **Economic Finality**
   - After 7 days, challenge window closes
   - Cost of fraud > benefit
   - Rational actors won't attempt

---

## Economic Security Model

### Validator Bonds

**Bond Requirement:**

Validators (sequencers) must post a **bond** before producing blocks.

```
Sequencer Registration:
  ↓
Lock Bond (e.g., 100 ETH)
  ↓
Can now produce L2 blocks
  ↓
If honest: Keep bond + earn fees
If dishonest: Bond slashed
```

**Purpose:**
- **Economic incentive** to act honestly
- **Skin in the game** - real money at risk
- **Deterrent** - makes fraud unprofitable
- Similar to Proof-of-Stake staking

**Bond Sizing:**
```
Bond Amount Should Be:
  > Expected Gain from Fraud
  > Challenge Cost
  < Validator's Capital Availability
```

---

### Economic Guarantees

**Cost-Benefit Analysis for Attackers:**

```
┌─────────────────────────────────────────┐
│ Potential Gain from Fraud              │
│   Max steal from smart contract: $10M  │
└─────────────────────────────────────────┘
              vs
┌─────────────────────────────────────────┐
│ Cost if Caught                          │
│   Lost bond: $50M                       │
│   Reputation damage: Priceless          │
│   Lost future sequencer revenue: $5M/yr │
│   Legal consequences: Possible          │
└─────────────────────────────────────────┘

Rational Decision: DON'T COMMIT FRAUD
```

**Security Condition:**
```
For security to hold:
  Bond Value + Lost Revenue + Reputation Cost >> Potential Gain

If: $50M + $100M + ∞ >> $10M
Then: Rational actors won't attempt fraud ✓
```

---

### Incentive Structure

**For Sequencers:**

| Action | Outcome | Economic Result |
|--------|---------|-----------------|
| **Act Honestly** | Blocks accepted, earn fees | + Transaction Fees + MEV - Operating Costs |
| **Commit Fraud** | Caught by verifier, slashed | - Full Bond - Reputation - Future Revenue |
| **Offline/Bug** | Miss blocks, no slash (if unintentional) | - Opportunity Cost |

**For Verifiers (Challengers):**

| Action | Outcome | Economic Result |
|--------|---------|-----------------|
| **Monitor Honestly** | Catch fraud, win challenge | + Slashed Bond (50%) + Reputation |
| **False Challenge** | Lose dispute, slashed | - Challenge Bond |
| **Ignore Fraud** | Fraud succeeds | 0 (but harms network) |

**For Users:**

| Action | Outcome | Economic Result |
|--------|---------|-----------------|
| **Use L2** | Fast, cheap transactions | + Savings vs L1 |
| **Wait for Finality** | Withdraw after 7 days | Guaranteed security |
| **Fast Withdrawal** | Use liquidity provider | + Speed - Small Fee |

---

### Game Theory

**Nash Equilibrium:**

In a properly designed system:
- **Sequencers** maximize profit by acting honestly
- **Verifiers** maximize profit by monitoring and challenging fraud
- **Users** benefit from secure, cheap transactions

**Attack Scenarios:**

1. **Sequencer Attempts Fraud**
   - Expected Value: -$40M (bond loss exceeds gain)
   - Outcome: Irrational, won't attempt

2. **Colluding Sequencer + Verifiers**
   - Challenge: Need to bribe ALL potential verifiers
   - Cost: Unbounded (permissionless participation)
   - Outcome: Economically infeasible

3. **Network-Wide Collusion**
   - Challenge: Coordinate all participants
   - Defense: Users can exit to L1, social consensus
   - Outcome: Kills network value, self-defeating

---

## Complete Architecture

### System Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────────┐
│                    Ethereum L1 (Base Layer)                          │
│                                                                      │
│  ┌──────────────────────┐         ┌──────────────────────────────┐ │
│  │  L2 Output Oracle    │         │  Transaction Data Storage    │ │
│  │  (State Roots)       │         │  (Compressed Batches)        │ │
│  │                      │         │                              │ │
│  │  - Stores checkpoints│         │  - Calldata / Blobs          │ │
│  │  - 7-day challenge   │         │  - Data Availability         │ │
│  │  - Fraud proof logic │         │  - Public & Verifiable       │ │
│  └──────────┬───────────┘         └──────────┬───────────────────┘ │
│             │                                 │                     │
│             │ ③ State Root                    │ ② Tx Data           │
│             │ Commitment                      │ Submission          │
│             │                                 │                     │
└─────────────┼─────────────────────────────────┼─────────────────────┘
              │                                 │
              │                                 │
┌─────────────┴─────────────────────────────────┴─────────────────────┐
│              Optimistic Rollup (L2 Network)                          │
│                                                                      │
│  ┌──────────┐    ┌──────────┐    ┌────────────┐                    │
│  │Sequencer │───→│ Batcher  │───→│ Proposer   │                    │
│  │          │    │          │    │            │                    │
│  │- Orders  │    │-Compress │    │-Commits    │                    │
│  │- Executes│    │-Aggregate│    │-State Roots│                    │
│  └────┬─────┘    └──────────┘    └────────────┘                    │
│       │                                                              │
│       │ ① Executes Transactions                                     │
│       ↓                                                              │
│  ┌─────────────────────────────────────────┐                        │
│  │      Execution Layer (EVM)              │                        │
│  │                                         │                        │
│  │  - Process Transactions                 │                        │
│  │  - Execute Smart Contracts              │                        │
│  │  - Update State                         │                        │
│  │  - Implementation: revm, Geth, etc.     │                        │
│  └─────────────────────────────────────────┘                        │
│                        │                                             │
│                        ↓                                             │
│  ┌───────────────────────────────────────────────────┐              │
│  │           L2 State Database                       │              │
│  │                                                   │              │
│  │  - Account Balances                               │              │
│  │  - Contract Storage                               │              │
│  │  - Contract Code                                  │              │
│  │  - Nonces                                         │              │
│  └───────────────────────────────────────────────────┘              │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
            ↑
            │ Submit Transactions
            │
       ┌────┴────┐
       │  Users  │
       │         │
       └─────────┘

                    ⚠️ Challenge Period (7 Days) ⚠️
                              │
                              ↓
              ┌───────────────────────────────┐
              │   Verifiers / Challengers     │
              │                               │
              │  - Download tx data from L1   │
              │  - Re-execute transactions    │
              │  - Monitor state roots        │
              │  - Submit fraud proofs        │
              │  - Earn rewards for catching  │
              │    fraud                      │
              └───────────────────────────────┘
```

---

### Component Interactions

```
Time: T0 (Transaction Submission)
  User → Sequencer: Submit tx

Time: T0 + 2s (Execution)
  Sequencer: Execute tx, update state
  State: UNSAFE

Time: T0 + 5min (Batching)
  Batcher: Compress and submit to L1
  State: SAFE (data on L1)

Time: T0 + 30min (Proposal)
  Proposer: Submit state root to L1
  Challenge Period: STARTS (7 days)

Time: T0 + 7 days (Finality)
  No successful challenges
  State: FINALIZED
  Withdrawals: NOW POSSIBLE
```

---

## Optimistic vs ZK Rollups

### Core Difference: Proof Mechanism

The fundamental difference between optimistic and ZK rollups lies in **when and how** they prove validity.

### Comparison Table

| Aspect | Optimistic Rollups | ZK Rollups |
|--------|-------------------|------------|
| **Assumption** | Transactions valid by default | Transactions invalid until proven |
| **Proof Type** | Fraud proofs (reactive) | Validity proofs (proactive) |
| **Proof Timing** | Only when challenged | Every batch |
| **Proof Generation** | Rare (only disputes) | Always (every batch) |
| **Challenge Period** | 7 days | None (immediately valid) |
| **Finality Time** | 7+ days | Minutes to hours |
| **Withdrawal Time** | 7+ days | Near-instant |
| **Computation Cost** | Low (only when challenged) | High (cryptographic proofs) |
| **Gas per Batch** | ~40,000 gas | ~500,000 gas |
| **Security Model** | Economic incentives + vigilance | Cryptographic certainty |
| **Trust Requirement** | ≥1 honest verifier | None (math-based) |
| **Complexity** | Simpler to implement | Complex (ZK circuits) |
| **EVM Compatibility** | Excellent (EVM equivalent) | Challenging (zkEVM complexity) |
| **Development Maturity** | Production-ready | Rapidly maturing |
| **Examples** | Optimism, Base, Arbitrum | zkSync, StarkNet, Polygon zkEVM |

---

### Detailed Comparison

#### Fraud Proofs vs Validity Proofs

**Fraud Proofs (Optimistic):**
```
Assume Valid → Post to L1 → Wait 7 days → If no challenge → Finalized
                                      ↓
                               IF challenged → Dispute → On-chain verification
```

**Benefits:**
- No computation needed for honest transactions (99.9%+ of cases)
- Cheaper per batch
- Simpler implementation

**Drawbacks:**
- Long withdrawal period
- Relies on economic security
- Requires active monitoring

**Validity Proofs (ZK):**
```
Compute Proof → Verify Proof → Post to L1 → Immediately Finalized
```

**Benefits:**
- Instant finality (no challenge period)
- Cryptographic security (mathematical certainty)
- Faster withdrawals
- Higher security guarantees

**Drawbacks:**
- Expensive proof generation
- Complex implementation (ZK circuits)
- Limited EVM compatibility (improving)
- Higher gas costs per batch

---

### When to Choose Which?

**Choose Optimistic Rollups:**
- ✅ Need full EVM equivalence
- ✅ Want simpler implementation
- ✅ Lower operational costs priority
- ✅ 7-day finality is acceptable
- ✅ Target: DeFi, NFTs, general-purpose apps

**Choose ZK Rollups:**
- ✅ Need instant finality
- ✅ Fast withdrawals critical
- ✅ Highest security priority
- ✅ Willing to pay for proof generation
- ✅ Target: Payments, high-frequency trading, privacy apps

---

## Advantages and Disadvantages

### Advantages of Optimistic Rollups

#### 1. Lower Operational Costs

**Cost Structure:**
```
Per Batch:
  Optimistic Rollup: ~40,000 gas
  ZK Rollup: ~500,000 gas

  Savings: 92% cheaper per batch
```

**Why Cheaper?**
- No need to generate expensive validity proofs for every batch
- Fraud proofs only computed when needed (rarely, if ever)
- Simpler on-chain verification logic

#### 2. Full EVM Equivalence

**Compatibility:**
- Can run standard Ethereum smart contracts with minimal/no changes
- Full support for existing Solidity code
- Compatible with all Ethereum tooling (Hardhat, Foundry, Remix)
- Easier developer onboarding (familiar environment)

**Example:**
```solidity
// Exact same code works on L1 and optimistic L2
contract MyDeFiProtocol {
    function swap(address tokenIn, address tokenOut, uint256 amount) external {
        // Standard ERC-20 operations
        // Works identically on Optimism, Base, Arbitrum
    }
}
```

#### 3. Simplicity

**Technical Advantages:**
- Less complex than zero-knowledge cryptography
- No need for specialized ZK circuits
- Easier to audit and maintain
- Faster development iteration
- Lower barrier to entry for developers

**Maintenance:**
- Simpler debugging (standard EVM)
- Easier to reason about security
- More developers understand the system

#### 4. Proven Technology

**Battle-Tested:**
- Optimism: Running since December 2021
- Arbitrum: Launched September 2021
- Base: Launched August 2023
- Billions in TVL secured across all chains

**Track Record:**
- No successful fraud attacks to date
- Stable operation for years
- Growing ecosystem and adoption

#### 5. Network Effects

**Ecosystem Benefits:**
- Large developer community
- Extensive tooling and infrastructure
- Multiple production implementations
- Shared learnings and best practices

---

### Disadvantages of Optimistic Rollups

#### 1. Long Withdrawal Times

**7-Day Challenge Period:**
```
User initiates withdrawal
  ↓
Wait 7 days (challenge period)
  ↓
Funds arrive on L1

Total: ~1 week delay
```

**Impact:**
- Poor user experience for L2→L1 withdrawals
- Capital inefficiency (funds locked)
- Liquidity fragmentation

**Mitigation:**
- **Fast withdrawal services**: Liquidity providers front funds for a fee
- **Third-party bridges**: Trade speed for small fee (~0.1-0.5%)
- **Example**: User withdraws instantly, LP waits 7 days, user pays 0.3%

#### 2. Less Secure Than ZK Rollups (Theoretically)

**Security Model Comparison:**

**Optimistic Rollups:**
- Rely on **economic incentives** (game theory)
- Require **at least one honest verifier** monitoring
- **Game-theoretic security** (rational actors won't fraud)
- Trust in economic rationality

**ZK Rollups:**
- Rely on **cryptographic proofs** (mathematics)
- **Mathematically guaranteed** correctness
- No trust assumptions
- Pure cryptographic security

**Practical Reality:**
- Optimistic rollups have excellent security in practice
- No successful fraud to date
- Economic incentives work well
- But theoretical ceiling is lower than ZK

#### 3. Capital Inefficiency

**Locked Capital:**

**Sequencer Bonds:**
- Must lock large amounts of ETH
- Reduces capital efficiency
- Opportunity cost

**User Funds During Withdrawal:**
- Funds locked for 7 days
- Can't be used on L1 or L2
- Affects liquidity

**Impact:**
- Higher capital requirements for operators
- Affects overall system efficiency
- Can impact yield opportunities

#### 4. Requires Active Monitoring

**Verifier Network Needed:**
- System security depends on verifiers watching
- Must have infrastructure to detect fraud
- Ongoing operational costs

**Centralization Risk:**
- If no one monitors, fraud could succeed
- Requires healthy verifier ecosystem
- Economic incentives must be sufficient

**Mitigation:**
- Multiple independent verifiers (Optimism Foundation, Coinbase, community)
- Open-source tooling for running verifiers
- Economic rewards for catching fraud

---

## Real-World Implementations

### 1. Optimism (OP Mainnet)

**Overview:**
- **Launch**: December 16, 2021
- **Developer**: Optimism Collective / OP Labs
- **TVL**: Billions (major DeFi ecosystem)
- **Architecture**: OP Stack

**Technical Specs:**
```
Sequencer: OP Labs operated (moving to decentralization)
Challenge Period: 7 days
Fraud Proof System: Interactive proving with binary search
Block Time: 2 seconds (post-Bedrock)
Execution Client: op-geth (primary), supports alternatives
Consensus: Inherited from Ethereum via OP Stack
```

**Features:**
- Full EVM equivalence
- OP token for governance
- Retroactive public goods funding
- Superchain vision (interoperable L2s)

**Use Cases:**
- DeFi protocols (Synthetix, Velodrome, etc.)
- NFT platforms
- DAO tooling
- General-purpose applications

---

### 2. Base (Coinbase)

**Overview:**
- **Launch**: August 9, 2023
- **Developer**: Coinbase
- **TVL**: Largest OP Stack L2 by TVL
- **Architecture**: OP Stack + Reth execution

**Technical Specs:**
```
Sequencer: Coinbase operated
Challenge Period: 7 days (same as Optimism)
Fraud Proof System: OP Stack (inherited)
Block Time: 2 seconds, sub-second with Flashblocks
Execution Client: op-rbuilder (Reth-based)
Special Feature: Flashblocks (sub-second block building)
```

**Key Innovations:**
- **Reth Integration**: Uses revm for execution (high performance)
- **Flashblocks**: Sub-second confirmations for better UX
- **Coinbase Integration**: Seamless fiat on/off ramps
- **No Native Token**: Uses ETH only (simplicity)

**Use Cases:**
- Consumer crypto applications
- Social platforms (Farcaster)
- Gaming
- NFTs
- High-frequency trading (low latency)

---

### 3. Arbitrum

**Overview:**
- **Launch**: September 2021
- **Developer**: Offchain Labs
- **TVL**: Largest optimistic rollup by TVL
- **Architecture**: Arbitrum Nitro

**Technical Specs:**
```
Sequencer: Offchain Labs operated
Challenge Period: ~7 days
Fraud Proof System: Multi-round interactive proving
Execution: WASM-based (Arbitrum Nitro)
Block Time: ~250ms
Unique: AnyTrust chains for higher throughput
```

**Features:**
- WASM-based execution (not just EVM)
- Multi-round fraud proofs (more efficient disputes)
- Stylus (Rust smart contracts)
- Orbit chains (L3s on top of Arbitrum)

**Use Cases:**
- DeFi (largest ecosystem)
- GMX (derivatives)
- Gaming (Treasure DAO)
- NFTs

---

### Comparison of Implementations

| Feature | Optimism | Base | Arbitrum |
|---------|----------|------|----------|
| **Launch Date** | Dec 2021 | Aug 2023 | Sep 2021 |
| **TVL Ranking** | #2-3 | #1 (OP Stack) | #1 (overall) |
| **Execution** | op-geth | op-reth (revm) | WASM (Nitro) |
| **Block Time** | 2s | 2s (sub-second) | 250ms |
| **Native Token** | OP | None (ETH) | ARB |
| **Unique Feature** | Governance | Flashblocks | Stylus (Rust) |
| **Developer** | Collective | Coinbase | Offchain Labs |
| **Focus** | Infrastructure | Consumer apps | DeFi |

---

## Security Assumptions

For Optimistic Rollups to be secure, several assumptions must hold:

### 1. At Least ONE Honest Verifier

**Requirement:**
```
Security Holds IF:
  ∃ at least 1 honest verifier monitoring the network

Security Fails IF:
  All verifiers are offline OR colluding
```

**Why This Works:**
- Single honest party can catch fraud
- Economic incentives encourage verification
- Open participation (permissionless)
- Low cost to run verifier
- Rewards for catching fraud

**Verifier Economics:**
```
Cost to Run Verifier:
  - Server costs: ~$100-500/month
  - Development: Open-source tools available
  - Maintenance: Minimal

Potential Reward:
  - Successful fraud proof: $1M+ (portion of slashed bond)
  - Reputation: Priceless

Net: Highly profitable if fraud occurs
```

**Redundancy in Practice:**
- Multiple independent verifiers (Optimism Foundation, exchanges, community)
- Anyone can run a verifier node
- Low barriers to entry
- Strong economic incentives

---

### 2. Data Availability Guarantee

**Requirement:**
```
All transaction data MUST be posted to L1

Users/Verifiers can:
  - Download all tx data
  - Reconstruct L2 state
  - Verify state roots independently
```

**Why Critical:**
- Enables fraud proof verification
- Ensures censorship resistance
- Allows anyone to exit to L1
- Prevents data withholding attacks

**Data Availability Methods:**

**Ethereum Calldata (Current):**
```
Pros:
  - Guaranteed availability (Ethereum security)
  - Immutable and persistent
  - Verifiable by anyone

Cons:
  - Expensive (~16 gas per byte)
  - Limits throughput
```

**EIP-4844 Blobs (Future):**
```
Pros:
  - Much cheaper (~1 gas per byte)
  - Higher throughput
  - Still Ethereum-secured

Cons:
  - Temporary storage (prune after 30 days)
  - Need archival for history
```

**Security Implication:**
```
IF data is available:
  → Anyone can verify state
  → Fraud proofs work
  → Security holds ✓

IF data is withheld:
  → Can't verify state
  → Fraud proofs impossible
  → Security fails ✗
```

---

### 3. Economic Rationality

**Assumption:**
Participants act to maximize economic benefit

**Sequencer Rationality:**
```
Honest Behavior:
  Revenue: Transaction fees + MEV
  Risk: None
  Long-term: Continued operation

Fraudulent Behavior:
  Gain: Steal X from smart contract
  Cost: Lose bond (Y >> X)
  Risk: Criminal prosecution
  Long-term: Expelled, reputation destroyed

Rational Choice: Stay honest ✓
```

**Verifier Rationality:**
```
Monitor Network:
  Cost: Low (server costs)
  Potential Reward: High (if fraud found)

Ignore Network:
  Gain: Nothing
  Cost: Miss reward opportunity

Rational Choice: Monitor ✓
```

**User Rationality:**
```
Wait for Finality:
  Security: Maximum
  Cost: 7 day delay

Fast Withdrawal (LP):
  Security: High (LP takes risk)
  Cost: 0.3% fee

Rational Choice: Depends on amount and urgency
```

---

### 4. L1 Security

**Foundation:**
Optimistic rollups inherit Ethereum's security

**Dependency:**
```
IF Ethereum is secure:
  → L1 data is immutable
  → L1 contracts are secure
  → L1 consensus is final
  → L2 security holds ✓

IF Ethereum is compromised:
  → L2 security also compromised ✗
```

**Implications:**
- L2 security ceiling = L1 security
- Cannot be more secure than base layer
- Ethereum's validator set protects L2
- 51% attack on Ethereum affects L2

---

### 5. Smart Contract Security

**Critical Contracts:**

**L2OutputOracle:**
- Stores state root proposals
- Manages challenge period
- Handles withdrawals

**L1CrossDomainMessenger:**
- Bridges messages L1↔L2
- Processes withdrawals
- Critical for fund security

**Fraud Proof Contracts:**
- Dispute resolution logic
- Binary search game implementation
- On-chain verification

**Risk:**
```
IF contract has bug:
  → Funds could be stolen
  → Fraud proofs could fail
  → System security compromised

Mitigation:
  - Extensive audits (multiple firms)
  - Formal verification
  - Bug bounties
  - Gradual rollout
  - Emergency pause mechanisms
```

---

## Conclusion

### Summary of Optimistic Rollup Logic

Optimistic rollups provide Ethereum scaling through a elegant "trust-but-verify" mechanism:

**Core Principles:**
1. **Optimistic Assumption**: Accept all transactions as valid by default
2. **Fraud Detection**: Community monitors for invalid state
3. **Challenge Mechanism**: Interactive proving game resolves disputes efficiently
4. **Economic Security**: Bonds and penalties ensure honest behavior
5. **L1 Settlement**: Ethereum provides final arbitration and security

---

### Key Takeaways

**What Makes It Work:**
- ✅ **Scalability**: 10-100x cheaper than L1, fewer proofs than ZK
- ✅ **Security**: Inherits Ethereum's security + economic guarantees
- ✅ **Compatibility**: Full EVM equivalence, easy migration
- ✅ **Simplicity**: Easier to understand, implement, and audit than ZK
- ⚠️ **Trade-off**: Slower finality (7 days) vs ZK's instant finality

**Security Model:**
```
Cryptographic Security (L1)
  +
Economic Security (Bonds + Game Theory)
  +
Community Vigilance (Verifiers)
  =
Practical Security (Proven in production)
```

---

### When to Use Optimistic Rollups

**Ideal For:**
- General-purpose applications (DeFi, NFTs, DAOs)
- Applications needing full EVM compatibility
- Cost-sensitive applications
- Teams wanting simpler implementation
- Established use cases with existing Solidity code

**Consider Alternatives (ZK) For:**
- Payment systems (need instant finality)
- High-frequency trading (need fast withdrawals)
- Privacy applications (ZK proofs enable privacy)
- Applications willing to pay for maximum security

---

### The Future

**Evolution of Optimistic Rollups:**
- **Decentralization**: Moving from single sequencer to decentralized networks
- **Interoperability**: Superchain vision (shared liquidity across L2s)
- **Performance**: Reth and modern execution layers improving throughput
- **User Experience**: Fast withdrawal services, better finality UX
- **Cost Reduction**: EIP-4844 blobs reducing data costs significantly

**Long-Term Outlook:**
- Optimistic and ZK rollups will likely coexist
- Different trade-offs suit different applications
- Continued innovation in both directions
- Multi-rollup future with specialized chains

---

## References

### Technical Documentation
- [Optimistic Rollups - Ethereum.org](https://ethereum.org/developers/docs/scaling/optimistic-rollups/)
- [Transaction Flow - Optimism Docs](https://docs.optimism.io/op-stack/transactions/transaction-flow)
- [Rollup Protocol Overview - Optimism](https://docs.optimism.io/stack/rollup/overview)
- [OP Stack Protocol Specification](https://specs.optimism.io/protocol/overview.html)

### Educational Resources
- [How Do Optimistic Rollups Work - Alchemy](https://www.alchemy.com/overviews/optimistic-rollups)
- [What Are Optimistic Rollups - Nervos](https://www.nervos.org/knowledge-base/What_are_optimistic_rollups_(explainCKBot))
- [Optimistic Rollups Explained - MoonPay](https://www.moonpay.com/learn/blockchain/what-are-optimistic-rollups)

### Fraud Proofs & Security
- [Full Guide to Fraud Proofs and Validity Proofs - Cyfrin](https://www.cyfrin.io/blog/a-full-comparison-what-are-fraud-proofs-and-validity-proofs)
- [What is Fraud Proof - Cube Exchange](https://www.cube.exchange/what-is/fraud-proof)

### Comparisons
- [Optimistic vs ZK Rollups - Cyfrin](https://www.cyfrin.io/blog/what-are-blockchain-rollups-a-full-guide-to-zk-and-optimistic-rollups)
- [ZK Rollups vs Optimistic Rollups - StarkWare](https://starkware.co/blog/zk-rollups-explained/zk-rollups-vs-optimistic-rollups/)
- [Optimistic vs ZK Rollups - Coinbase](https://www.coinbase.com/learn/tips-and-tutorials/what-is-the-difference-between-optimistic-rollups-and-zk-rollups)
- [ZK vs Optimistic Rollups - Nervos](https://www.nervos.org/knowledge-base/zk_rollup_vs_optimistic_rollup)

### Implementation Details
- [How Optimism Works - TianPan.co](https://tianpan.co/notes/288-how-does-optimism-work)
- [OP Stack Rollup Process - Medium](https://medium.com/@rayer1045643889/detailed-explanation-of-the-op-stack-rollup-process-and-the-corresponding-code-9807c545d323)

---

*Document Version: 1.0*
*Last Updated: 2026-02-11*
