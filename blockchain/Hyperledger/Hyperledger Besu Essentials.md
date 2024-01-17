# Hyperledger Besu Essentials



## Installing Besu 



### Linux

#### 사전 세팅 - java 설치

- `sudo apt install openjdk-17-jdk-headless`

#### Besu 설치

- Install Besu

  - 아래 url에서 zip 파일 다운로드 
  - :link: https://github.com/hyperledger/besu/releases

- 압축풀고 ubuntu에서 해당 파일로 이동한뒤 설치 여부 확인

  ```sh
  cd /mnt/c/Users/???/lecture_code/besu-24.1.0
  ./bin/besu --help

![image-20240115072126518](Hyperledger Besu Essentials.assets/image-20240115072126518.png)



### Windows

#### 사전 세팅

- [Have a 64-bit version of Windows installed](https://support.microsoft.com/en-us/windows/which-version-of-windows-operating-system-am-i-running-628bec99-476a-2c13-5296-9dd081cdd808#:~:text=Select the Start button > Settings > System > About .&text=Under Device specifications > System type,Windows your device is running) 
- [Download a 64-bit version of JDK/JRE](https://www.oracle.com/java/technologies/javase-downloads.html). We recommend that you also remove any 32-bit JDK/JRE installations.
- Download [Git for Windows](https://git-scm.com/downloads) if you do not already have it installed.

#### Besu 설치

- Install Besu

  :bulb: 사전에 git bash 사전 설정 필요 (파일 경로 제한 해제)

  ```sh
  git config --system core.longpaths true
  ```

  - 설치

  ```sh
  git clone --recursive https://github.com/hyperledger/besu
  ```

- Build Besu

  ```sh
  export JAVA_HOME="<java경로>"
  cd besu
  ./gradlew build -x test
  cd build\distributions
  tar -xzf besu-<version>.tar.gz
  cd besu-<version>
  ```

  - `<java경로>` :  java가 설치된 경로 기입 (ex. `C:\Program Files\Java\jdk-17`)
  - `<version>`  : Besu 버전 설치

- Confirm Installation
  
  ```sh
  bin\besu --help
  ```
  
  



## Starting and Understanding Besu



### 개요

- Hyperledger Besu can be used to connect to the existing public Ethereum network, referred to as mainnet, and can be used to create the Ethereum Virtual Machine (EVM) compatible private blockchain network. 
- Hyperledger Besu can be used to run a node on Ethereum mainnet, from **a full node** that helps sync Ethereum mainnet, to **an archive node**, which contains the data of the blockchain but does not participate in adding new blocks. 
- On mainnet, **Besu runs as an execution client**, performing tasks such as executing transactions and accepting changes to the ledger.



### 실행

- `./bin/besu`

- `besu --network=<network> --data-path=<path>/<networkdata-path>`

  - `<network>` : the network you are connecting to. ex) sepolia and goerli

  - `<path>` : connecting to a network other than the network previously connected to, you must either delete the local block data or use the `--data-path` option to specify a different data directory
    - To delete the local block data, delete the `database` directory 

  - ex) `./bin/besu --network=sepolia --data-path=./besu-test-db`




### Using Config

- 개요

  - Options and subcommands can be stored in what we call a configuration file, or config file. 

  - This file allows for the specifications of **initial parameters for Besu**, and gives us a file where we can save those parameters in the event we want to **reuse** them.

- 예시

```toml
# Valid TOML config file
data-path="./besudata" # command 실행 기점 상대경로
# This is the datapath where the blockchain data will be stored

# Network
bootnodes=["enode://001@123:4567", "enode://002@123:4567", "enode://003@123:4567"]
# In a private network, an enode URL is the identifier for a node and
# allows for the bootnodes to discover each other as they start up. In
# this example, there are three bootnodes stipulated for this network

