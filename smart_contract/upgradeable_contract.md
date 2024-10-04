# 업그레이더블 컨트랙트



https://medium.com/@aiden.p/%EC%97%85%EA%B7%B8%EB%A0%88%EC%9D%B4%EB%8D%94%EB%B8%94-%EC%BB%A8%ED%8A%B8%EB%9E%99%ED%8A%B8-%EC%94%A8-%EB%A6%AC%EC%A6%88-part-1-%EC%97%85%EA%B7%B8%EB%A0%88%EC%9D%B4%EB%8D%94%EB%B8%94-%EC%BB%A8%ED%8A%B8%EB%9E%99%ED%8A%B8%EB%9E%80-b433225ebf58



### 업그레이더블 컨트랙트란?

- 개요

  - 업그레이드가 가능한 컨트랙트를 의미

    <= **프록시 패턴(Proxy Pattern)**

- 동작

  - 사용자가 프록시 컨트랙트를 통해 어떠한 함수를 호출하면, 프록시 컨트랙트에 저장되어 있는 주소를 이용하여 로직 컨트랙트를 호출
  - `delegatecall`은 다른 컨트랙트 어카운트의 code를 사용하되, storage는 기존 컨트랙트 어카운트의 그것을 사용

  

  :bulb: 솔리디티는 다른 컨트랙트를 호출할 때, 크게 `call`과 `delegatecall` 이다.

  -  `call`이 바로 우리가 통상적으로 컨트랙트를 호출할 때 사용하는 opcode
     -  opcode(operation code) : low-level instruction used in EVM to perform arithmetic calculations, data storage and retrieval, control flow
  -  `delegatecall`은 다른 컨트랙트의 코드를 사용하되, 실행 환경(Context)은 기존 컨트랙트에서 수행될 수 있도록 한다.
    - A 컨트랙트가 B 컨트랙트를 호출할 때, `delegatecall`을 이용하게 되면 B 컨트랙트의 Code를 사용하지만, Storage는 A 컨트랙트를 사용하게 된다. 트랜잭션 실행의 컨텍스트(Context)가 그대로 유지되는것이 `delegatecall`의 핵심이다.

- 문제점
  - 슬롯 충돌
  - 생성자 실행으로 인한 값 초기화



### 슬롯 충돌

#### 프록시 슬롯 - 로직 컨트랙트 슬롯 충돌 

EIP-1967: Standard Proxy Storage Slots

스토리지 슬롯을 순차적으로 사용하면 충돌 가능성이 높으니, **슬롯을 랜덤에 가깝게(pseudo-random) 배정**하면 된다는 것이다.

저장하고 싶은 변수의 이름을 keccak256으로 해싱한 후 1을 뺀 값을 슬롯 넘버로 사용하는 것이다. 이 때 주의해야 할 점은 해싱에 사용되는 값을 **절대로 중복해서 사용하지 않아야 한다**는 것이다. 그렇지 않다면 동일하게 스토리지 충돌이 발생하게 된다.

#### 로직 컨트랙트 업그레이드 시 슬롯 충돌

EIP-1967으로도 피해갈 수 없는 스토리지 충돌이 존재한다. 바로 이전 버전의 로직 컨트랙트와 업그레이드된 새로운 버전의 로직 컨트랙트의 스토리지 충돌

이러한 충돌을 피하기 위해서는 업그레이드시 상태 변수의 선언 순서에 주의를 기울여야 한다.

상태 변수들의 선언 위치를 하나하나 신경 쓰는것은 상당한 주의를 요한다. 때문에 실제로 업그레이더블 컨트랙트를 작성할 때는 **기존의 로직 컨트랙트를 상속해서 작성**하여 기존의 변수 선언에 변화가 없도록 하는것이 일반적이다.





### 생성자 초기화 코드(Initializing Constructor Code)

프록시 패턴에서는 **로직 컨트랙트에서** 생성자(Constructor)를 사용할 수 없다. 생성자 함수는 컨트랙트 배포시에만 단 한번 호출되고 런타임 바이트코드에 포함되지 않으므로, 프록시 컨트랙트는 이를 호출할 수 없다.

생성자 함수의 역할을 `initialize()` 함수로 옮기고, 해당 함수가 컨트랙트 라이프사이클에서 단 한번만 호출되도록 보장하게 하는 방식이다.



