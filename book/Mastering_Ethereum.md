

# 마스터링 이더리움



## 1. 이더리움이란 무엇인가?



### 개요

- 이더리움은 스마트 컨트랙트라는 프로그램을 실행하는 오픈 소스에 기반을 둔, 전 세계에 걸쳐 탈중앙화된 컴퓨팅 인프라스트럭처

- 이더(Ether)라고 하는 암호화폐를 이용하여 실행 자원 비용을 측정하고 제한
  - 이더의 사용 목적은 월드 컴퓨터로서의 이더리움 플랫폼 사용료를 지불하기 위한 유틸리티 화폐

- 임의성과 무한 복잡성을 가진 코드를 실행할 수 있는 **가상 머신**을 운영하는 범용 프로그래밍

- **튜링 완전** 언어



### 비교

- vs 비트코인

  - 비트코인

    - 트랜잭션이 **상태 전이**(state transition)를 일으켜 코인의 소유권을 변경하는 탈중앙화된 합의 **상태 머신**(state machine)
    - 화폐 소유 상태만 추적

  - 이더리움

    - 탈중앙화된 합의 **상태 머신**(state machine)

    - 범용 데이터 저장소 **키-밸류 튜플**로 표현할 수 있는 모든 데이터를 저장

      => 상태 머신에 코드를 로드하고 그 코드를 실행하고 그 결과 상태를 저장할 수 있음

- 일반적인 범용 컴퓨터와의 차이

  - 이더리움의 상태 변화가 합의 규칙에 의해 관리
  - 상태가 전체적으로 배포



### 이더리움의 구성요소

- 피어투피어 네트워크 (P2P network)
- 합의 규칙
- 트랜잭션
  - 보낸 사람, 받는 사람, 값, 데이터 페이로드가 포함된 네트워크 메시지
- 상태 머신
  - 이더리움 상태 전이는 **바이트코드를 실행**하는 **EVM**(Ethereum Virtual Machine)에 의해 처리
- 데이터 구조
  - 머클 패트리샤 트리라고 하는 시리얼라이즈된 해시 데이터 구조
  - 각 노드의 데이터베이스에 저장
- 합의 알고리즘
- 경제적 보안성 (economic security)
- 클라이언트
  - 클라이언트 소프트웨어를 상호운용할 수 있는 몇 가지 구현체 존재
  - Geth, Parity



### 이더리움과 튜링 완전

- 튜링 완전 정의

  - 튜링은 순차적 메모리에서 기호를 읽고 쓰는 방식으로 기호를 조작하는 상태 머신으로 구성된 컴퓨터의 수학적 모델 만듬

  - 튜링 머신을 시뮬레이션하는 데 사용할 수 있다면, 시스템이 튜링 완전하다고 정의

- 기능으로서의 튜링 완전
  - 튜링 완전은 이더리움이 어떠한 복잡한 프로그램이라도 계산할 수 있음을 의미

​	 => 이러한 유연성은 보안과 자원 관리에 몇 가지 어려운 문제를 야기. (응답이 없는 시스템에 대한 재부팅 불가)

- 튜링 완전의 함축적 의미

  - 프로그램을 실행하지 않고서는 프로그램의 경로를 예측할 수 없다. 튜링 완전 시스템은 무한 루프에서 실행될 수 있다

  - 노드가 유효성 검사를 시도할 때 우연히 또는 의도적으로 스마트 컨트랜트가 영원히 지속하도록 만들 수 있음. 사실서 서비스 거부 공격 (Denial Of Service)

​	=> 스마트 컨트랙트가 사용하는 자원 제한 필요 => **가스**(gas)라는 과금 매커니즘 도입



### 가스

- 가스는 각 명령어의 비용을 일일이 계산

- 스마트 컨트랙트를 실행하는 데 사용할 수 있는 가스의 최대 사용량을 가지고 있어야 한다.

- 계산에 소비되는 가스의 총량이 트랜잭션에서의 가스 가용량을 초과한다면 EVM은 실행을 중지

  => 리소스 제한

- 트랜잭션의 부분 구성요소로서만 구매
  - 이더로만 살 수 있음





### 탈중앙화 애플리케이션 (DApp)

- 최소 구성 요소로서의 댑
  - 스마트 컨트랙트 + 웹 사용자 인터페이스

- 큰 의미로서의 댑
  - 공개되고 탈중앙화된 피어투피어 기반 서비스 위에 제공되는 웹 애플리케이션



:bulb: 스마트 컨트랙트는 단순하게 업그레이드할 수 없다. 새로운 애플리케이션을 배포하고, 사용자와 앱, 자금을 이전하고 다시 시작할 준비가 되어야 한다.





## 2. 이더리움 기초



### 이더 단위

- 이더리움의 화페 단위는 이더(ether)

  - 이더리움은 시스템, 이더가 화폐

- 이더는 웨이(wei)라는 가능한 가장 작은 단위까지 내려감

  - 1이더 = 10^18wei

  - 이더의 가치는 항상 이더리움 내부에서 웨이로 표시된 부호 없는 정숫값으로 표현



### 이더리움 지갑

- 이더리움 계정을 관리하는데 도움이 되는 소프트웨어 애플리케이션이라는 의미로 지갑이라는 용어 사용

- 지갑은 사용자의 키를 보유하고, 사용자를 대신해 트랜잭션을 생성하고, 브로드캐스트 O



### 통제와 책임

- 이더리움의 각 사용자가 자금 및 스마트 컨트랙트에 대한 접근을 제어하는 자체 개인키를 관리하고 제어할 수 있어야 한다는 것이다.

- 기본 원칙은 하나의 개인키가 하나의 계정과 동일



:bulb: 새 계정을 만들 때 새 주소로 작은 테스트 트랜잭션만 보내는 것으로 시작하라



### 외부 소유 계정(EOA) 및 컨트랙트

- 외부 소유 계정(Externally Owned Account, EOA)
  - 개인키가 있는 계정. 자금 또는 컨트랙트에 대한 접근 제어
  - 트랜잭션을 시작(initaite)할 수 있음

- 컨트랙트 계정(contract account) 

  - 단순한 EOA가 가질 수 없는 스마트 컨트랙트 코드가 있음
    - 스마트 컨트랙트 코드는 계정 생성 시 이더리움 블록체인에 기록되고 EVM에 의해 실행되는 소프트웨어 프로그램
  - 개인키가 없음. 대신, 스마트 컨트랙트의 코드의 로직으로 제어
  - 스마트 컨트랙트 실행
    - EOA와 마찬가지로, 주소가 있으며, 이더를 보내고 받을 수 있음. 
    - 트랜잭션 목적지가 컨트랙트 주소일 때 트랜잭션과 트랜잭션 데이터를 입력으로 사용하여 컨트랙트가 EVM에서 실행됨
      - 실행할 컨트랙트의 특정 함수와 해당 함수에 전달할 파라미터를 나타내는 데이터(data)가 포함될 수 있음
      - 트랜잭션은 컨트랙트 내의 함수를 호출(call)할 수 있음
  - 컨트랙트는 다른 컨트랙트를 호출해서 컨트랙트에 반응(react)할 수 있다.

  - 등록

    - 블록체인에 컨트랙트를 등록하는 것은 목적지 주소가 제로 어드레스인 특수 트랜잭션을 만드는 것이다. 
      - 제로 어드레스는 컨트랙트를 등록하고자 하는 이더리움 블록체인에 알리는 특별한 주소

    - 컨트랙트가 블록체인에 생성되면 이더리움 주소를 갖게 된다. 누군가가 컨트랙트 주소로 트랜잭션을 보낼 때마다 그 트랜색션을 입력값으로 하여 컨트랙트가 EVM에서 실행된다.





