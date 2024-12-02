# DID W3C 표준



## 개요

크리덴셜(Credential)은 좁은 의미로는 신원 확인에 필요한 정보들을 의미하고, 보다 넓은 의미로는 특정 주체(사람, 조직, 동물, 사물 등 모든 것)와 관련된 모든 정보로 확대



디지털 세상, 즉 웹에서는 이러한 신원이나 자격을 표현하기 어렵고 그 크리덴셜의 유효성을 보장하는 것도 쉽지 않다

W3C의 [Verifiable Credentials Data Model](https://www.w3.org/TR/vc-data-model/)(검증가능한 크리덴셜 데이터 모델)에서는 웹에서 암호학적으로 안전하고, 프라이버시를 보장하며, 기계가 검증할 수 있는 방식으로 크리덴셜을 활용할 수 있는 표준 사양을 제시한다.



## 주요 개념



### **검증가능한 크리덴셜**

우리가 디지털 세계에서 사용하려는 크리덴셜도 물리적 세계의 신분증과 동일한 정보를 제공할 수 있어야 한다.

자기주권신원(Self-sovereign Identity)체계에서는 이를 Verifiable Credential (검증가능한 크리덴셜, 이하 VC) 이라고 하는데, 여기서의 핵심은 크리덴셜이 **‘검증가능한’** 것이라는 점

블록체인과 같은 분산원장, 전자서명 등의 기술을 적용하여 내가 증명하고자 하는 정보가 정말 믿을 수 있는 정보라는 것을 검증할 수 있는 형식이 바로 VC



![img](https://miro.medium.com/v2/1*OAm5IQ_E21Nra8lUe8pMPQ.png)

- Issuer (발급자)
  - Holder의 신원을 보증하는 주체이다. 
  - 일반적으로 Holder에 대한 정보를 가지고 있으며 Holder가 요청하면 VC의 형태로 발급한다.

- Holder (보유자)
  - 자신의 신원을 주장하기 위해 VC를 보유하고 있는 주체이다. 
  - VC를 바탕으로 Verifiable Presentation(검증가능한 프레젠테이션, 이하 VP)를 생성할 수 있다.

- Verifier (검증자)
  - Holder의 신원을 검증하려는 주체이다. 
  - 보통 자신의 서비스를 제공하기 위해 Holder에 대한 정보를 요구한다.

- Verifiable Data Registry (검증가능한 데이터 저장소)
  - VC를 검증하기 위해 필요한 식별자, 키 쌍, 메타데이터 등을 저장하는 저장소이다. 
  - 현재 블록체인이 주로 사용되고 있으나 신뢰할 수 있는 방식이면 어떤 종류라도 가능하다.





### **클레임 (Claim)**

디지털 환경에서 신원정보는 모두 데이터로 표현할 수 있다. 이러한 각 단위 데이터를 클레임(Claim)이라고 하는데 아래 그림과 같이 주체-속성-값의 구조를 가진다.

![img](https://miro.medium.com/v2/resize:fit:1050/1*66MR_q2Jubw3prDlgbOf7Q.png)



단위 클레임은 다른 클레임과 결합하여 아래와 같은 연결 정보(Graph of Information)를 생성할 수 있다.

![img](https://miro.medium.com/v2/resize:fit:1050/1*9MHWCBrfYLVs7PjZKOAkeQ.png)



### **크리덴셜(Credential)**

크리덴셜은 자격증, 암호학적 정보, 인증서 등 다양한 단어로 번역

 DID 체계에서는 크리덴셜을 주체에 대한 하나 혹은 그 이상의 클레임으로 구성된 데이터 집합이라고 기술적으로 정의

W3C에서는 크리덴셜 데이터의 교환을 위한 표준을 정의하고 있는데 이를 Verifiable Credentials Data Model(검증가능한 크리덴셜 데이터 모델)



### **검증가능한 크리덴셜 (Verifiable Credential)**

따라서 블록체인 상에 기록된 각 주체의 전자서명을 확인하므로써 궁극적으로 개인이 제시하고 있는 신원정보가 발급된 사실과 다르지 않다는 것을 검증할 수 있도록 한다. 그런 이유로 검증가능한 크리덴셜 즉 VC 라고 지칭

![img](https://miro.medium.com/v2/resize:fit:1050/1*QTpnl1FTgpqFVaRUAQEO8A.png)



### **검증가능한 프레젠테이션 (Verifiable Presentation)**

자기주권신원은 최소한의 정보 공개(Minimum Disclosure)를 원칙으로 하는 만큼, 증명이 필요한 정보들로만 구성된 새로운 형식이 필요한데 이를 검증가능한 프레젠테이션(이하, VP)

VP에 포함된 VC는 하나일 수도 있고 그 이상일 수도 있는데, 기존에 가지고 있던 다양한 VC를 바탕으로 조합이 가능

![img](https://miro.medium.com/v2/resize:fit:1050/1*en2IxMi0T88Cr1Dr3meptg.png)





