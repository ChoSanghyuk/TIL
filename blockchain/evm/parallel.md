





We leverage a Bonsai feature that tracks addresses and slots touched or modified during a block or transaction's execution, called the accumulator. More information on this code can be found in our [Bonsai Explainer](https://consensys.io/blog/bonsai-tries-guide). If a slot, code, or anything else related to an account is modified, the Bonsai accumulator will keep track of this information. This is how we enable Bonsai's storage benefits too, only keeping track of state diffs block to block in our storage. We only need to take what the accumulator tracks at the block and transaction level, compare the modified state slots, and check for conflicts. By comparing the list of touched accounts from the transaction against the block's list, we can identify potential conflicts. Each time a transaction is added to the block, its list is incorporated into the block's list. Before adding a new transaction, we verify that it hasn't interacted with an account modified by the block (i.e., by previous transactions).
Note: Accounts read by the block are not considered conflicts. 



https://www.lfdecentralizedtrust.org/blog/introducing-parallel-transaction-execution-in-hyperledger-besu