p2p-host="1.2.3.4"
# Stipulates the advertised host that can be used to access the node
# from outside the network in Peer to Peer communication
p2p-port=1234
#
max-peers=42
# The maximum number of peer to peer connections this network can
# establish

rpc-http-host="5.6.7.8"
# Specifies the host on which HTTP JSON-RPC listens
rpc-http-port=5678
# The HTTP JSON-RPC listening port (TCP)

rpc-ws-host="9.10.11.12"
# The host for Websocket WS-RPC to listen on
rpc-ws-port=9101
# The Websockets JSON-RPC listening port (TCP)

genesis-file="~/genesis.json"
# Path to the custom genesis file. The next lesson will explain how to
# create a genesis file and what is contained in the genesis file

miner-enabled=true
# This option enables mining when the node is started. The type of
# consensus mechanism will be stipulated in the genesis file

miner-coinbase="0xfe3b557e8fb62b89f4916b721be55ceb828dbd73"
# miner-coinbase provides the account to which mining rewards will be
# paid for this blockchain
```

- 실행
  - `besu --config-file=<file-path>/config.toml`



### Understanding the Genesis File

- 개요

  - **The first block** in a blockchain is called the genesis block. 
  - In order to join or create any network, the data for the genesis block must be included. 
  - The genesis file **defines the data** that is in the first block of a blockchain, as well as **rules** for the blockchain itself.

  - The genesis file is a JSON formatted file

- 샘플

```json
{
  "config": {
    "chainId": 2018,
    "muirglacierblock": 0,
    "ibft2": {
      "blockperiodseconds": 2,
      "epochlength": 30000,
      "requesttimeoutseconds": 4
    }
  },
  "nonce": "0x0",
  "timestamp": "0x58ee40ba",
  "extraData": "0xf83ea00000000000000000000000000000000000000000000000000000000000000000d5949811ebc35d7b06b3fa8dc5809a1f9c52751e1deb808400000000c0",
  "gasLimit": "0x1fffffffffffff",
  "difficulty": "0x1",
  "mixHash": "0x63746963616c2062797a616e74696e65206661756c7420746f6c6572616e6365",
  "coinbase": "0x0000000000000000000000000000000000000000",
  "alloc": {
    "9811ebc35d7b06b3fa8dc5809a1f9c52751e1deb": {
      "balance": "0xad78ebc5ac6200000"
    }
  }
}
```

- 구성

    - **config key section** : contains the following information about the blockchain

      - `"chainId": 2018`

        - Ethereum networks have two identifiers, a network ID and a chain ID. Although they often have the same value, they have different uses.

        - Peer-to-peer communication between nodes uses the *network ID*, while the transaction signature process uses the *chain ID*.

          => *network ID*와 *chain ID* 둘 중 하나만 바뀌어도 기존 체인과 피어링 불가

      - `"muirglacierblock": 0`

        - This field is called a “milestone block”
        - Muir Glacier refers to a specific network upgrade that occurred at block 9,200,000 on Ethereum mainnet
        - For private networks, like the one that is being created in this example, the name of the latest milestone block can be listed, and set to be the genesis block
      
      - `"ibft2":`
        - This specifies that the consensus protocol for the blockchain is IBFT 2.0
        - `"blockperiodseconds": 2`
          - The minimum block time, in seconds. 
          - In this case, after two seconds, a new block will be proposed by the network.
        - `"epochlength": 30000`
          - The number of blocks at which to reset all votes
          - The votes refer to **validators voting** to add or remove validators to the network
          - In this case, after 30,000 blocks are created, this IBFT 2.0 network will discard all pending(보류중인) votes collected from received blocks
        - `"requesttimeoutseconds": 4`
          - The time by which a new block must be proposed or else a new validator will be assigned by the network
          - If a validator goes down, the request time out ensures that proposal of a new block passes on to another validator. 
          - **The request time out seconds should be set to be double the minimum block time**
      
    - **second section** : contains information about the **genesis block**
      
      - `"nonce": "0x0"` 
        - a part of the blockheader for the first block. 
        - Set to 0x0
      
      - `"timestamp": "0x58ee40ba"`
        - The creation date and time of the block. 
        - Often it can be set to 0x0, but as long as it is any value in the past, it will work
      
      - `"extraData": "0xf83ea00000000000000000000000000000000000000000000000000000000000000 000d5949811ebc35d7b06b3fa8dc5809a1f9c52751e1deb808400000000c0"`
        - Extra data is a recursive length prefix (RLP) encoded string (which is space efficient) containing the validator address of the IBFT 2.0 private network.
        - :link: ["Extra Data"](https://besu.hyperledger.org/private-networks/how-to/configure/consensus/ibft#extra-data)
      
      - `"gasLimit": "0x1fffffffffffff"`
        - The block gas limit, which is the total gas limit for all transactions included in a block. 
        - It defines **how large the block size** can be for the block, and is represented by an hexadecimal string. 
        - For this network, the gas limit is the maximum size, and is therefore a [“free gas network”](https://besu.hyperledger.org/private-networks/how-to/configure/free-gas)
      
      - `"difficulty": "0x1"`
      
        - The difficulty of creating a new block
        - Represented as a hexadecimal string
        - the difficulty is set to 1, effectively the lowest difficulty.
      
      - `"mixHash": "0x63746963616c2062797a616e74696e65206661756c7420746f6c6572616e6365"`
      
        - The mixHash is the **unique identifier** for the block
      
      - `"coinbase": "0x0000000000000000000000000000000000000000"`
      
        - The coinbase account, which is where all block rewards for this network will be paid. 
        - In this case it is to 0x0000000000000000000000000000000000000000, which is sometimes called address(0) or the zero address.
      
      - ```json
        "alloc": {
          "9811ebc35d7b06b3fa8dc5809a1f9c52751e1deb": {
            "balance": "0xad78ebc5ac6200000"
          }
        }
        ```
      
        - The alloc field creates an address on our network, which is sometimes also referred to as an externally owned account, as it is an account not associated with a smart contract
        - The number starting with “98” is the public key of the address. 
        - The balance can be passed in as a decimal OR a hexadecimal (like it has in this case and corresponds to 200 ETH, or 2*10^20 Wei). The balance is always in Wei, or 10^-18 Ether.
      
        :bulb: Wei는 이더리움 네트워크에서 사용되는 최소 단위로, 이더(ETH)의 가장 작은 단위입니다

- 실행
  - `./bin/besu --genesis-file=./genesis.json`



:bulb: gas

- Transactions use computational resources so have an associated cost. **Gas is the cost unit** and the gas price is the price per gas unit. The transaction cost is the gas used * gas price.

- In **public networks**, the account submitting the transaction pays the transaction cost, in Ether. The **miner** (or validator in PoA networks) that includes the transaction in a block **receives transaction cost**. (항동은 아니고, gas price로 부터 결정됨)

- In many **private networks**, network participants run the validators and **do not require gas as an incentive**. Networks that don't require gas as an incentive usually **configure the gas price to be zero** (that is, free gas). Some private networks might allocate Ether and use a non-zero gas price to limit resource use.





## Creating a Private Network with the Quickstart



node 설치하기

nvm 설치

`curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash`

nodejs 설치

`nvm install 20.11.0`

Nodesource 설치 : Nodesource에서 Nodejs 패키지를 설치할 것임을 Ubuntu에 알립니다.

```
curl -sL https://deb.nodesource.com/setup_18.x | sudo -E bash -
```

NodeJS 설치

Nodesource 세팅이 끝나면, 이제 Nodejs v14.4 설치를 할 수 있습니다.

```
sudo do-release-upgrade
```



https://www.freecodecamp.org/korean/news/how-to-install-node-js-on-ubuntu-and-update-npm-to-the-latest-version/

