## 4. 암호학



이더리움 기반 기술 중 하나는 암호학



### 키와 주소

- 개인키

  - 개인키는 단순히 무작위로 선택한 숫자
  - 키를 생성하는 가장 중요한 첫 번째 단계는 엔트로피, 무작위성을 확보
  - 계정에 대한 제어권 제공
    - 개인키는 비공개로 유지

- 공개키

  - 이더리움 공개키는 개인키로부터 '단방향으로만' 계산할 수 있음

  - 개인키가 있는 경우 공개키를 계산하기는 쉽지만, 공개키에서 개인키를 계산할 수는 없음
  - 공개키는 타원 곡선 암호화를 통해 생성
  - 계정을 식별

- 계정 주소

  - 개인키에서 직접 파생, 개인키는 **계정**이라고도 불리는 단일 이더리움 주소를 고유하게 결정
  - 이더리움 주소는 해시 함수를 사용하는 공개키 또는 컨트랙트에서 파생한 고유 식별자

  - 모든 이더리움 주소가 공개키-개인키 쌍으로 나타나진 않음. 개인키로 뒷받침되지 않는 컨트랙트를 표시 O



### 공개키 암호화와 암호화폐

- 공개키 암호화
  - 공개키 암호화는 고유한 키를 사용하여 정보를 보호

  - 트랩 도어 함수 : 역산하기 위한 단축키로 사용할 수 있는 비밀 정보가 없으면 거꾸로 계산하기 어려움
  - 서명과 검증
    - 공개키 암호는 고유성을 가진 개인키를 이용해서 서명을 작성하는 행위(Sign)가 가능
    - 개인키로 한 행위에 대해서 상대방이 검증할 수 있는 과정도 필요하기 때문에 공개가 가능한 공개키는 누구나 검증(Verify) 할 수 있도록 공개

- 디지털 서명
  - 디지털 개인키, 이더리움 주소, 디지털 서명을 통해 외부 소유 계정의 이더 소유권을 확립
  - 디지털 서명을 통해 자금의 접근과 통제가 이루어짐
  - 이더리움 트랜잭션은 유효한 디지털 서명이 블록체인에 있어야 함
  - 디지털 서명은 개인키의 소유권을 증명
    - 개인키를 가진 누구나 해당 계정과 해당 계정이 가진 이더를 제어

- 이더리움 트랜잭션 검증

  - 트랜잭션은 기본적으로 특정 이더리움 주소로 특정 계정에 접근하는 요청

    - 이더리움 트랜잭션에선 트랜잭션 자체의 세부사항이 메시지로 사용. 
    - 암호 수학은 메시지를 개인키와 결합하여 개인키를 알아야만 만들 수 있는 코드 생성 방법을 제공 : 디지털 서명

    - 주소에 해당하는 개인키로 생성된 디지털 서명도 함께 보내야 함

  - 타원 곡선 수학이란, 디지털 서명, 트랜잭션 세부 정보, 접근하는 이더리움 주소가 일치하는 확인하여, 트랜잭션이 유효한지 확인할 수 있음을 의미
    - 검증 프로세스에서는 그 트랜잭션이 공개키에 대응되는 개인키에 의해 만들어졌음을 확증할 수 있음



### 암호화 해시 함수

- 해시 함수

  - 임의 크기의 데이터를 고정된 크기의 데이터로 매핑하는데 사용할 수 있는 모든 함수

  - 해시 함수에 대한 입력을 사전 이미지, 메시지, 입력 데이터라고 함

  - 그 결과를 해시라고 함

- 암호화 해시 함수의 주요 속성

  - 결정론 (determinism)
    - 주어진 입력 데이터는 항상 동일한 해시 결과를 생성
  - 검증성 (verifiability)
    - 메시지의 해시 계산은 효율적이다 (선형 복잡성)
  - 비상관성 (noncorrelation)
    - 메시지에 대한 작은 변화는 해시 출력을 너무 광범위하게 변경해야 해서, 원본 메시지의 해시와 상관 관계가 없다
  - 비가역성(irreversibility)
    - 해시로부터 메시지를 계산하는 것은 불가능

  - 충돌 방지 (collision protection)
    - 같은 해시 결과를 생성하는 2개의 서로 다른 메시지를 계산하는 것은 불가능


- 특성

  - 단방향 특성은 입력 데이터를 다시 작성하는 것이 계산적으로 불가능함을 의미

  - 동일한 결과에 해시 처리한 두 입력 데이터 집합을 찾는 것을 해시 충돌





## 5. 지갑



### 지갑

- 개요

  - 넓은 의미
    - 이더리움의 주요 사용자 인터페이스를 제공하는 소프트웨어 애플리케이션

  - 개발자 시작
    - 사용자의 키를 보관하고 관리하기 위해 사용되는 시스템
    - 모든 지갑은 키 관리 구성요소 有

- 지갑 기술의 개요

  - 설계 시 중요 고려사항

    - 편의성과 프라이버시 사이의 균형 맞추기

      - 극단적 편의성 : 하나의 개인키와 주소를 가지고 이를 재사용해서 모든 것을 처리

        => 프라이버시에 대한 악몽 可

      - 극단적 프라이버시 : 모든 트랜잭션에 새로운 키를 사용

        => 관리하기가 몹시 어렵

- 키체인

  - 지갑은 단지 키만 보유 이더 혹은 다른 토큰은 이더리움 블록체인에 기록
  - 이더리움 지갑은 개인키와 공개키 쌍을 포함하는 키체인과 같음

- 유형

  - 비결정적 지갑(nondeterministic wallet)

    - 각기 다른 무작위 수로부터 각각의 키를 무작위적으로 추출

  - 결정적 지갑(deterministic wallet).

    - 모든 키가 시드(seed)라고 하는 단일 마스터 키로부터 파생

    - 모든 키는 서로 관련이 있고, 원래의 시드를 갖고 있다면 다시 키를 파생시킬 수 있음

    - 여러 키 파생 방식 존재

    - 니모닉 코드 단어(mnemonic code words)로 시드 생성

      - 데이터 분실 위험에 따른 더 안전한 결정적 지갑 필요

        => 시드를 단어 목록으로 인코딩하여 생성

      - 복구 단어 목록은 아주 조심스럽게 다루고 절대로 컴퓨터나 휴대전화의 전자파일로 저장하지 말고 종이에 적어서 안전한 곳에 보관 권고



### 비결정적(무작위) 지갑

- 개요

  - 자금을 받을 때마다 새로운 주소(새로운 개인키가 필요)를 사용

    - 이더리움 주소의 재사용을 피하는 것이 좋은 지침으로 간주됨

  - 정기적으로 키 목록을 증가

    => 정기적인 백업이 필요

