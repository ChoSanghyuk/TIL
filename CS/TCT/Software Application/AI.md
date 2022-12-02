# AI Tech Letter



## 1. 인공지능 첫 걸음



### 인공지능에 대한 오해와 진실

- 오늘날 인공지능의 90% 이상은 지도학습

- 인공지능의 학습 및 추론이란 데이터를 벡터나 매트릭스 형태의 텐서로 만들어 복잡한 행렬 연산을 하는 것



### 인공지능이란

#### 강 인공지능 vs 약 인공지능

- 강 인공지능 = 범 인공지능
  - 명령 없이 스스로 판단하고 결정
  - 다양한 상황에 유연하게 대처
  - 사람과 자연스러운 대화
- 약 인공지능
  - 제한된 환경에서 구체적인 특정 업무 수행
  - ex) 알파고
- 범용성과 전문성은 절대적인게 아니라 정도의 차이

#### 넓은 의미의 AI vs 좁은 의미의 AI

- 넓은 의미 : 자동화 프로그램
- 좁은 의미 : 딥러닝(인공신경망)을 기반으로 데이터만 가지고 인간의 사고/행동 표방 가능
- 기분 기준
  - 판단 방식
  - 특징 추출

- AI : 기계 자동화 > Machine Learning > Deep Learning
  - 기계 자동화
    - 주요 정보 및 판단 규칙을 사람이 설계
    - 규칙 기반 프로그래밍
  - 머신러닝
    - 데이터로부터 의사결정 위한 패턴 학습
    - 특징만 사람이. 판단 모델은 모델링과 학습 통해 구현
  - 딥러닝 
    - 인공신경망 기반의 모델로, 비정형 데이터로브터 특징 추출 및 판단까지 기계가 한번에 수행
    - Raw Data로부터 모델링과 학습으로 모든 것을 구현
    - ex) CNN, RNN



## 2. 기계도 사람처럼 판단할 수 있을까?



### 머신러닝과 딥러닝

- 머신러닝 기반 기술
  - 정형 데이터 처리하는 빅데이터 분석 기법
  - ex)
    - 선형/로지스틱 회귀분석
    - 의사 결정 나무
    - ARMA, ARMIA (시계열)
- 딥러닝
  - 비정형 데이터, raw 데이터



### 모라벡의 역설

- 사람에게 쉬운 것이 기계에게 어렵고, 기계에게 쉬운 것이 사람에게 어렵다



### 인공신경망

- 생물학적 뉴런의 모양을 본따서 만든 것이 인공 뉴런
- 이전 뉴런이 넘겨준 데이터 받아들여 가중합 연산 후 비선형 함수를 적용해 정보를 가공 => 다음 뉴런에 토스



### 딥러닝 유행 배경

- 양질의 데이터를 다량 필요
- 높은 스펙의 하드웨어 필요





## 3. 시각을 얻은 인공지능



### 인공신경망 연산

- 딥러닝 모델은 인공뉴런을 여럿 연결한 인공신경망 기반

- 인공신경망은 가중합과 비선형 함수로 이루어진 연산

  => 입력 데이터로 벡터나 행렬 필요

### 기계의 이미지 인식

- 흑백
  - 2차원 
  - 1, 0
- 컬러
  - 3차원 텐서 (RGB)
  - 3 channel, 3 depth



### CNN (Convolutional Neural Network)

- Feature Extraction 영역

  - 이미지로부터 특징을 추출

  - 컨볼루션 연산과 풀링 연산 수행

    - 컨볼루션 연산

      - 컨볼루션 필터(커널)가 입력 이미지를 상하좌우로 훑으며 주요 특징 찾아냄

      - 필터 多 => 다양한 특징 파악 可

        => Feature Map (Convolved Feature)

    - 풀링 연산
      - Feature Map을 훑으며 핵심적인 정보만을 영역별 샘플링
      - 주로 MaxPooling : 영역 내 가장 큰 값만 남김

    => Feature Leanring

- 태스크 수행 영역

  - 특정 태스크 수행을 위해 덧붙임
  - 종류
    - Classification
    - Detection
    - Segmentation (경계 / 영역)



## 4. 언어를 이해하는 인공지능



### 자연어이해(NLU : Natural Language Understanding)

- NLP > NLU
  - NLP : 언어의 형식, 구몬론 포함
  - NLU : 감성분류, 요약

- 자연어 <=> 인공어



### 텐서 변환

- Tokenizing ( Parsing )
  - 한국어는 교착어로 형태소 단위 or 음절 단위로 자름
  - 뉴스, 논문 => 형태소 / 채팅, SNS => 음절이 좋음
  - 등장 빈도가 높은 N-gram 단위 토크나이징 : 둘의 장점 취함