```solidity
import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

contract A {
	constructor(address foo) {
		// do something...
	}
}
contract B is Initializable {
    // cannot call initialize more than once due to the `initializer` modifier
    function initialize(address foo) public initializer {
        // do something same as contract A contructor code
    }
}
```

위 코드 예시에서 B와 같이 코드를 작성하면 생성자 코드와 동일한 역할을 `initialize` 함수를 통해 수행할 수 있다. 주의할 점은 반드시 `initializer` modifier를 `initialize` 함수에 적용해야 한다는 것이다.



생성자 vs initializer

- 생성자 : 로직 컨트랙트가 새로 배포될 때마다 실행 => 원치 않은 변수 수정이 발생할 수 있음
- initializer : 첫 로직 컨트랙트가 배포될 때에만 실행되며 업그레이된 로직 컨트랙트가 새로 배포되었을 때에는 실행되지 않음
  - initializer는 로직 컨트랙트 내에 있지만, 실질적으로 동작되는 것은 프록시 컨트랙트의 state이기 때문에, 처음 배포 여부 알 수 있음



Storage Layout

When you use a struct the layout of the struct in storage is determined by the order of its elements.

If you upgrade a contract and change the order the elements in the struct or add new elements, it can cause inconsistencies and overwrite existing data in storage





### 함수 충돌(Function Clashes)

핵심은 프록시 컨트랙트에 존재하지 않는 함수 식별자를 통해 호출하게 되면, 자연스럽게 `fallback` 함수로 이어져 `delegatecall`로 로직 컨트랙트의 함수를 호출하게 되는것이었다.



주목할점은 프록시 컨트랙트의 기능 구현은 로직 컨트랙트에서 이뤄지는것이 맞지만, 업그레이드 관련 기능을 수행하는 함수는 여전히 필요하다는 것이다. 이 때, 로직 컨트랙트도 동일한 함수를 가지고 있으면 어떻게 되는걸까? 호출자가 프록시 컨트랙트의 업그레이드 함수를 호출하는건지, 로직 컨트랙트의 그것을 호출하는건지 그 의도를 알 수 없게 된다.

솔리디티에서 함수 식별자는 함수 시그니처(signature)를 해싱하여 앞의 4바이트만 사용. 솔리디티 컴파일러는 같은 컨트랙트 내에서 시그니처가 다른데도 불구하고 식별자가 겹치는 경우를 미리 방지한다. 하지만, 프록시와 로직 컨트랙트는 구분된 ‘다른’ 컨트랙트이다

=> Transparent 패턴



### Transparent

Transparent 프록시 패턴의 핵심은 두가지이다.

1. 사용자 어카운트와 어드민 어카운트의 함수 호출 대상 컨트랙트를 구분하는 것

   => 유저 어카운트는 항상 로직 컨트랙트의 함수를 실행하도록 하고, 프록시 컨트랙트 오너는 항상 프록시 컨트랙트의 함수를 실행하도록 한다. 

2. 업그레이드 관련 로직을 프록시 컨트랙트에 구현하는 것

   => 프록시 컨트랙트의 오너가 항상 업그레이드를 문제 없이 수행할 수 있도록 보장



컨트랙트 개발을 할 때 항상 명심해야 하는것. 바로 보안과 가스다. 

=> UUPS 패턴