- 단점

  - 백업 전 데이터를 잃어버리면 자금과 스마트 컨트랙트에 접근할 수 없게 됨
  - 다루기 어려움

:memo:  **키저장소(keystore)**

- 이더리움 클라이언트는 암호문으로 암호화된 단일(무작위 생성된) 개인키가 들어 있는 JSON 인코딩 파일인 키저장소 사용

- 키저장소 형식은 무차별(brute-force), 사전(dictionary), 레인보우 테이블(rainbow table) 공격을 대비해 암호 확장 알고리즘으로 알려진 키 파생 함수(key derivation function, KDF) 사용

:bulb: 간단한 테스트 외에는 비결정적 지갑은 사용 권장 X





### 결정적 지갑

- 개요

  - 단일 마스터 키 또는 단일 시드로부터 파생된 개인키를 포함

  - 시드는 모든 파생된 키를 복구할 수 있다.

- **HD 지갑**
  - 개요
    - HD 지갑은 BIP-32 표준으로 정의된 결정적 지갑
    - 트리 구조로 파생된 키들을 가지고 있음
  - 장점
    - 트리 구조
      - 분기별 다른 용도 지정이 가능하여 구조적인 의미를 표현 O
    - 개인키에 접속하지 않고 사용자가 공개키 시퀀스 생성 O
      - HD 지갑은 보안상 안전하지 않는 서버, 보기 전용, 수신 전용의 용도로 사용 O
      - 자금을 움직이는 개인키를 들어 있지 않게 가능

- 시드와 니모닉 코드(BIP-39)

  - 니모닉
    - 올바른 순서로 단어 시퀀스가 입력되면 고유한 개인키를 다시 만들 수 있음
    - BIP-39에 의해서 표준화

  - 장점
    - 다루기 쉬움
      - 실용적인 측면에서, 16진수 시퀀스를 기록할 때에는 오류가 발생한 확률이 매우 높음
      - 단어 목록은 중복성이 커서 다루기가 매우 쉬움

  - 보안
    - 시드는 종이에 써서 보관할 것을 추천

- 지갑의 모범 사례

  - 유연한 지갑을 만들기 위한 일반적인 사업 표준 등장

    - BIP-39 : 니모닉 코드 단어

    - BIP-32 : HD 지갑

    - BIP-43 : 다목적 HD 지갑 구조

    - BIP-44 복수화폐 및 복수계정 지갑



### 니모닉 코드 단어(BIP-39)

- 용도

  - 니모닉 코드 단어는 결정적 지갑을 파생하기 위해 시드로 사용되는 난수를 인코딩하는 단어 시퀀스

  - 단어 시퀀스는 시드를 다시 만들어내고, 파생된 키들을 재생성할 수 있다.

- 선택적 암호문

  - BIP-39 표준은 시드의 표준에 선택적 암호문을 사용할 수 있음

  - 암호문의 유무 및 암호문의 값을 어떻게 입력하냐에 따라서 각기 다른 시드를 생성

  - 본질적으로 잘못된 암호문은 없음

    - 모든 암호문은 유효하며, 각각 다른 시드를 만듬
    - 가능한 한 초기화되지 않은 많은 지갑을 형성

  - 특징

    - 니모닉 자체만으로는 의미가 없어짐. 니모닉 도난으로부터 보호
      - 협박 받았을 시, 그럴 듯한 가짜 암호문을 제공

    - 암호문의 사용은 손실의 위험 또한 동반
      - 니모닉과 암호문 둘 중 하나만 분실해도 계정 및 자금을 잃어버림
      - 소유자가 암호문을 시드와 동일한 위치에 백업하는 것은 2차 팩터의 목적에 어긋남

​		=> 복구할 수 있는 가능성을 고려해, 신중한 계획 필요



### HD 지갑(BIP-32)과 경로(BIP-43/44)

- 키의 확장

  - 개요
    - BIP-32의 용어로, 키는 확장(extended)될 수 있음
    - 적절한 수학적 연산을 사용하여 확장된 부모키는 자식키를 파생시킬 수 있게 되고, 키와 주소의 계층 구조를 만들 수 있게 됨

  - 체인코드

    - 키를 확장하는 것은 키 자체를 가져와서 특수 **체인 코드**를 추가하는 것
    - 체인 코드는 자식키를 생성하기 위해 각 키와 혼합된 256비트 이진 문자열이다. 

    - 활용

      - 접두어 xprv  => **확장된 개인키**

      - 접두어 xpub => **확장된 공개키**는

- 공개키 생성
  - 부모 공개키에서의 자식 공개키 파생
    - HD 지갑의 매우 유용한 특징으로 부모 공개키에서 자식 공개키를 파생할 수 있음
    - 매우 안전한 공개키 전용 배포 만드는데 사용 O
    - 단지 그 주소로 보낸 돈은 쓸 수 없음
  - 자식 개인키로부터 직접 파생하는 방법과 부모 공개키로부터 직접 파생하는 방법
    - 확장된 공개키는 해당 분기에서 모든 공개키(단지 공개키들만)를 파생하는 데 사용될 수 있다.

- 강화된 자식 키의 파생

  - 확장된 공개키에서의 공개키 분기는 잠재적 위험 존재
    - xpub이 체인 코드를 포함하므로, 만약 하위 개인키가 유출될 경우, 다른 모든 자식 개인키를 파생 O
  - **강화 파생**
    - 부모 공개키와 자식 체인 코드 간의 관계를 끊는 대체 가능 파생 함수
    - 강화 파생 함수는 자식 체인 코드를 파생하기 위해 부모 공개키 대신에 **부모의 개인키를 사용**

  - 보안 권고 사항

    - 유출된 체인 코드의 위험에 노출되지 안고 편리하게 xpub을 이용해 공개키의 분기를 파생하기 위해서는 일반적인 부모가 아닌 강화된 부모로 공개키 분기를 파생

    - 마스터 키의 유출을 방지하기 위해서는 항상 강화 파생으로 파생된 마스터 키의 1단계 자식 사용을 강력히 추천

- 인덱스 번호

  - 개요

    - 여러 개의 자식 키를 파생

    - 관리하기 위해 인덱스 번호 사용

    - 각 인덱스 번호는 부모 키와 결합될 때 각각 다른 자식 키를 만들어냄

  - 분리
    - 0 ~ 2^31-1 : 일반 파생용 인덱스
    - 2^31 ~ 2^32-1 : 강화 파생용 인덱스

  - 표기
    - 다만, 인덱스 번호를 좀 더 읽기 쉽도록 표시하기 위해, 강화 인덱스도 0부터 시작하는 것으로 표현(대신 `'` 붙임) 
    - ex) 첫 번째 강화된 자식(0x80000000)은 `0'`, 두 번째 강회된 자식(0x80000001)은 `1'`으로 표기

- HD 지갑 키 식별자(경로)

  - HD 지갑의 키는 경로 이름 규칙을 사용하여 식별, 트리의 각 레벨은 슬래시(/) 문자로 구분

  - 마스터 개인키에서 파생된 개인키는 m으로 시작하며, 마스터 공개키에서 파생된 공개키는 M으로 시작됨

