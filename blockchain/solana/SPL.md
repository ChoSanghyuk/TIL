





The derivation of a PDA requires 3 inputs.

- **Optional seeds**: Predefined inputs (e.g. string, number, other account addresses) used to derive a PDA. These inputs are converted to a buffer of bytes.
- **Bump seed**: An additional input (with a value between 255-0) that is used to guarantee that a valid PDA (off curve) is generated. This bump seed (starting with 255) is appended to the optional seeds when generating a PDA to "bump" the point off the Ed25519 curve. The bump seed is sometimes referred to as a "nonce".
- **Program ID**: The address of the program the PDA is derived from. This is also the program that can "sign" on behalf of the PDA



https://solana.com/docs/core/tokens





Mint Account stores data such as:

- Supply: Total supply of the token
- Decimals: Decimal precision of the token
- Mint authority: The account authorized to create new units of the token, thus increasing the supply
- Freeze authority: The account authorized to freeze tokens from being transferred from "token accounts"



Token account stores data

- Mint: The type of token the Token Account holds units of
- Owner: The account authorized to transfer tokens out of the Token Account
- Amount: Units of the token the Token Account currently holds





account를 사전에 생성한 뒤에, 해당 account를 새로 mint하는 토큰의 account(data 보관소)로 사용

해당 account가 기본적인 authority를 가짐

account의 정보가 유지되기 위해서는 저장하고 있는 데이터의 양과 비례되는 rent 비용을 validator에게 지불해야 함

어카운트의 rent_epoch 정보 시점에 account가 보유하고 있는 lamport 양이 rent 비용보다 적다면 데이터는 말소됨

(Rent-exempt) 어카운트가 약 2년치에 해당하는 rent 비용을 보유하고 있다면 rent 비용이 면제됨 - solana tool로 계산 가능



Mint시 필요 계정

- payer - pays lamports to a created account 

- freeze authority - can "freeze" the tokens inside a wallet. So the owner of the tokens would not be able to move/transfer/sell them to a different wallet. think of this sort of like one way to create non-transferable tokens (aka soul-bound tokens)
- mint authority - is required to sign a transaction that actually creates new tokens (aka mints new token). So if create a fungible token called TokenX, and initially mint 10 tokens for a total supply of 10. You can later come back and mint new additional tokens into the max supply when this mint authority signs the transaction. raising the total supply of your fungible token



Solana에서는 특정 토큰을 들고 있기 위해서는 해당 토큰의 account를 별도로 받아야 함

=> 주로 ata (Associated Token Account)라고 하는 owner의 Pulic Key와 Mint account(token)의 Public Key를 hash해서 생성한 account 사용



### **How Is the Mint Account Different from Token Accounts?**

| **Aspect**           | **Mint Account**                          | **Token Account (e.g., ATA)**           |
| -------------------- | ----------------------------------------- | --------------------------------------- |
| **Purpose**          | Represents the token itself.              | Tracks balances of the token for users. |
| **Unique Per Token** | Always unique for a token.                | One per user (wallet) per token.        |
| **Tracks**           | Metadata (decimals, supply, authorities). | Token balance for a specific user.      |
| **Owned By**         | SPL Token Program.                        | Individual wallet or program.           |











시그니처 조회

`GetSignatureStatuses` 

solana는 기본적으로 signature cache를 대략 2분 동안만 들고 있음. 그 이후에 조회할 경우 nil값 반환

Unless the `searchTransactionHistory` configuration parameter is included, this method only searches the recent status cache of signatures, which retains statuses for all active slots plus `MAX_RECENT_BLOCKHASHES` rooted slots.



`GetTransaction`

전체 transaction hist에서 조회. 단, 노드 타입에 따라서 트랜잭션 데이터를 전부 저장하지 않음

The devnet node you're using doesn't have all history, and the transactions you're querying are likely from before its cutoff.

If we [search for your transaction signature in Explorer](https://explorer.solana.com/tx/8crW2M8mwCTLmbUbnbthi8KYyvN9iQd2a1XnTQvs9Zu4cTrFAaprU6fic66GLCVoovp5BX8e6cCJ38cYWhr7vg1?cluster=devnet), we get a clue: "Note: Transactions processed before block 116113408 are not available at this time"







### **What Does `decimals` Represent?**

The `decimals` value specifies the number of decimal places the token supports. For example:

- If `decimals = 0`, the token is indivisible, and the smallest unit is `1`.
- If `decimals = 2`, the token supports up to two decimal places (e.g., `0.01` is the smallest unit).
- If `decimals = 9`, the token supports up to nine decimal places (e.g., `0.000000001` is the smallest unit).

This is similar to how cryptocurrencies like Bitcoin or Ethereum have a smallest unit:

- Bitcoin: `decimals = 8` (1 BTC = 100,000,000 satoshis)
- Ethereum: `decimals = 18` (1 ETH = 10^18 wei)







