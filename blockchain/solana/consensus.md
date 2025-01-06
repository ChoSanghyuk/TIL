# solana consensus





This time problem gets pretty difficult in distributed systems. When you need to move fast and process transactions as soon as possible, you need to be able to time in small units. But many programmable blockchains, like Ethereum, rely on outside programs to assign a “median” timestamp — which they then use to validate transactions in the order they were received.



But referring back to a centralized source defeats the purpose of a decentralized system. Solana solves this problem by using an innovative technology called **Proof of History**, which allows these “timestamps” to be built into the blockchain itself. This is done through a verifiable delay function, a VDF.

Solana does this by inserting data into the sequence by appending the hash of the data of the previously generated states. The state, input data, and count are all published — and impossible to recreate or create alternate versions of. This sets up an upper bound on time — and because Proof of History can reference previous hashes, there is also a lower bound of time. While this VDF won’t tell you it’s 12:02:01 PM, it will tell you exactly when in the past and future of the global state machine a transaction occurred.

The block producers are doing this locally, in approximate real time, with a SHA256 hash function — which is what most major chip manufacturers are optimized towards. And using this hash to keep track of time, Yakovenko says, “gives the ledger this interesting property where you can infer when events occurred when you examine it.”



On the Solana blockchain, any individual node can validate the entire chain with just a small piece of information — even if they’re not connected to the rest of the network. In fact, even if every computer runs at a slightly different speed, the ASIC will stay within 30% of what is bound in the network. “Everybody has this local synchronized atomic clock and these clocks never need to be resynchronized,” Yakovenko says. “So even if we get cut off and communication links go down, our clocks never drift because they are logical based on this SHA256.”

Even better, because the blockchain can be verified by a small piece of info, it means that it can be verified in parallel, or more than one piece can be verified at any given time. Most programmable blockchains can only validate a blockchain one at a time.

Think of it this way: Other Chain Railways only has one attendant doing that long verification process for every letter on the train. But the Solana Railroad has multiple attendants that can check many different letters on the train for their stamps at the same time — which means the trains move much faster.



https://solana.com/news/proof-of-history



---