| HD 경로     | 키 설명                                                      |
| ----------- | ------------------------------------------------------------ |
| m/0         | 마스터 개인키(m)의 첫 번째(0) 자식 개인키                    |
| m/0/0       | 첫 번째 자식(m/0)의 첫 번째 자식 개인키                      |
| m/0'/0      | 첫 번째 강화된 자식(m/0')의 첫 번째 일반 자식                |
| m/1/0       | 두 번째 자식(m/1)의 첫 번째 자식 개인키                      |
| M/23/17/0/0 | 24번째 자식의 18번째 자식의 첫 번째 자식의 첫 번째 자식 공개키 |



### HD 지갑 트리 구조 탐색

- 개요

  - HD 지갑 트리 구조는 대단히 유연함. 무한한 복잡성을 허용

    - 각 부모의 확장 키는 40억 개의 자식을 가질 수 있음

    - 그 자식들은 각각 40억개의 자식들을 가질 수 있음

    => 무한한 세대가 될 수 있음

- BIP-43 / BIP-44

  - HD 지갑의 표준을 만들어 잠재적인 복잡성을 관리할 수 있는 방법을 제공

  - BIP-43

    - 강화된 첫 번째 자식 인덱스를 트리 구조의 목적을 나타내는 특수 식별자로 사용하도록 제안

    - 트리의 구조와 나머지 레벨의 네임스페이스를 식별 => 지갑의 목적을 정의하는 인덱스 번호와 함께 트리 레벨 1 분기만 사용

  - BIP-44

    - 개요

      - 확장된 사양 제공
      - 목적 번호를 44'로 설정하여, 복수화폐 복수계정 구조를 제안

    - 구조

      - BIP-44를 따르는 모든 HD 지갑 구조는 하나의 트리 분기(m/44'/*)만을 사용

      - 다섯 가지 트리 레벨로 구성된 구조를 지정

        ```
        m/ purpose' / coin_type' / account' / change / address_index
        ```

        - `purpose'`
          - 항상 `44'`로 설정
        - `coin_type'`
          -  암호화폐의 동전의 유형
          - 이더리움 : `60'`
          - 비트코인 : `44'`
        - `account'`
          - 회계 또는 조직 목적을 위한 별도의 논리적 하위 계좌로 세분화
        - `change`
          - 비트코인을 위한 필드로 특이점(quirk) 포함
          - 2개의 하위 트리
            - 입금 주소 작성용 : 0
            - 잔액 주소 작성용 : 1 ??
          - 이더리움은 단지 입급 경로만 사용 (0 고정)
          - 일반 파생 사용
        - address_index
          -  인덱스 값

    - ex) `M/44'/60'/0'/0/2`
      - 주 메인 계정에서 이더리움 지급을 위한 세번째 입금 주소





<<<<<<< HEAD
## 6. 트랜잭션



### 트랜잭션

- 개요
  - 외부 소유 계정(EOA)에 의해 서명된 메시지
    - 네트워크에 의해 전송되고 이더리움 블록체인에 기록됨
  - EVM에서 상태 변경을 유발하거나 컨트랙트를 실행할 수 있는 유일한 방법
  - 이더리움 = 글로벌 싱글톤 상태 머신 & 트랜잭션은 상태 머신을 움직여서 상태를 변경
    - 모든 것은 트랜잭션으로부터 시작

- 트랜잭션 구조

  - 논스(nonce)
    - 메시지 재사용을 방지하는 데 사용되는 일련번호
  - 가스 가격(gas price)
    - 발신자가 지급하는 가스의 가격
  - 가스 한도(gas limit)
    - 이 트랜잭션을 위해 구입할 가스의 최대량
  - 수신자(recepient)
    - 목적지 이더리움 주소
  - 값(value)
    - 목적지에 보낼 이더의 양
  - 데이터(data)
    - 가변 길이 바이너리 데이터 페이로드

  - v,r,s
    - EOA의 ECDSA 디지털 서명의 세 가지 구성요소

  :bulb: 내부 정보를 보여주거나 사용자 인터페이스를 시작화하기 위해서는 트랜잭션 구조 이외에도 트랜잭션이나 블록첸인에서 파생된 추가 정보를 사용 ex) v,r,s 구성요소 => 발신자 정보





### 트랜잭션 논스

- 개념
  - 해당 주소에서 보낸 **트랜잭션 건수** 또는 연결된 코드가 있는 계정의 경우 **컨트랙트 생성 건수**와 동일한 스칼라 값

- 특징

  - 발신 주소의 속성
    - 발신 주소의 컨텍스트 안에서만 의미 가짐

- 중요성

  - 트랜잭션의 생성 순서대로 포함되다는 점에서 생기는 사용성상의 기능

    - 특정 노드가 다른 노드보다 특정한 트랜잭션을 먼저 받을 것이라는 보장 X => 논스로 순서 보장

  - 트랜잭션 복제 방지

    - 이더리움 네트워크에서 트랜잭션을 보는 사람은 원래 트랜잭션을 복사하여 붙여넣고 네트워크로 다시 보내는 방식으로 여러분의 이더가 소진될 때까지 계속 트랜잭션을 반복해서 재실행 가능 

      => 논스 값을 사용하면 **각각의 개별 트랜잭션은 고유**해짐

- 논스 추적

  - 지갑은 그 지갑에서 관리하는 각 주소에 대한 논스를 추적

  - 새 트랜잭션을 만들 때 시퀀스상 다음 차례 논스 값 부여

    - 컨펌될 때까지는 `getTransactionCount` 합계에 포함되지 않음

    - 미해결 트랜잭션이 모두 확인된 경우에만 `getTransactionCount`의 출력을 신뢰 O

      =>다음 논스 파악 O

- 논스의 간격, 중복 논스 및 확인
  - 논스에 따라 트랜잭션을 순차적으로 처리
  - 누락된 논스가 나타날 때까지 기다리는 동안 트랜잭션은 멤풀(mempool)에 저장됨
  - 모든 노드는 누락된 논스가 단순히 지연되었고, 순서가 맞지 않게 수신되었다고 가정
  - 유효하지 않거나 가스가 모자란 트랜잭션은 논스 시퀀스에 의도치 않게 '갭'을 만들 수 있음
  - 다시 트랜잭션을 계속되게 하려면 누락된 논스가 있는 유효한 트랜잭션을 전송
  - 모든 브로드캐스트된 트랜잭션이 차례대로 유효해진다는 점도 똑같이 염두
  - 트랜잭션을 회수하는 것은 불가능

- 동시 실행
  - 개요
    - 여러 독립 시스템에 의한 동시적인 계산이 있는 경우를 의미
    - 이더리움은 동시 실행을 허용하지만 합의를 통해 싱글통 상태를 강제
  - 해결
    - 단일 프로세스를 만드는 것처럼 병목 지점을 어쩔 수 없이 받아드림
    - 독립적 처리 후 중간중간 데이터 조정



### 트랜잭션 가스

