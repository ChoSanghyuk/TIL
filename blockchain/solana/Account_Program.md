# Solana Account & Program



can store up to 10MB of data

Accounts require a rent deposit in SOL, proportional to the amount of data stored, which is fully refundable when the account is closed.

Each account is identifiable by its unique address, represented as 32 bytes in the format of an [Ed25519](https://ed25519.cr.yp.to/) `PublicKey`.

It's important to note that while only the owner may deduct the balance, anyone can increase the balance.



Solana contains a small handful of native programs

When developing custom programs on Solana, you will commonly interact with two native programs, the System Program and the BPF Loader.



### System Program

all new accounts are owned by the System Program

- [New Account Creation](https://github.com/solana-labs/solana/blob/27eff8408b7223bb3c4ab70523f8a8dca3ca6645/programs/system/src/system_processor.rs#L145): Only the System Program can create new accounts.
- [Space Allocation](https://github.com/solana-labs/solana/blob/27eff8408b7223bb3c4ab70523f8a8dca3ca6645/programs/system/src/system_processor.rs#L70): Sets the byte capacity for the data field of each account.
- [Assign Program Ownership](https://github.com/solana-labs/solana/blob/27eff8408b7223bb3c4ab70523f8a8dca3ca6645/programs/system/src/system_processor.rs#L112): Once the System Program creates an account, it can reassign the designated program owner to a different program account. This is how custom programs take ownership of new accounts created by the System Program.



The [BPF Loader](https://github.com/solana-labs/solana/tree/27eff8408b7223bb3c4ab70523f8a8dca3ca6645/programs/bpf_loader/src) is the program designated as the "owner" of all other programs on the network, excluding Native Programs. It is responsible for deploying, upgrading, and executing custom programs.



- **Program Account**: The main account representing an on-chain program. This account stores the address of an executable data account (which stores the compiled program code) and the update authority for the program (address authorized to make changes to the program).
- **Program Executable Data Account**: An account that contains the executable byte code of the program.
- **Buffer Account**: A temporary account that stores byte code while a program is being actively deployed or upgraded. Once the process is complete, the data is transferred to the Program Executable Data Account and the buffer account is closed.



Sysvar accounts are special accounts located at predefined addresses that provide access to cluster state data.



![Program and Executable Data Accounts](./assets/program-account-expanded.svg)

The program account is literally just the address of the program executable data account. 

For simplicity, you can think of the "Program Account" as the program itself.

The address of the "Program Account" is commonly referred to as the “Program ID”, which is used to invoke the program.

The **Program Executable Data Account** is optimized to store compiled binary data, while the **Program Account** is optimized for lightweight metadata (rogram's public key and permissions, ...)





creating a data account for a custom program requires two steps:

1. Invoke the System Program to create an account, which then transfers ownership to a custom program
2. Invoke the custom program, which now owns the account, to then initialize the account data as defined in the program code



:bulb: **Solana 주요 프로그램 종류**

- Native Program
  - System Program : Account의 생성 및 Owner Program 지정 수행
  - BPF Loader : Native Program이 아닌 모든 Program의 owner로, program의 배포, 수정, 실행을 담당
- SPL Program : Solana 커뮤니티가 관리하는 표준 프로그램들의 집합
- Custom Program : 사용자 정의 프로그램





## Verifiable Programs [#](https://solana.com/docs/core/programs#verifiable-programs)

Ensuring the integrity and verifiability of on-chain code is essential. A verifiable build ensures that the executable code deployed on-chain can be independently verified to match its public source code by any third party. This process enhances transparency and trust, making it possible to detect discrepancies between the source code and the deployed program.



## PDA

Program Derived Addresses (PDAs) provide developers on Solana with two main use cases:

- **Deterministic Account Addresses**: PDAs provide a mechanism to deterministically derive an address using a combination of optional "seeds" (predefined inputs) and a specific program ID.
- **Enable Program Signing**: The Solana runtime enables programs to "sign" for PDAs which are derived from its program ID.

PDAs are addresses derived deterministically using a combination of user-defined seeds, a bump seed, and a program's ID.

PDAs are addresses that are deterministically derived and look like standard public keys, but have no associated private keys.

The derivation of a PDA requires 3 inputs.

- **Optional seeds**: Predefined inputs (e.g. string, number, other account addresses) used to derive a PDA. These inputs are converted to a buffer of bytes.
- **Bump seed**: An additional input (with a value between 255-0) that is used to guarantee that a valid PDA (off curve) is generated. This bump seed (starting with 255) is appended to the optional seeds when generating a PDA to "bump" the point off the Ed25519 curve. The bump seed is sometimes referred to as a "nonce".
- **Program ID**: The address of the program the PDA is derived from. This is also the program that can "sign" on behalf of the PDA