The timestamp, being a hash of the previous PoH and the current block, establishes a chronological order of blocks in the [blockchain](https://unchainedcrypto.com/interested-in-crypto-here-are-10-essential-terms-you-need-to-know/). This timestamp is broadcast across the network and can be authenticated and stored by all network nodes.

The Verifiable Delay Function is a cryptographic function that demands significant computational effort to compute but can be swiftly verified. Nodes within the network can easily confirm that timestamps were produced within the expected timeframe and were not pre-computed before block addition to the chain. This mechanism ensures the integrity and accuracy of the blockchain’s timestamping process, enhancing its reliability and trustworthiness.

Comparatively, in Proof of Work and Proof of Stake, nodes must synchronize their clocks, a time-consuming task limiting transaction throughput. 

Despite complex computations in PoH, the absence of time synchronization issues allows for improved transaction throughput in PoH. Thus, Proof of History offers a promising solution to scalability challenges.



https://unchainedcrypto.com/solana-proof-of-history/







---

노드 설명



Solana has two types of nodes: validator and RPC nodes



Validator N\nodes (also known as Consensus Nodes) within Solana’s Proof-of-Stake (PoS) consensus model are the entities responsible for confirming if blocks are valid, and ultimately finalizing the transactions by voting on blocks produced by a leader node, or by being the leader. 

Solana’s Proof-of-History (PoH) protocol is a cryptographic way to reliably order transactions and events recorded on the decentralized ledger. Solana's architecture enables transactions to be ordered as they enter the network, rather than by block, which is done by the validator nodes. 

Solana uses a delegated Proof-of-Stake protocol. Anyone who owns SOL can delegate it to a validator, whereby the validator then earns influence on the network which leads to them being assigned as a leader for more slots, as well as earning more rewards for voting.



Consensus nodes **participate in the cluster consensus by voting on the blocks produced by the leader** whereas RPC nodes do not. If a voting validator has a staked account, it will earn vote credit for performing voting as long as it successfully votes on blocks that are added to the blockchain. 



Solana RPC node providers facilitate easy access for developers to send requests to and receive payloads from nodes on Solana's network instead of requiring Solana dApps to be responsible for their own node infrastructure, which is a heavy cost in terms of time and money.

https://www.alchemy.com/overviews/solana-nodes



---

The transaction’s blockhash expires in 1 minute and 19 seconds (after 150 blocks).



The RPC provider sends the transaction to both the current and next leaders(validator node). Thus, the RPC provider could retry sending the transaction for 1 minute and 19 seconds until it is included in a block. However, Mert from Helius notes that this could spam the network, and the team might avoid this approach. Instead, Helius documentation recommends dapps retry every 2 seconds to avoid spamming. Another strategy is to include multiple RPC providers, as Drift does, allowing users to switch providers if one is not performing well.



In Solana, a leader is elected to produce four consecutive blocks, with each block being created approximately every 400 milliseconds (referred to as a slot). Each block is 128MB in size and contains 48 million compute units.

Leaders for each epoch, comprising 432,000 slots (equivalent to 2–3 days), are selected randomly based on their stake-weight.



Solana is mempool-less (using Gulf Stream), so transactions must reach the validator to be included in a block, and then be propagated to other validators using Turbine. For a transaction to be considered final, 31 additional blocks must be built on top of it. Solana uses QUIC, a reliable UDP protocol developed by Google for YouTube, instead of traditional TCP/UDP protocols.



**What Goes Inside a Validator That Is a Leader Building the Block?**
A validator uses a single-threaded scheduler that employs an algorithm called Prio-Graph. This algorithm prioritizes transactions based on their priority fee(/it being a vote transaction) and creates edges between transactions with lower priority that conflict with higher-priority ones (those touching the same state/account and thus can’t be parallelized). This allows non-conflicting transactions to run in parallel.
There are two threads for executing vote transactions and four threads for non-vote transactions (user/bot transactions). Signature verification is the most compute-intensive stage in the Transaction Processing Unit (TPU) pipeline and utilizes GPUs. In total, six threads handle these transactions.
To conserve bandwidth, validators avoid gossip/n² P2P connections for block propagation. Instead, they communicate block validations by submitting votes(vote transactions) once they have run Transaction Validation Unit (TVU) on the transactions of the block that they are voting for. Approximately half of the transactions in a block are vote transactions, prioritized to maintain network synchronization.



https://medium.com/@dattgoswami/understanding-transaction-inclusion-in-solana-from-wallets-to-validators-9e412ae792b3



---



PoH is not a consensus algorithm, but rather a tool used by Solana’s consensus mechanism for synchronization.



**Solana has two primary confirmation rules**: one for short-term fork selection (*optimistic confirmation*) and another for full PoS consensus for finality (*finalized/rooted*). Clients and users are able to adhere to these confirmation rules for desired security properties and customized UX selections. This is reflected by two commitment levels: “confirmed” and “finalized”.



The core of PoH is a simple hashing algorithm [akin to](https://github.com/solana-labs/solana/issues/388) a Verifiable Delay Function (VDF) but is [not technically a VDF](https://www.youtube.com/watch?v=oBW2KJq3FnA&ab_channel=NervosNetwork). Solana implements this using a sequential pre-image resistant hash function (SHA-256) that continuously runs, using the output of one iteration as the input for the next. This computation runs on a single core of each validator.

While the sequence generation is sequential and single-threaded, the output can be verified in parallel, allowing for efficient verification on multi-core systems. While there exists an upper bound on hashing speed, improvements in hardware can potentially offer additional performance benefits. 



Ethereum, for example, [uses the Network Time Protocol (NTP) in each block for consensus](https://ethereum.stackexchange.com/questions/5924/how-do-ethereum-mining-nodes-maintain-a-time-consistent-with-the-network). On Solana, each validator independently verifies that each block was produced in the correct time slot endogenously, without the use of an external protocol.











https://www.helius.dev/blog/consensus-on-solana







---



### **Why Leader D Cannot Produce a Block Before Leader C**

1. **Leader Schedule Enforces Order:**
   - Solana has a **pre-determined leader schedule** where each slot is assigned to a specific leader.
   - Only the designated leader for a slot can produce a valid block for that slot.

Validators verify the leader's identity and ensure the block corresponds to the correct slot.



































