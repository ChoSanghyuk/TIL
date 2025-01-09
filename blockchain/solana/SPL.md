





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