### UUPS(Universal Upgradeable Proxy Standard)

 [EIP-1822](https://eips.ethereum.org/EIPS/eip-1822) 에서 제안된 패턴으로, Transparent 패턴과 달리 업그레이드 로직이 구현체, 즉 로직 컨트랙트에 위치하게 된다.

로직 컨트랙트 내부에 업그레이드 함수를 구현하고, 해당 함수 내부에서만 `msg.sender`의 어드민 여부를 확인하면 된다.

UUPS 패턴을 사용할 때 조심해야 할 점은 반드시 업그레이드를 수행할 때, 업그레이드 기능이 포함되어야 한다는 것이다. 그렇지 않을 경우 앞으로 업그레이드를 영원히 진행하지 못하는 이슈가 발생할 수 있다. 따라서, 업그레이드시에 **이를 확인해줄 수 있는 기능이 포함된 검증된 라이브러리를 기반으로 작업하는것을 권장**한다. 이를테면 오픈제플린의 [UUPSUpgradeable](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/proxy/utils/UUPSUpgradeable.sol) 컨트랙트를 상속하여 사용하자.

*큰 장점 장기적으로 컨트랙트의 탈중앙성을 지킬 수 있다는 것이다 UUPS는 업그레이드가 영원히 필요하지 않을 때, upgradeTo() 함수를 누구도 호출할 수 없도록 하는 업그레이드를 진행하여 업그레이드 기능을 영원히 봉인할 수 있다!*





### 컨트랙트를 재사용하자

프록시 패턴의 핵심은 **로직 컨트랙트에 실행을 위임(delegatecall)**하는 것에 있다. 여기서 한가지 주목해야 할 점은 로직 컨트랙트에 실행을 위임하는 프록시 컨트랙트는 **단 하나만 존재할 이유가 없다는 것**이다.

![img](https://miro.medium.com/v2/resize:fit:630/1*M7D4_vRIpqeOag4DmgRSjg.png)

프록시 컨트랙트는 대체로 로직 컨트랙트의 주소를 관리하는것과 `delegatecall`을 통한 로직 컨트랙트의 호출 부분만 구현되므로, 배포시에 필요한 가스가 기존에 비해 상당히 절감되는 편이다.

모든 컨트랙트를 업그레이드하기 위해서는 위 그림처럼 10000개의 프록시 컨트랙트에서 한땀 한땀 `upgradeTo()` 함수를 호출해야 한다. 즉 **10000번의 업그레이드 실행 트랜잭션**이 필요하다. 이런 방식은 사용성을 심각하게 저해하는것은 물론, 가스 비용 또한 폭발적으로 증가시킨다. 또한 만약 컨트랙트의 업그레이드 권한이 개별로 존재할 경우 동시에 업그레이드를 수행하는것이 불가능에 가까워진다.



### 비콘 프록시(Beacon Proxy)

이런 문제를 해결하기 위해 등장한것이 바로 비콘 프록시 패턴이다

비콘 : 봉화나 등대와 같이 위치 정보를 전달하기 위해 어떤 신호를 주기적으로 전송하는 기기

비콘 컨트랙트는 말 그대로 **프록시 컨트랙트가 어떤 로직 컨트랙트를 호출해야 하는지를 알려주기 위한 이정표** 역할

비콘 컨트랙트가 프록시 컨트랙트에서 어떤 로직 컨트랙트를 호출해야 하는지를 알려주고 있다. 따라서, 비콘 컨트랙트에 저장된 로직 컨트랙트 주소만 변경해주면, 비콘 컨트랙트를 바라보는 수 많은 프록시 컨트랙트들이 동시에 업그레이드 된다.

`implementation` 컨트랙트 주소를 바로 가져오지 않고, **비콘 컨트랙트에서 가져오고 있다**는 점이다. 나머지는 일반적인 프록시 컨트랙트와 모두 동일



 프록시 패턴 자체가 컨트랙트를 한번 거쳐서 실행되는 구조이기 때문에, 어느정도 가스 비효율이 발생할 수 밖에 없다고 했다. 비콘 프록시는 여기에 하나의 레이어를 더 추가한 격이므로, 가스 비효율이 더욱 증가하게 된다.

즉, 비콘 프록시 패턴은 대규모 업그레이드의 효율성을 위해 일상의 비효율을 어느정도 감수하는것을 선택한 것이다. 그렇다면 비콘 프록시는 언제 사용해야 잘 사용한걸까? 그 답은 매우 간단하다.

**동일한 로직**을 사용하는 **대규모의** 컨트랙트를 **한번에 업그레이드** 해야하는 경우 사용하면 된다.

- **동일한 로직**을 사용하는 컨트랙트를 **대규모로 배포**하고 **동시에 업그레이드 가능**하게 하려면 **비콘 프록시**를 사용하자.
- **비콘 프록시는 만능이 아니다.** 기존 프록시 패턴에 비해 컨트랙트 호출을 위해 거쳐야 하는 컨트랙트가 하나 더 추가된다. 즉, 트랜잭션 실행을 위한 가스 부담이 증가하게 되므로, 반드시 목적에 맞게 잘 사용해야 한다.





### Minimal Proxy

***업그레이드 없이 동일한 로직의 컨트랙트를 대규모로 효율적으로 배포\****하는것 자체에만 관심이 있다면,* 해답으로 나온것이 바로 [**EIP-1167: Minimal Proxy Contract**](https://eips.ethereum.org/EIPS/eip-1167)**, 즉 미니멀 프록시 컨트랙트이다.**



미니멀 프록시의 관심사는 오직 **컨트랙트 배포의 최적화**에 있다. 프록시라는 이름이 붙었지만, 이는 오직 로직의 재사용만을 위한 목적일 뿐, 여타 다른 프록시 패턴과 달리 업그레이드 기능이 필요하지 않다. 따라서, 미니멀 프록시는 컨트랙트의 효율적 배포만을 위해 최소한으로 구현된 컨트랙트 패턴이라는 의미이다.



미니멀 프록시의 기능은 단 하나이다. 바로 배포시에 정해진 컨트랙트 주소로만 `delegatecall`

오직 forward() 함수만 구현된다. `forward()` 함수의 기능도 매우 단순하다. 모든 컨트랙트 호출을 `IMPLEMENTATION_ADDR` 주소의 컨트랙트로 `delegatecall` 하는 것이다. 미니멀 프록시 컨트랙트에서 필요한 기능은 이처럼 매우 매우 단순하다.

솔리디티 코드로 작성되는것이 아닌, 동일한 기능을 수행하는 EVM 바이트코드로 구현



---



Delegating **proxy contracts** are widely used for both upgradeability and gas savings. These proxies rely on a **logic contract** (also known as implementation contract or master copy) that is called using `delegatecall`. This allows proxies to keep a persistent state (storage and balance) while the code is delegated to the logic contract.

To avoid clashes in storage usage between the proxy and logic contract, the address of the logic contract is typically saved in a specific storage slot guaranteed to be never allocated by a compiler.





Solidity maps variables to storage based on the order in which they were declared, after the contract inheritance chain is linearized: the first variable is assigned the first slot, and so on. The exception is values in dynamic arrays and mappings, which are stored in the hash of the concatenation of the key and the storage slot



https://eips.ethereum.org/EIPS/eip-1967



---





why subtract 1

as there's no known input that would result in this value if it was hashed

Furthermore, a `-1` offset is added so the preimage of the hash cannot be known, further reducing the chances of a possible attack.

https://stackoverflow.com/questions/75366863/why-in-solidity-proxies-from-positions-index-subtract-one

---

Upgradeable smart contracts use three contracts: Proxy, Implementation, and ProxyAdmin. This pattern enables iterative releases and patching of source code.





https://docs.alchemy.com/docs/upgradeable-smart-contracts



---



 initializer functions following the naming convention `__{ContractName}_init`. Since these are internal, you must always define your own public initializer function and call the parent initializer of the contract you extend.







---

**컨트랙트 구조**

우선, 이더리움 계정이 어떠한 구조를 가지는 지부터 살펴보겠다. 이더리움 계정들은 다음 4의 필드를 가진다.

- nonce :  A counter that indicates the number of transactions sent from an externally-owned account or the number of contracts created by a contract account.
- balance : he number of wei owned by this address
- code hash : refers to the *code* of an account on the Ethereum virtual machine 
- storageRoot(storage hash)

`codeHash` – This hash refers to the *code* of an account on the Ethereum virtual machine (EVM). Contract accounts have code fragments programmed in that can perform different operations. This EVM code gets executed if the account gets a message call. It cannot be changed, unlike the other account fields. All such code fragments are contained in the state database under their corresponding hashes for later retrieval. This hash value is known as a codeHash. For externally owned accounts, the codeHash field is the hash of an empty string.

`storageRoot` – Sometimes known as a storage hash. A 256-bit hash of the root node of a Merkle Patricia trie that encodes the storage contents of the account (a mapping between 256-bit integer values), encoded into the trie as a mapping from the Keccak 256-bit hash of the 256-bit integer keys to the RLP-encoded 256-bit integer values. This trie encodes the hash of the storage contents of this account, and is empty by default.

https://ethereum.org/en/developers/docs/accounts/