- 개요

  - 가스는 이더리움의 연료

  - 이더에 대한 자체 환율을 가진 별도의 가상 화폐

  - 자원의 양을 제어함
  - 이더와의 분리
    - 이더 가치의 급격한 변화와 함께 발생할 수 있는 변동성으로부터 시스템을 보호
    - 가스가 지급하는 다양한 자원의 비용 사이의 중요하고 민감한 비율을 관리

- gasPrice

  - 지갑은 신속한 트랜잭션 컨펌을 위해 gasPrice를 조정

    - gasPrice가 높을수록 트랜잭션이 더 빨리 컨펌

    - 우선순위가 낮은 트랜잭션은 낮은 가격을 설정해서 컨펌이 느려지게 함

  - zero price

    - 블록 공간에 대한 수요가 낮은 기간에는 수수료가 0인 트랜잭션들도 포함되기도 함
      - 단, 영원히 컨펌되지 않을 수도 있음. 

- gasLimit

  - gasLimit은 트랜잭션을 완료하기 위해 트랜잭션을 시도하는 사람이 기꺼이 구매할 수 있는 최대 가스 단위 수

- gas 청구

  - 목적지 주소가 컨트랙트인 경우, 필요한 가스양을 추정할 수는 있지만 정확하게 결정할 수는 없다.
    - 트랜잭션 첫 번째 유효성 확인 단계 중 하나는 계정이 가스 가격 X 가스 요금을 지급할 만큼 충분한 이더를 갖고 있는지 확인
    - 완료될 때까지 여러분의 계좌에서 금액이 실제로 차감되지 않음
  - 소비된 가스의 요금만 청구





### 트랜잭션 수신자

- to 필드에 트랜잭션 수신자가 지정됨

- 검증하지 않음. 모든 20바이트 값은 유효한 것으로 간주

- 트랜잭션을 잘못된 주소로 보내면, 다시 사용할 수 없는 상태가 되므로 영원히 소실된 것으로 간주됨



### 트랜잭션 값과 데이터

- 페이로드는 값과 데이터라는 2개의 필드에 포함

- 경우의 수

  - 값만 있을 경우 : 지급(payment)

  - 데이터만 있을 경우 : 호출(invocation)

  - 모두 : 지급과 호출

  - none : 가스 낭비



### EOA 및 컨트랙트에 값 전달

- to EOA 주소
  - 주소 잔액에 보낸 값을 추가
  - 데이터 페이로드를 EOA에 보낼 수 없다는 뜻은 아님
    - but, 대부분의 자갑은 지산이 제어하는 EOA에 대한 트랜잭션에서 수신된 모든 데이터를 무시

- to 컨트랙트 주소

  - 트랜잭션의 데이터 페이로드에 지정된 함수를 호출

    - 대부분의 컨트랙트에서는 데이터를 함수 호출로 사용, 명명된 함수를 호출하고 인코딩된 인수를 함수에 전달

    - ABI 호환 컨트랙트(모든 컨트랙트)로 전송된 데이터 페이로드는 다음을 16진수로 시리얼라이즈한 인코딩

      - 함수 선택기(function selector)
      - 함수 인수(function argument)

      :bulb: 함수의 프로토타입(prototype)은 함수의 이름을 포함하는 문자열로 정의되고, 각 인수의 데이터 유형이 괄호 안에 들어 있으며, 쉼표로 구분

  - 데이터가 없으면 폴백(fallback) 함수 호출
    - 폴백 함수가 없다면 지급하는 것과 마찬가지로 컨트랙트의 잔액을 늘린다



### 특별 트랜잭션 : 컨트랙트 생성

- 새로운 컨트랙트를 만들어, 제로 어드레스라고 하는 특수 대상 주소로 전송

- 컨트랙트 생성 트랜잭션은 컨트랙트를 생성할 컴파일된 바이트코드를 포함하는 데이터 페이로드만 포함하면 됨



### 디지털 서명

- 이더리움의 디지털 서명 알고리즘은 ECDSA

- 세가지 용도

  1. 인증
     - 서명은 이더리움 계정과 개인키의 소유자가 이더 지출 또는 컨트랙트 이행을 **승인**했음을 증명

  2. **부인 방지**(non-repudiation)
     - 허가의 증거는 부인할 수 없다

  3. 무결성

     - 서명은 트랜잭션이 서명된 후에는 트랜잭션 데이터가 수정되지 않았고, 트랜잭션 데이터를 **수정할 수 없음**을 증명

     

### 디지털 서명 작동 방법

- 디지털 서명은 두 단계로 구성된 수학적 체계

  1. 메시지에서 개인키를 사용하여 서명을 만드는 알고리즘

  2. 누구나 메시지와 공개키만 사용하여 서명을 검증할 수 있게 해주는 알고리즘

- 서명 확인

  - 공개키를 생성한 개인키의 소유자만이 트랜잭션에서 서명을 생성할 수 있음을 의미

  - 메시지, 서명자의 공개키 및 서명(r 및 s 값)을 가져와서 서명이 메시지와 공개키에 유효하면 `true` 반환

- 트랜잭션 서명 실습

  1. nonce, gasPrice, gasLimit, to, value, data, chainID, 0, 0의 9개 필드를 포함하는 트랜잭션 데이터 구조를 만든다

  2. RLP로 인코딩된 트랜잭션 데이터 구조의 시리얼라이즈된 메시지를 생성

  3. 이 시리얼라이즈된 메시지의 Keccak-256 해시를 계산한다.

  4. 원래 EOA의 개인키로 해시에 서명하여 ECDSA 서명을 계산한다.

  5. ECDSA 서명의 계산된 v,r,s 값을 트랜잭션에 추가한다.



### EIP-155를 사용한 원시 트랜잭션 생성

- EIP-155 단순 재생 공격 방지 표준은 서명하기 전에 트랜잭션 데이터 내부에 **체인 식별자**(chaing identifier)를 포함

  - 하나의 블록체인에 대해 생성된 트랜잭션이 다른 블록체인에서 유효하지 않다

  - 타 네트워크에서의 재생 방지



### 서명 및 전송 분리(오프라인 서명)

- 개요

  - 두 단계로 나누어 트랜잭션을 생성하고 서명 O

  - 서명된 트랜잭션이 있으면, 트랜잭션을 16진수로 인코딩하고 서명해서 네트워크에서 전송

- 서명과 전송을 분리 이유 : 보안

  - 서명하는 컴퓨터에는 잠금 해제된 개인키가 메모리에 로드되어 있어야 함

  - 전송을 수행하는 컴퓨터는 인터넷에 연결되어 있음. 이더리움 클라이언트를 실행해야 한다.

  - 두 기능이 하나의 컴퓨터에 있으면, 온라인 시스템에 개인키가 있게 되며, 위험

  =>  **오프라인 서명** (서명 및 전송 기능을 분리하여 다른 시스템에서 수행)

- 프로세스

  1. 서명되지 않은 트랜잭션을 온라인 컴퓨터에서 생성

  2. 서명되지 않은 트랜잭션을 QR 코드 또는 USB 플래시 드라이브를 통해 서명을 위한 '에어 갭' 오프라인 장치로 전송

  3. 서명된 트랜잭션을 QR 코드 또는 USB 플래시 드라이브를 통해 온라인 장치로 전송



