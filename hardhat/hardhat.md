# hardhat



## 설치

### npm 및 hardhat 설치

스마트 컨트랙트 작성 폴더에 npm 및 hardhat 세팅

```bash
npm init -y
npm install --save-dev hardhat
npx hardhat init
```

- `--save-dev` : means Hardhat is only required during development (not for production)



### hardhat.config.ts

```tsx
module.exports = {
  solidity: "0.8.20",
  networks: {
    localhost: {
      url: "<http://127.0.0.1:8545>",
    },
  },
};
```



### 패키지 다운로드

```
npm install @openzeppelin/contracts
```



### 네트워크 세팅

```bash
npx hardhat node #서명자 불러오는 용도
```



:bulb: 컨트랙트들은 hardhat-project 위치에서 contracts 폴더 밑에 있어야 함



## 테스트 스크립트

### ignition/modules/Upgradeable.ts

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