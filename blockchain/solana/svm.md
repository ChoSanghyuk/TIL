# [Solana] 네트워크 동작 요소





## SVM - Sealevel Virtual Machine



### 개요

- SVM
  - Solana’s parallel smart contracts runtime
  - Solana transactions describe all the states a transaction will read or write while executing
    - non-overlapping transactions & reading the same state transactions be executed concurrently
- 비교 (vs EVM)
  - EVM (& EOS’s WASM-based runtimes) : single threaded
    - one contract at a time modifies the blockchain state
  - SVM :  process transactions in parallel



### Cloudbreak 구조

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



### Program Authority

- Program
  1. Programs can only change the data of accounts they own.
  2. Programs can only debit accounts they own.
  3. Any program can credit any account.
  4. Any program can read any account.
- System Program
  1. System Program is the only program that can assign account ownership.
     - :memo: By default, all accounts start as owned by the System Program
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

:bulb: The key insight here is that programs are code, and within our key-value store, there **exists some subset of keys that the program and only that program has write access.**



### Transactions 처리

- Transaction
  - Transactions specify an instruction vector
  - Each instruction contains 
    - the program
    - program instruction
    - list of accounts the transaction wants to read and write.

- Transaction Optimization

  - each instruction tells the VM which accounts it wants to read and write ahead of time

  - Interfaces such as readv or writev tell the kernel ahead of time all the memory the user wants to read or write

    => allows the OS to prefetch, prepare the device, and execute the operation concurrently if the device allows it

  - VM optimization

    1. Sort millions of pending transactions.
    2. Schedule all the non-overlapping transactions in parallel.









































https://solana.com/ko/news/sealevel---parallel-processing-thousands-of-smart-contracts