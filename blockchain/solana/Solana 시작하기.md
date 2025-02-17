# Solana 시작하기

## **설치**

### **Rust**

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y
rustc --version
```

### **Solana CLI**

```bash
sh -c "$(curl -sSfL https://release.anza.xyz/stable/install)"
echo 'export PATH="$HOME/.local/share/solana/install/active_release/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
solana --version
```

- update

```bash
agave-install update
```

### **Anchor CLI**

Anchor is a framework for developing Solana programs. The Anchor framework leverages Rust macros to simplify the process of writing Solana programs.

```bash
cargo install --git https://github.com/coral-xyz/anchor avm --force
avm --version
avm install latest
avm use latest
anchor --version
```

### **Node.js & Yarn**

- node

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/master/install.sh | bash
command -v nvm
nvm install node
node --version
```

- yarn

```bash
npm install --global yarn
yarn --version
```

## **설정**

### **네트워크**

- 확인

```bash
solana config get
```

- 변경

```bash
solana config set --url mainnet-beta
solana config set --url devnet
solana config set --url localhost
solana config set --url testnet
```

- 변경 (shortcut)

```bash
solana config set -um    # For mainnet-beta
solana config set -ud    # For devnet
solana config set -ul    # For localhost
solana config set -ut    # For testnet
```

### **지갑**

- 지갑 생성

```bash
solana-keygen new
solana address
```

- devnet SOL airdrop

```bash
solana config set -ud
solana airdrop 2
solana balance
```

💡 Phantom 지갑과 연결할 때에는 `id.json`에 있는 private key로 연동 필요 (니모닉 사용 시, 사용하는 key path의 차이로 다른 주소 반환)

## **account**

### **Solana accounts**

- contain either:
    - State: This is data that's meant to be read from and persisted. It could be information about tokens, user data, or any other type of data defined within a program.
    - Executable Programs: These are accounts that contain the actual code of Solana programs. They contain the instructions that can be executed on the network.
- AccountInfo
    
    ```json
    {
      "data": {
        "type": "Buffer",
        "data": []
      },
      "executable": false,
      "lamports": 7000000000,
      "owner": "11111111111111111111111111111111",
      "rentEpoch": 18446744073709552000,
      "space": 0
    }
    ```
    
    - `data` - This field contains what we generally refer to as the account "data"
        - When data is "buffered" in this way, it maintains its integrity and can be later deserialized back into its original form for use in applications
    - `executable` - A flag that indicates whether the account is an executable program
        - `owner` - This field shows which program controls the account.
            - For wallets, it's always the System Program, with the address `11111111111111111111111111111111`.
    - `lamports` - The account's balance in lamports (1 SOL = 1,000,000,000 lamports).
    - `rentEpoch` - A legacy field related to Solana's deprecated rent collection mechanism (currently not in use).
    - `space` - Indicates byte capacity (length) of the `data` field, but is not a field in the `AccountInfo` type

## **hardware**

### **solana hardware requirements**

- **CPU**
    - A high-performance CPU with at least 12 cores and a 2.8 GHz clock speed.
    - AMD CPUs are often preferred because of their compatibility with the blockchain software.
- **RAM**
    - At least 128 GB for consensus validator nodes, and 258 GB or more for RPC nodes.
- **Storage**
    - At least 1 TB for each of the 2–4 NVME drives.
    - A larger ledger disk is needed if you want a longer transaction history.
- **Network**
    - A 1 GBPS up and down link speed that is unshaped and unmetered.
    - Bandwidth requirements vary based on stake level and location on the network.