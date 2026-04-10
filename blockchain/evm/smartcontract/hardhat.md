# hardhat



## 개요

### hardhat

- 개념

  - Ethereum development environment
  - help developers build, test, and deploy smart contracts and dApps

- key features

  - Hardhat Network (The Local Blockchain)

    - When you run Hardhat, it automatically spins up a **local Ethereum network instance** on your machine.
    - Fast deploy & test / Built-in Debugging / "fork" a live public network

  - Compilation and Artifacts

    - turn human-readable Solidity code into machine-readable bytecode and generating essential metadata

  - Testing

    - unit tests for your smart contracts
      - Integrated Testing Framework (javaScript testing libraries like **Mocha** and **Chai**)
      - `ethers.js` Integration:

    



## 설치

### npm 및 hardhat 설치

스마트 컨트랙트 작성 폴더에 npm 및 hardhat 세팅

```bash
npm init -y
npm install --save-dev hardhat
npx hardhat init
```

- `--save-dev` : means Hardhat is only required during development (not for production)



**:bulb: npm vs npx**

| npm                                             | npx                                             |
| ----------------------------------------------- | ----------------------------------------------- |
| package installer and manager                   | package **executor**                            |
| **install, manage, share, and update** packages | **execute** Node.js package executables (CLIs). |

npx became bundled with npm starting with version 5.2.0.



### hardhat.config.ts

```tsx
module.exports = {
  // 1. Solidity Compiler Configuration
  solidity: {
    compilers: [
      {
        version: "0.8.13", // 프로젝트에서 사용할 solidity compiler 버전 명시.
        settings: {
          optimizer: {
            enabled: true, 	// true => optimizer 실행.
            runs: 200,			// 컨트랙트의 예상 효출 횟수	
          },
          metadata: {
              useLiteralContent: true
          }
        },
      },
    ],
  },
  
  // 2. Network Configuration (Crucial for deployment/interaction)
  networks: {
    // Hardhat's own integrated EVM (Default)
    hardhat: {
      // Configuration specific to the built-in Hardhat Network
    },
    // Local development network (별도로 로컬 노드 띄웠을 경우)
    localhost: {
      url: "<http://127.0.0.1:8545>",
    },
    // Example Testnet Configuration
    sepolia: {
      url: "YOUR_SEPOLIA_RPC_URL", // Public RPC endpoint
      accounts: ["YOUR_PRIVATE_KEY_HERE"], // Private key for the deployment account
      chainId: 11155111, // Optional but good practice
    },
  },
  
  // 3. Path Configuration (Usually left as default)
  paths: {
    sources: "./contracts", // default
    tests: "./test",
    cache: "./cache",
    artifacts: "./artifacts",
  },
};
```

- `solidity.settings.optimizer.runs`
  - It tells the compiler **how many times** you expect the contract's functions to be executed over the contract's lifetime.
  - 높은 숫자 => 실행 가스에 최적화. 낮은 숫자 => 배포 가스에 최적화



### 패키지 다운로드

```
npm install @openzeppelin/contracts
```



### 네트워크 세팅

```bash
npx hardhat node #서명자 불러오는 용도
```



## Compile

```bash
npx hardhat compile
```





## 테스트

### harthat test 명령어

```bash
# Run All Tests
npx hardhat test 
# Run Specific Test File
npx hardhat test test/TickMath.test.js 
# Run Tests with Gas Reporter
REPORT_GAS=true npx hardhat test
# Run Tests with Coverage
npx hardhat coverage
# Clean Build Artifacts
npx hardhat clean
# Run Hardhat Console
npx hardhat console
# Run Tests with Verbose Output
npx hardhat test --verbose
# Run Specific Test Case (using grep)
npx hardhat test --grep "MIN_TICK"
# Run Tests on Specific Network
npx hardhat test --network fuji
```



### 스크립트 예제

- ignition/modules/Upgradeable.ts

```tsx
import { ethers } from "hardhat";

async function main() {
  console.log("Deploying Implementation Contract...");
  const Implementation = await ethers.getContractFactory("Implementation");
  const implementation = await Implementation.deploy();
  await implementation.waitForDeployment();
  const implementationAddress = await implementation.getAddress();
  console.log(`✅ Implementation deployed at: ${implementationAddress}`);

  // Encode the initialize function call (initialize(42))
  const initData = Implementation.interface.encodeFunctionData("initialize", [42]);

  console.log("Deploying Proxy Contract...");
  const Proxy = await ethers.getContractFactory("BifrostProxy");
  const proxy = await Proxy.deploy(implementationAddress, initData);
  await proxy.waitForDeployment();
  const proxyAddress = await proxy.getAddress();
  console.log(`✅ Proxy deployed at: ${proxyAddress}`);

  // Interact with the contract via Proxy
  const proxiedContract = await ethers.getContractAt("Implementation", proxyAddress);

  console.log("Retrieving stored value from proxy...");
  const storedValue = await proxiedContract.getValue();
  console.log(`🔹 Stored Value in Proxy: ${storedValue.toString()}`);
}

main().catch((error) => {
  console.error("❌ Deployment failed:", error);
  process.exit(1);
});
```



### 실행

```bash
npx hardhat run ignition/modules/Upgradeable.ts --network localhost
```

- local network에 스크립트 실행



## 오류 케이스

`npx hardhat node` 노드 미실행

- signer가 없어서 `Cannot read properties of null (reading 'sendTransaction')`  오류 발생
- 해결:  `npx hardhat node` 실행



dependency 버전 불일치

- script에서 사용하는 라이브러리가 필요로하는 `ethers` 버전과 root project에서 설치한 `ethers` 버전의 차이로 오류 발생

- 해결 : `ethers` 삭제 후 버전에 맞게 재설치

  ```bash
  # Remove the incompatible ethers version
  npm uninstall ethers
  
  # Install ethers v6 (minimum v6.1.0) - 라이브러리에서 쓰는 버전으로 명시
  npm install --save-dev ethers@^6.1.0
  
  # 버전 확인
  npm list ethers
  ```



❌ Deployment failed: Error: Deployment at address {ImplementV1 address} is not registered

- `await upgrades.upgradeProxy(proxyAddress, ImplementationV2);` 코드에서 업그레이드 진행할 때 오류 발생
- 원인 : 처음 proxy를 `upgrades` 라이브러리를 사용 안해서 프록시로 인식을 못 해서 발생
- 해결 1: 강제 프록시임을 주입. `await upgrades.forceImport(proxyAddress, Implementation);`
- 해결 2 : 처음부터 `upgrades` 통해서 프록시 생성