- 워드 임베딩 (word embedding)
  - 토큰 => 벡터화
  - 종류
    - 원-핫 인코딩
      1. 토큰나이징 후 토큰들 중복 제거 => 사전
      2. 사전 길이만큼 벡터 정의
      3. 벡터에서 토큰의 순서에 해당 위치만 1. 아니면 0
      4. 토큰들을 벡터로 교체.
    - CBOW , SKIPGRAM
      - 단어(토큰)을 특정 길이의 임의 벡터로 생성 (길이는 사람이 지정)
      - CBOW
        - 문장 INPUT => 중간 단어 유추
        - 비슷한 단어들끼리 유사한 벡터 공간으로 임베딩
        - 토큰 끼리의 의미 연산 可
      - SKIPGRAM
        - 토큰 INPUT => 주변 문맥 생성



### 자연어이해 과제들

- 문장/문서 분류
- Sequence-to-Sequence
  - 번역 / 요약 / 자유 대화
- 질의 응답
  - MRC (Machine Reading Comprehension) : 최적 답변 리턴
  - IR (Information Retrieval) : 가장 유산 과거 질문 / 답변 리턴



## 5. 과거의 경험을 통해 현재를 배우는 인공지능



### RNN (Recurrent Neural Network)

- 누적된 과거 정보를 가지고 예측
- 장점
  - 과거 처리 내역 반영 => 더 나은 결정
  - 가변 길이의 데이터 처리 O
  - 다양한 구성의 모델
    - 1:1 ~ 多 : 多
    - 입력 데이터 정보 누적 (인코딩) + 결과 출력(디코딩)

- 단점
  - 느린 연산 속도
    - 병렬 처리 X. 순차적으로 데이터 처리
  - 불안정한 학습
    - Timestep이 길수록 문제 발생 확률 :arrow_up:
    - Gradient Exploding : 인공 신경망이 학습해야 할 값이 폭발적으로 증가
    - Gradient Vanishing : 과거의 이력이 현재 추론에 거의 영향 X
  - 실질적으로 과거 정보 잘 활용 X
    - 장기 종속성 / 의존성 문제 (Long-term dependency)
    - 먼 과거 정보 여러번 압축 + 누적 => 영향 :arrow_double_down:



### 보안책

- LSTM (Long-Short Term Memory)
  - 과거 정보 중 가중치
  - 구성 : 3 Gate
    1. Forget Gate
       - 불필요 정보 폐기
    2. Input Gate
       - 현재 정보 반영 정보 결정
    3. Output Gate
       - 현재 시점 최종 정보를 다음 시점에 얼마나 넘길지 결정
  - 단점 : 느린 속도
  - GRU(Gated Recurrent Unit) 과 유사



:bulb: 전통적인 머신러닝 기법에서는 ARIMA 기법을 통해 데이터 정보 흐름을 파악하고, 주기적으로 반복되는 패턴을 반영하여 분석함



## 6. 헛똑똑이 인공지능 제대로 가르치기



### AI Process

- Offline Process = Training Pipeline

  - 과거 데이터 가공
    - Generate Features (데이터 정제 및 필요 부분 확보)
    - Collect Labels (라벨링)
  - Trani Modles
    - Validate & Select Moldes (좋은 성능 도달까지 학습)
    - 튜닝 (실험 Experiment 반복)

  - Pulish Model
    - 배포 선택

- Online Process

  - 머신/딥러닝 < 개발 영역 (Application)

  - Load Model

    - AI를 운영 환경에 띄움

  - Live Data

    - 기존 정리된 데이터 X

      

### 오버피팅과 일반화 성능

- Generalization

  - 이전에 본적 없는 데이터에 대해서도 잘 수행하는 능력

- Overfitting

  - 훈련 시에만 정상 동작

    

### Training, Validation, Test

- 811 622
- Training Set
  - 모델 학습에 이용 데이터
  - 최적화
- Validation Set
  - 튜닝에 이용
  - 일반화 성능 판단
- Test Set
  - 학습에 어떤 식으로도 전형 관여 X.
  - 모델 최종 성능 평가 



### 학습 곡선 (Learning Curve)

- 학습이 진행됨에 따라 모델의 성능을 기록

- Training Set과 Validaiton Set의 학습 곡선 확인으로 오버피팅 확인



### Regularization

- 정규화

- 일반화 성능 향상

- 방식

  - 데이터 증강 (Data Augmentation)
    - 과도한 변형 주의
  - Capacity 줄이기
    - 모델의 복잡도 :arrow_down:
    - 필요 이상의 Capacity ~> 데이터 암기

  - 조기 종료 (Early Stopping)
  - 드롭아웃 (Dropout)
    - 학습 과정에서 일정 비율 p 만큼의 노드(인공뉴런) 무작위 끔



















