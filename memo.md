```
const path = require('path');
const fs = require('fs-extra');
const { Web3 } = require('web3');

// member1 details
const { tessera, besu, accounts } = require("./keys.js");
const { log } = require('console');
const host = besu.member1.url;
const accountAddress = besu.member1.accountAddress;

const contractJsonPath = path.resolve(__dirname,'SimpleStorage.json');
const contractJson = JSON.parse(fs.readFileSync(contractJsonPath));
const contractAbi = contractJson.abi;
const contractBytecode = contractJson.bytecode //.object



async function createContract(host) {
  const web3 = new Web3(host);
  const account = accounts.account1 //web3.eth.accounts.create();
  const contractConstructorInit = web3.eth.abi.encodeParameters(['uint256'], ['47']).slice(2);
  log("contractBytecode ",contractBytecode)
  log("contractConstructorInit ",contractConstructorInit)


  const txn = {
    chainId: 1981,
    nonce: await web3.eth.getTransactionCount(account.address),       // 0x00 because this is a new account
    from: account.address,
    to: "0x0000000000000000000000000000000000000000", //besu.member1.accountAddress,            //public tx
    value: null,
    data: '0x'+contractBytecode+contractConstructorInit,
    gasPrice: "0x0",     //ETH per unit of gas
    gas: "0x2CA51"  //max number of gas units the tx is allowed to use
  };

  console.log("create and sign the txn")
  console.log(txn)
  const signedTx = await web3.eth.accounts.signTransaction(txn, account.privateKey);
  console.log("sending the txn")
  const txReceipt = await web3.eth.sendSignedTransaction(signedTx.rawTransaction);
  console.log("txReceipt ", txReceipt)
  return txReceipt;
};

async function main(){
  createContract(host)

}

if (require.main === module) {
  main();
}

module.exports = exports = main

```



```
txReceipt  {
  blockHash: '0x1d23dcbebd7601dde29f118af010a2009ac41490e38e5f77e2996b0e5bb7978e',
  blockNumber: 6248n,
  cumulativeGasUsed: 77940n,
  from: '0xfe3b557e8fb62b89f4916b721be55ceb828dbd73',
  gasUsed: 77940n,
  effectiveGasPrice: 0n,
  logs: [],
  logsBloom: '0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000',
  status: 1n,
  to: '0x0000000000000000000000000000000000000000',
  transactionHash: '0x446789ee72c802f5a02aabadf6eca3bfcbd07863bd7064207c77b581c14327eb',
  transactionIndex: 0n,
  type: 0n
}
```