### 트랜잭션 전파

- 개요

  - 이더리움 클라이언트는 **메시**(mesh) 네트워크를 형성하는 **피어투피어(P2P)** 네트워크에서 **노드(node)**역할을 한다.

  => 직접 연결된 다른 모든 이더리움 노드로 전송

- 과정

  - 평균적으로 이웃이라 불리는 적어도 13개의 다른 노드에 대해 연결 유지

  - 각 이웃 노드는 트랜잭션을 수신하자마자 즉시 유효성 검사

  - 타당하다는 것에 동의 => 사본 저장 및 이웃에 전파 

  - 모든 노드가 트랜잭션 사본을 가질 때까지 원래 노드에서 바깥쪽으로 물결치며 퍼짐

- 특징

  - 출처를 식별할 수 없음



### 블록체인에 기록하기

- **채굴 팜**에 트랜잭션 및 블록을 제공

- 트랜잭션을 후보 블록에 추가하고 후보 블록을 유효하게 만드는 **작업증명**을 찾으려고 시도

- 내부 상태를 변경하는 컨트랙트를 호출하여 트랜잭션은 이더리움 싱글톤 상태를 수정

- 변경사항은 트랜잭션 **영수증** 형식으로 기록



### 다중 서명 트랜잭션

- 비트코인에서는 여러 당사자가 트랜잭션에 서명할 때만 자금을 사용할 수 있는 다중 서명 지원

- 이더리움의 기본 EOA 값 트랜잭션에는 다중 서명 조항이 없음
  - 스마트 컨트랙트를 사용해 임의의 서명 제한룰을 적용





## 10. 토큰



### 토큰

- 블록체인에서의 토큰

  - 소유할 수 있고, 자산, 화폐 혹은 접근 권한 등 블록체인 기반의 추상화된 의미로 재정의

  - 다양한 용도로 사용되며, 서로 교환되거나 거래 가능
  - 사용과 소유에 대한 제한이 없어지면서, 내재 가치에 대한 한계 없어짐

- 용도

  - 화폐

  - 자원
    - 스토리지,  CPU

  - 자산

  - 접근

  - 지분

  - 투표
  - 수집
    - 디지털 혹은 물리적 수집물

  - 신원

  - 증명

  - 유틸리티

  :bulb: 여러 기능을 포함하거나 이전에 합쳐진 기능을 분리, 독립적으로 개발할 수 있음

- 속성

  - 대체성

    - 개념
      - 개별 단위가 본질적으로 서로 호환성을 가지고 있는 재화 상품의 속성
      - 단일 단위를 값이나 기능의 차이 없이 다른 토큰으로 대체할 수 있는 경우에 대체 가능하다
    - 토큰의 과거 출처를 추적 관리할 수 있다면 토큰은 완전히 대체 가능 하지 않다. 
      - 과거 출처를 추적하는 능력
        - 블랙리스트, 화이트리스트를 낳을 수 있음
        - 대체성을 줄이거나 없앰
      - 대체 가능하지 않은 토큰은 고유한 유형 또는 무형의 항목이므로 상호 교환이 불가능

  - 내재성

    - 개요

      - 일부 토큰은 블록체인에 내재적인 디지털 아이템을 나타냄
      - 디지털 자산은 합의 규칙에 의해 관리

    - 주요 특징

      - **내재적 자산을 나타내는 토큰에는 추가적인 거래상대방 위험이 없음**

        - 합의 규칙이 적용되고 개인키의 소유권 가짐

          => 중개자 필요없음

        :bulb: 거래상대방 위험

        - 거래상대방 위험은 트랜잭션에서 상대방이 자신의 의무를 이행하지 못하는 위험
        - 두 개보다 더 많은 주체들이 개입되어 있는 경우, 추가적인 거래상대방 위험이 존재

      - 외재적 자산을 내재적 자산으로 변환

        - 기존 토큰은 외재적인 것을 나타내는 데 사용
          - 별도로 법률, 관습 및 정책에 의해 관리
          - non-smart 컨트랙트에 여전히 의존
          - 추가적인 거래상대방 위험이 있음

        => 블록체인은 이를 내재적 자산으로 변환하여 거래상대방 위험을 제거

- 역할
  - 유틸리티 토큰
    - 서비스, 애플리케이션 또는 자원에 접근이 요구되는 곳에 사용
    - 오남용 시, 서비스에 대한 수용자를 낮출 수 있음
  - 지분 토큰
    - 스타트업 같은 곳의 소유권에 대한 지분을 나타내는 토큰



### 이더리움 토큰

- 개요

  - 토큰은 이더와 완전히 다른 개념. 이더리움 프로토콜은 토큰에 대해 아무것도 모름

    - 이더 잔액은 프로토콜 수준에서 처리

    - **토큰은 스마트 컨트랙트 수준에서 처리**

  - 새 토큰을 만들려면 새로운 스마트 컨트랙트를 만들어야 함
    - 배포된 스마트 컨트랙트는 소유권 이전 및 접근 권한을 포함한 모든 것을 처리
  - 토큰 처리 기능이 없는 컨트랙트에 토큰 전송 시, 영원히 사라짐

- ERC20 토큰 표준

  - 개념

    - 대체 가능한 토큰
    - 다른 단위가 상호 교환이 가능하고 고유한 특성이 없음

  - 함수 및 이벤트

    - `totalSupply`
      - 현재 존재하는 이 토큰의 전체 개수를 리턴
    - `balanceOf`
      - 해당 주소의 토큰 잔액을 반환
    - `transfer`
      - 주소와 금액이 주어지면, 주소로 토큰의 양 전송
    - `transferFrom`
      - 보낸 사람, 받는 사람, 금액이 주어지면 한 계정에서 다른 계정으로 토큰 전송
      - `approve`와 조합되어 사용
    - `approve`
      - 그 주소가 승인을 한 계정에서 최대 금액까지 여러 번 송금할 수 있도록 승인
    - `allowance`
      - 소유자 주소와 지출자 주소가 주어지면, 지출자에게 `approve`된 금액 리턴
    - `Transfer`
      - 전송이 성공하면 이벤트가 트리거됨
    - `Approval`
      - `approve`가 성공하면 이벤트가 기록

  - 선택적 함수

    - `name`
      - 사람이 읽을 수 있는 토큰의 이름 반환
    - `symbol`
      - 사람이 읽을 수 있는 기호 반환
    - `decimals`
      - 토큰 양을 나눌 수 있는 소수 자릿수 반환. ???

  - 데이터 구조

    - 2개의 데이터 구조 (data mapping으로 구현)
      1. 잔고 추적
         - 소유자별로 토큰 잔액을 내부 테이블로 구현
      2. 허용량 추적
         - 토큰 소유자가 권한을 위임자에게 위임하여 소유자의 잔액에서 특정 금액을 지출

  - 전송 워크 플로

    1. `transfer` 

       - 단일 트랜잭션인 간단한 워크플로

       - 지갑에서 다른 지갑으로 토큰을 보내는데 사용되는 워크플로

    2. `approve` & `transferFrom` 

       - 두 단계 트랜잭션 워크플로

       - 토큰 소유자가 제어를 다른 주소에 위임 할 수 있게 해줌

  - ERC20 토큰 문제

    - 토큰과 이더 자체 사이의 미묘한 차이
      - 이더는 수신자의 주소를 목적지로 가지고 있는 트랜잭션에 의해 전송
      - 토큰 전송은 특정한 토큰 컨트랙트 상태 안에서 일어나고 수신자의 주소가 아닌 토큰 컨트랙트를 목적지로 함
      - 토큰 컨트랙트는 밸런스를 관리하고 이벤트를 발생 시킴
        - 트랜잭션이 토큰 수신자에게 실제로 보내는 것이 아님
        - 받는 사람 주소가 토큰 컨트랙트 자체의 맵에 추가됨
        - 주소의 상태 변경
    - 이더를 보내거나 이더리움 컨트랙트를 사용하려면 가스를 지급하기 위해 이더가 필요함
      - 토큰을 보내는데도 이더가 필요함
      - 트랜잭션의 가스를 토큰으로 지급할 수 없으며, 토큰 컨트랙트는 가스를 지급할 수 없다.

- 여타 이더리움 토큰 표준(ERC)

|      | ERC-223                                     | ERC-721                                                      | ERC-777                                                      | ERC-1155                                                     | ERC-1400                                                     |
| ---- | ------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 개요 | 토큰 컨트랙트 인터페이스 표준               | 고유한 식별자를 갖는 불가분 토큰(Non-Fungible Token, NFT)을 만들기 위한 표준 | 향상된 토큰 표준으로서, 보안과 기능 면에서 향상된 기능을 제공 | 다중 자산을 하나의 스마트 컨트랙트에 발행하는 표준           | 보안 토큰 및 STO(보안 토큰 제의)를 위한 표준                 |
| 목표 | 실수로 토큰을 컨트랙트에 전송하는 문제 해결 | 유일한 NFT 자산을 만들고 관리하는 표준화된 방법 제공         | 토큰 보안성 및 기능성을 향상시키고, 사용자 경험을 개선하는 표준화된 방법 제공 | 단일 스마트 컨트랙트 내에서 다중 자산을 관리하는 표준화된 방법 제공 | 보안 토큰의 발행 및 관리에 필요한 표준화된 방법 제공         |
| 특성 | 목적지 주소가 컨트랙트인지 아닌지 여부 감자 | 각 토큰은 고유한 속성과 식별자를 가지며, 각 토큰은 서로 교환 가능하지 않움 | 토큰을 보유한 주소에게 자동으로 이벤트를 발생시키고, 보안 및 전송 기능을 향상시키는 등의 기능을 포함 | ERC721과 ERC20의 장점을 결합<br />=> 단일 스마트 컨트랙트 내에서 다양한 유형의 토큰을 관리 | 토큰을 발행, 이전, 소유권 확인 및 분할하는 데 필요한 기능을 제공 |
| 한계 | 널리 채택되지 않아 호환성 문제 有           | 토큰이 고유하다는 특성 때문에 대량의 토큰을 다루기에는 비효율적 | 널리 채택되지 않아 호환성 문제 有                            | 각 토큰의 고유성이 없어서 일부 사용 사례에는 적합하지 않을 수 있음 | 안 토큰을 다루기 위한 고급 기능을 제공하지만, 일반적인 토큰 사용 사례에는 복잡함 |

- 토큰 표준 사용

  - 토큰 표준이란
    - 구현을 위한 **최소** 사양

  - 목표

    - 컨트랙트 간의 **상호운용성**을 장려

    - 예측 가능한 방식으로 **인터페이스**

  - 특징

    - **규범적**(prescriptive)이라기보다는 **기술적**(descriptive)인 것을 의도

    - 구현할지 결정하는 것은 생성자에게 달려 있음
      - 내부 기능은 표준과 관련이 없다

  - 표준 사용 목적

    - 상호운용성 및 광범위한 보급이 가지는 가치

    - 해당 표준을 사용하도록 설계된 모든 시스템의 가치를 얻게 됨

- 토큰 인터페이스 표준 확장

  - 토큰 표준은 최소한의 인터페이스 제공

  - 많은 프로젝트가 애플리케이션에 필요한 기능을 제공하기 위해 확장된 구현을 만듦

  - 기능
    - 소유자 제어
    - 소각
    - 발행
    - 크라우드펀딩
    - 캡
    - 복구 백도어
    - 화이트리스트
      - 토큰 전송 등의 작업을 특정 주소로 제한
    - 블랙리스트
      - 특정 주소를 허용하지 않음으로 토큰 전송을 제한

  - 토큰 표준을 확장하는 데는 혁신/위험과 상호운용성/보안 간의 절충이 필요




## 13 이더리움 가상 머신


### EVM

- EVM이란 무엇인가

  - 스마트 컨트랙트 배포 및 실행을 처리하는 이더리움의 일부
  - 대부분의 작업 수행
    - 하나의 EOA에서 다른  EOA로의 간단한 값을 전송하는 트랜잭션은 사실상 EVM이 필요없음
    - 그 외 모든 것은  EVM에 의한 상태 업데이트를 수반

  - EVM은 자체 영구 데이터 저장소가 있는 수백만 개의 실행 가능 객체를 가진 전 세계의 탈중앙화된 컴퓨터

- EVM은 유사 튜링 완전 상태 머신

  - 가스양에 따라 모든 실행 프로세스가 유한개의 계산 단계로 제한된다는 것을 의미
  - 정지 문제 해결

- 동작 및 구성 요소

  - 동작
    - 메모리 내의 모든 값을 스택에 저장하는 스택 기반 아키텍처
    - 256  비트의 단어 크기로 동작
  - 주소지정이 가능한 여러 개의 데이터 구성요소 有
    - 바이트코드가 저장되는 불변 **프로그램 코드 ROM**
    - 모든 위치가 0으로 초기화된 휘발성 **메모리**
    - 영구 **스토리지** 
      - 0 으로 초기화됨

- 기존 기술과의 비교
  - 실행 순서가 외부에서 구성되기 때문에 스케줄링 기능이 없다
    - 클라이언트가 어떤 순서로 실행되어야 하는지를 결정
    - 단일 스레드
  - 인터페이스할 실제 물리적인 장비가 없음
    - 이더리움 월드 컴퓨터는 완전히 가상 환경

![img](https://user-images.githubusercontent.com/39115630/216258316-8d4741e6-8f50-4eef-b723-89af97e5d0f5.png)

[그림 13-1 EVM(Ethereum Virtual Machine) 아키텍처 및 실행 컨텍스트](https://user-images.githubusercontent.com/39115630/216258316-8d4741e6-8f50-4eef-b723-89af97e5d0f5.png)

- EVM 명령어 집합(바이트코드 연산)
  - 산술 및 비트 논리 연산
  - 실행 컨텍스트 조회
  - 스택, 메모리 및 스토리지 접근
  - 흐름 제어 작업
  - 로깅 호출 및 기타 연산자
  - 이 외의 계정 정보 및 블록 정보 접근



### 이더리움 상태

- EVM 작업은 이더리움 프로토콜에 정의된 대로 스마트 컨트랙트 코드의 실행 결과로 유효한 상태 변화를 계산하여 이더리움 상태를 업데이트하는 것

- 외부 주체가 트랜잭션 생성, 수락 및 주문을 통해 상태 변화를 시작한다는 사실을 반영

- 이더리움 상태 구성

  - 월드 상태(world state)
    - 가장 상위 레벨
    - 이더리움 주소를 계정(account)에 매핑
      - 각 이더리움 주소는 이더 잔액, 논스, 계정의 스토리지, 계정의 프로그램 코드를 의미
        - 이더 잔액(balance) : 웨이로 저장
        - 논스(nonce)
          - EOA => 성공적으로 전송한 트랜잭션의 수
          - 컨트랙트 계정 => 생성된 컨트랙트의 수
        - 계정의 스토리지(storage)는 스마트 컨트랙트에서만 사용하는 영구 데이터 저장소
        - 프로그램 코드는 계정이 스마트 컨트랙트 계정일 때만 존재

- EVM 실행 단계

  1. 트랜잭션이 스마트 컨트랙트 코드를 실행하면,  EVM은 생성 중인 현재 블록 및 처리 중인 특정 트랜잭션과 관련하여 필요한 모든 정보로 인스턴스화된다.

  2. EVM의 프로그램 코드 ROM에는 컨트랙트 계정 코드가 로드되고, 프로그램 카운터는 0으로 설정되며, 메모리는 모두 0으로 설정되고, 스토리지는 컨트랙트 계정의 스토리지에서 로드되어, 모든 블록 및 환경 변수가 설정된다. 

  3. 주요 변수는 이 실행을 위한 가스 공급량이며, 트랜잭션 시작 시 송금자가 지급한 가스의 양으로 설정된다. 

  4. 코드 실행이 진행되면서, 실행된 작업의 가스 비용에 따라 가스 공급량이 감소한다. 

  5. 가스 부족 예외의 경우,

     1. 어떤 시점에서 가스 공급량이 0으로 감소하면 '가스 부족' 예외가 발생하고, 실행 즉시 중단되고 트랜잭션이 중단된다. 

     2. 이더리움 상태는 변경되지 않으며, 단지 송금자의 논스가 증가되고 이더 잔액이 정지 지점까지 코드를 실행하는 데 사용된 자원에 대한 블록의 수혜자에게 지급하기 위해 줄어든다.
     3. 이 시점에서 EVM은 이더리움 월드 상태의 샌드박스 사본에서 실행되고 있다고 생각할 수 있다.
     4. 어떤 이유로든 실행을 완료할 수 없는 경우 이 샌드박스 버전은 완전히 삭제된다. 

  6. 실행이 성공적으로 완료된 경우, 실제 상태가 호출된 컨트랙트의 저장 데이터 변경, 생성된 새로운 컨트랙트 및 시작된 모든 이더 잔액 전송을 포함하여 샌드박스 버전과 일치하도록 업데이트된다.



### 튜링 완전성과 가스

- 개요

  - 어떤 종류의 프로그램이라도 실행할 수 있다면 그 시스템 또는 프로그래밍 언어는 **튜링 완전**이다.

    - 단. 일부 프로그램은 영원히 실행될 수 있다
    - 사전에 종료 여부를 알 수 없다

    => **정지 문제**

- 유사 튜링 완료 머신

  - 미리 지정된 최대 계산량을 수행한 후에 실행이 종료되지 않는다면 프로그램 실행이 EVM에 의해 중단
  - 한계가 주어져 있기 때문에 가스를 너무 많이 소비하는 트랜잭션은 중단

  => 정지 문제 해결

- 가스

  - 개요

    - 가스는 이더리움 블록체인에서 작업을 수행하는 필요한 계산 및 스토리지 자원을 측정하는 이더리움의 단위
    - 가스는 이더리움의 (휘발성) 가격과 채굴자에 대한 보상 버퍼 역할과 DoS(Denial-of-Service) 공격에 대한 방어 수단 역할을 한다

  - 실행 중 가스 계산

    1. 트랜잭션을 완료하는 데 EVM이 필요한 경우,  첫 번째 인스턴스에는 트랜잭션의 가스 한도로 지정된 양과 동일한 가스 공급량이 제공

    2. 모든 연산노드에는 가스가 소비되므로 EVM이 프로그램을 단계별로 진행함에 따라 EVM의 가스 공급량이 감소

    3. 작업을 수행하기 전에 필요한 비용을 지급할 만큼 충분한 가스가 있는지 확인
    4. 가스가 충분하지 않으면 실행이 중지되고 트랜잭션이 원상태로 되돌아간다.
    5. 성공적, 사용된 가스 비용이 채굴자에게 지급되고 트랜잭션에 지정된 가스 가격에 따라 이더로 변환된다.
    6. 남아 있는 가스는 발신자에게 돌려주며, 트랜잭션에서 지정된 가스 가격에 따라 다시 이더로 변환

  - 가스 없음 예외
    - 가스를 모두 사용하면 작업이 즉시 종료되어 가스 없음 예외가 발생
    - 트랜잭션이 되돌려지고 상태에 대한 모든 변경사항이 롤백
    - 채굴자가 이미 그 시점까지 전산 작업을 수행
    - 수수료가 부과되고 보상을 해야 함

  - 가스 비용 대 가스 가격

    - 가스 **비용**은 EVM에 사용되는 계산 및 스토리지의 척도로, 특정 작업을 수행하는 데 필요한 가스 단위 수
    - 가스 **가격**은 가스 단위당 지급하고자 하는 이더의 양
    - 발신자는 각 가스 단위에 대해 가스 가격을 지정하여 시장이 이더 가격과 컴퓨팅 운영 비용 간의 관계를 결정할 수 있게 함
    - 채굴자들은 펜딩되어 있는 트랜잭션들 중에 더 높은 가스 가격을 지불하려는 트랜잭션을 선택할 수 있음
    - 더 높은 가스 가격을 제공하면 채굴자에게 트랜잭션을 포함하고 더 빨리 확인하도록 유도

  - 네거티브 가스 비용

    - 컨트랙트 실행 중에 사용된 가스 중 일부를 환불함으로써 사용된 저장 변수 및 계정을 삭제하도록 권장

    - 음의 가스 비용이 드는 두 가지 작업

      - 컨트랙트를 삭제 => 24,000 가스의 환급  가치
      - 0이 아닌 값에서 0으로 저장 주소를 변경 => 15,000 가스의 환급 가치

      :bulb: 악용을 피하기 위해 최대 환불액은 사용된 총 가스양의 반으로 설정

  - 블록 가스 한도

    - 개요
      - 블록 가스 한도는 블록의 모든 트랜잭션에서 소비될 수 있는 가스의 최대량
      - 한 블록에 들어갈 수 있는 트랜잭션 건수를 제한
      - 채굴자가 현재 블록 가스 한도를 초과하는 가스가 필요한 트랜잭션을 포함하려 한다면, 그 블록은 네트워크에 의해 거절됨

    - 블록 가스 한도는 누가 결정하는가
      - 채굴자들은 블록 가스 한도를 집합적으로 결정
      - 채굴자가 가스 한도에 투표할 수 있는 메커니즘이 내장


