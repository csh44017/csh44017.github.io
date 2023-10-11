---
title:  "전이 학습(Transfer Learning)"
excerpt: "전이 학습 관련 지식 정리"

categories:
  - ML
tags:
  - [TransferLearning]

toc: true
toc_sticky: true

date: 2023-03-19
last_modified_at: 2023-10-05
---

- 참고자료  
  - [나동빈님 전이학습 설명 유튜브](https://www.youtube.com/watch?v=P5qemCucZ14&list=PLRx0vPvlEmdADpce8aoBhNnDaaHQN1Typ&index=44){:target="_blank"}  
  논문 이해에 필요한 배경 지식과 이미 익숙한 사람이라면 스킵할만한 부분도 꼼꼼하게 설명해주신다.  


## 전이 학습(Transfer Learning)  
### 소개  
- [Big Transfer(BiT) 논문](https://arxiv.org/pdf/1912.11370.pdf){:target="_blank"}  
  Google Research 팀에서 발표한 이 논문은 이미지 분류를 목적으로 **작은 데이터셋만으로도 좋은 성능의 모델을 쉽게 얻어낼 수 있는 방법**에 대해서 작성되었다.  

  대상의 표현적인 정보를 흔히 'Representation' 또는 'Semantic Feature'라고 말하는데, 'Representation Learning'은 이미지나 텍스트와 같은 데이터셋에 존재하는 feature들을 학습하는 것이다.  

  신경망의 깊이가 깊어지면 서로 다른 종류의 복합적인 feature들을 추출해낼 수 있다.  
  신경망의 낮은층에서 학습되는 특징인 low-level feature에서는 주로 색의 변화나 경계의 방향에 대한 정보가 추출되고,  
  깊은 층에서 학습되는 특징인 high-level feature에서는 주로 객체의 패턴이나 형태 정보를 추출할 수 있다.  

  <div align="center">  
  <img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/75edc825-7333-41a4-a159-0f72ab395ab8" width="500" height="270">  
  </div>  
<br><br>  


### 전이 학습이 필요한 이유  
좋은 성능의 모델을 만들기 위해서는 양질의 많은 데이터셋이 필요하고, 수집한 데이터셋으로 큰 리소스를 들여 설계한 모델을 처음부터 학습시켜야 한다.  
Google과 같은 큰 기업에서는 이러한 과정이 가능하겠지만 개인이나 작은 기업의 경우에는 양질의 데이터를 대량으로 수집하는 것부터가 쉽지 않다.  
이러한 한계를 극복하면서 좋은 성능의 모델을 효율적으로 개발하기 위한 아이디어로 **전이학습(Transfer Learning)** 을 사용할 수 있다.  
<br><br>  


### 전이 학습 방법  
Transfer Learning은 네트워크의 인코더 부분에 적용해서 사용한다.  

1. 먼저 사전에 대량의 데이터로 **학습된(pre-trained) 모델을 불러온다**.  
  (여기서 pre-trained 모델은 이미지 처리의 경우 CNN모델이 될 수 있고 자연어 처리에서는 BERT 같은 모델이 사용될 수 있다.)  
2. 입력 데이터에 맞게 네트워크의 **Input shape을 조정**한다.  
3. **전이할 Feature Extractor Layer 부분을 초기 가중치로 재활용**하여 네트워크를 구성한다.  
4. 이 네트워크의 마지막에 **새로운 Fully Connected Layer를 필요한 Output shape에 맞게 구성**하여 이어붙인다.  
5. 재구성한 **네트워크를 학습**한다.  
<br>  

pre-trained 모델에서 진행된 사전에 이루어진 학습을 '**업스트림(upstream)**'이라고 하고, 사전 모델을 전이해서 새롭게 진행하는 학습은 '**다운스트림(downstream)**'이라고 한다.  
(다운스트림은 일반적으로 과하지 않은 epoch 수에 많지 않은 데이터셋으로 높은 정확도를 얻을 수 있기 때문에 비용이 적게 든다.)  
<div align="center">  
<img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/c233ca1d-5104-43ac-988e-ccc1bc49580a" width="500" height="150">  
</div>  

다운스트림은 전이된 네트워크 부분의 가중치를 학습시키지 않는 '**프리징(Freezing)**'과 전체 네트워크를 모두 학습시키는 '**파인튜닝(Fine-Tuning)**'으로 나뉜다.  
- 프리징  
  현재 데이터셋에 대한 오버 피팅으로 부터 비교적 안전하다.  
  <div align="center">  
  <img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/96dba00d-709e-4475-973d-e305e53cec54" width="500" height="150">  
  </div>  

- 파인튜닝  
  일반적으로 많이 사용되는 방식  
  미세조정만 하기 때문에 일반적인 학습보다 학습률을 낮게 설정한다.  
  <div align="center">  
  <img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/53509de4-833a-430c-ab07-61d33e6cac43" width="500" height="150">  
  </div>  

  - 원샷러닝(One-shot Learning)  
    **하나의 데이터로 학습**하여 식별할 수 있는 모델을 만드는 방식  
    의료 분야처럼 하나의 데이터를 수집하는 것이 비용이 크거나 희귀한 경우 사용  

  - 퓨샷러닝(Few-shot Learning)  
    **몇 개의 데이터로 학습**을 진행하는 방식  
<br><br>  


### 전이 학습의 장점  
- 어느 정도 훈련된 가중치를 가지고 학습을 시작하므로 **최종 수렴에 도달하는 속도가 빨라질 수 있다**.  
- 사전에 학습된 네트워크와 새로 학습시킬 네트워크가 완전히 동일한 목적의 태스크가 아니더라도 두 데이터셋이 유사한 특징을 가진다면 **정확도가 향상**될 수 있다.  
  (ex. 50개 클래스의 꽃 분류 모델에서 10개 꽃 분류 모델로 전이 학습)  
- 가지고 있는 **데이터셋의 크기가 작더라도 모델이 좋은 성능**을 내도록 학습할 수 있다.  
<br><br>  


### 다른 태스크의 Feature Extractor Layer가 긍정적인 영향을 미치는 이유는?  
사용할 데이터셋의 조건이 pretrain할 때의 데이터셋과 동일하다면 모델을 그대로 가져와서 재학습하는 것이 이상하지 않지만,  
조건에 차이가 있는 경우에도 Feature Extractor Layer를 전이학습에 사용한다.  

그 이유는, 대량의 데이터로 얕지 않은 신경망을 사용하여 학습했기 때문에  
**네트워크가 보편적인 이미지 종류에 대한 feature를 추출하는 쪽으로 학습**되어  
유의미한 가중치를 가지게 되고 결과적으로 좋은 모델이 만들어진다.  
<br><br>  


### 전이 학습 예제  
[CNN과 Transfer Learning](https://www.kaggle.com/code/gpreda/cats-or-dogs-using-cnn-with-transfer-learning){:target="_blank"}  

- ResNet-50을 이용하여 모델 구성  
  ```python  
  model = Sequential()
  model.add(ResNet50(include_top=False, pooling='max', weights=RESNET_WEIGHTS_PATH))
  model.add(Dense(NUM_CLASSES, activation='softmax'))
  # ResNet-50 model is already trained, should not be trained
  model.layers[0].trainable = True
  ```
  모델을 ResNet-50으로 초기화하고 마지막에 원하는 클래스들로 분류하도록 softmax를 활성화함수로 사용하는 Dense 레이어를 추가하여 간단한 전이 학습 모델을 구성  

- 모델 컴파일  
  ```python  
  model.compile(optimizer='sgd', loss='categorical_crossentropy', metrics=['accuracy'])

  model.summary()
  ```
  ```shell  
  _________________________________________________________________
  Layer (type)                 Output Shape              Param #   
  =================================================================
  resnet50 (Model)             (None, 2048)              23587712  
  _________________________________________________________________
  dense (Dense)                (None, 2)                 4098      
  =================================================================
  Total params: 23,591,810
  Trainable params: 23,538,690
  Non-trainable params: 53,120
  _________________________________________________________________
  ```  
<br><br><br>  


## BiT 논문  
### Pre-Training 실험  
- 데이터셋  
  - BiT-S (약 1,300,000개 이미지의 ILSVRC-2012)  
  - BiT-M (약 14,000,000개 이미지의 ImageNet-21K)  
  - BiT-L (약 300,000,000개 이미지의 JFT-300M)  
- 네트워크 아키텍처  
  - ResNet-50x1 (레이어 50, 너비 1)  
  - ...
  - ResNet-152x4 (레이어 152, 너비 4)  
    (너비 width의 증가는 블록 내 필터 수의 증가)  
<br>  

### Upstream / Pre-Training  
#### 정규화 기법  
입력되는 feature들에 따라서 스케일의 분포 정도가 다를 경우 각각의 분산, 평균에 차이가 생긴다.  
(ex. 나이는 입력이 주로 0에서 100내외이지만 시력은 입력이 0 내외로 들어옴)  

**입력되는 데이터들마다 mean, variance와 같은 통계적 특성(Statistics)이 다르면 학습의 난이도가 어려워질 수 있다**. 이를 방지하기 위한 기법으로 input 전에 정규화를 진행하곤 한다.  
(이미지의 경우 Input Layer 전에 RGB채널마다 정규화를 진행해서 각각의 픽셀들이 평균이 0 표준편차가 1이 되도록 조정)  
<br>  

#### 배치 정규화(Batch Normalization)  
Input Layer 뿐아니라 **Hidden Layer에 입력하기 전에도 정규화를 진행**  
<div align="center">  
<img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/1cd7999b-bfd5-4ea7-b890-d73f5f0de3b4" width="500" height="200">  
</div>  

- 효과  
  - 입력데이터의 분포가 안정되어 **훈련 과정이 안정되고 학습 속도가 빨라진다**.  
  - 모델의 최종 성능이나 수렴에 큰 영향을 미치는 **가중치 초기화에 덜 민감**하다.  
  - 과적합을 억제하는 효과가 있어 **모델이 일반화**된다.  
  - **그래디언트 소실 또는 폭주 문제를 완화**해서 깊은 신경망 훈련에 도움을 준다.  
<br>  

학습할 데이터 양이 많기 때문에 전체 데이터셋을 배치 사이즈로 분배하고 이를 다시 GPU와 같은 디바이스들끼리 분담하여 처리하는 방법을 사용하는데,  
모델이 큰 경우 디바이스당 요구되는 메모리가 크기 때문에 일반적으로 한번에 작은 데이터를 입력 받는다.  

이로 인해 **모든 디바이스마다 계산한 Statistics 값들을 수합하면 동기화하는 비용이 더 커지게 되면서 지연**될 수 있고,  
배치 사이즈가 작으면 **배치의 평균과 분산이 iteration마다 달라져 BN이 가진 배치의 통계 값이 전체 데이터셋을 대표한다는 가정에 어긋나면서 정확도 하락으로 이어질 수 있기 때문에** BiT 논문에서는BN Layer를 사용하지 않는다.  
<div align="center">  
<img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/137060e4-0d0f-49ca-81db-2ced7cb72901" width="500" height="170">  
</div>  

대신 몇 개의 채널에 대한 피처를 묶어서 **정규화하는 방법으로 Layer Norm과 Instance Norm의 장점을 적절히 사용한 'Group Normalization(GN)'과 가중치를 표준화하는 'Weight Standardization(WS)'를 함께 사용**하여 배치 사이즈로 인해 발생할 수 있는 문제를 개선하고 성능을 향상시켰다.  
(배치 사이즈에 의해 성능이 좌우되지 않기 때문에 배치 사이즈를 키울 수 있고 덕분에 학습 속도가 향상됨)  
<div align="center">  
<img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/c3333348-61e2-441c-a0b4-37c4194b8da6" width="500" height="130">  
</div>  

- 가중치 표준화(Weight Standardization)  
  **표준편차가 1이 되도록 레이어의 가중치를 정규화**하는 것  
<br>  

#### 데이터 전처리  
1. **224 x 224로이미지를 resize** 한 후, **정사각형**으로 만듦  
2. **Crop out**으로 이미지에서 원하는 부분을 잘라냄  
3. 무작위로 **좌우 반전**시켜 데이터 증대  
  (인간의 눈으로 보았을 때 좌우 반전은 sementic한 정보가 크게 바뀌지 않으므로 상하 반전은 잘 사용하지 않고 좌우 반전을 사용)  

|             |Pre-training|Fine-tuning|  
|-------------|------------|-----------|  
|Weigth Decay |O           |X          |
|MixUp        |X           |O          |  

- 가중치 감쇠(Weight Decay)  
  **손실 함수에 패널티로 정규화 항($l_2$ norm)을 추가 손실 항목으로 더하여 신경망의 과적합을 방지**하는 정규화 기술  
- MixUp  
  데이터 증강(Data Augmentation) 기법 중 하나로,  
  두 샘플데이터를 **한 쌍으로 데이터와 라벨을 혼합해 새로운 합성 데이터, 새로운 합성 라벨을 만들고 이를 훈련에 사용**하여 과적합을 방지하는 기술  
<br>  

BiT에서는 사전 학습에 Weight Decay로 정규화를 수행하고 데이터가 많으므로 MixUp을 하지 않았다.  
또한, 배치 사이즈를 4096으로 하여 512개의 칩을 가지고 있는 TPUv3-512에 칩마다 8개의 이미지가 들어가도록 진행하였다.  
(SGD momentum 0.9, 초기 learning rate 0.03으로 세팅하고  
학습률을 점진적으로 줄여나가는 learning rate decay 사용)  
<br><br>  


### Downstream / Fine-Tuning  
SGD momentum 0.9, 초기 learning rate 0.003에 learning rate decay 사용  
(Pre-training 과정에서 학습되었던 가중치가 많이 변하는 것을 방지하기 위해 **Fine-tuning 과정에서는 learning rate을 상대적으로 작은 크기로 사용**함)  
medium과 large 태스크에서 $α$ = 0.1로 MixUp 진행  

|task                       |step        |  
|---------------------------|------------|  
|small 태스크 (20k개 미만)   |500 steps   |  
|medium 태스크 (500k개 미만) |10,000 steps|  
|large 태스크                |20,000 steps|  

- step  
  각 미니배치마다 한번씩 수행되는 모델의 가중치 업데이트  
- epoch  
  모든 미니배치를 처리하여 전체 데이터셋을 1회 학습하는 것  
<br><br>  


### 실험 결과  
<div align="center">  
<img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/a3131a64-6ca6-44f9-8db7-8ed2fa4e6a88" width="500" height="180">  
</div>  

데이터가 적을 때 많은 성능 차이를 보이며, 기존의 SOTA 보다 높은 정확도를 보여줌  
<br>  

<div align="center">  
<img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/20cbb7ff-7872-4c6c-aaf5-29b0197165d7" width="500" height="150">  
</div>  

- Specialist  
  해당 태스크만을 목적으로 pre-trained representations를 이용  
- Generalist  
  일반적인 목적으로 pre-trained representations를 이용  
- Baseline  
  일반적으로 많이 사용되는 사전 학습 모델  
<br>  

<div align="center">  
<img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/57f766e9-8c42-49b3-8cae-fe737e5c9e68" width="500" height="120">  
</div>  

ResNet-152x4 처럼 아키텍처가 크면 데이터셋의 크기가 클 수록 Downstream 태스크에서 정확도가 향상됨  
<br>  

<div align="center">  
<img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/3a6f0b11-7b90-4378-b14f-5bc92838f39e" width="500" height="180">  
</div>  

Upstream 과정에서는 라벨링된 데이터만으로 학습이 되었다는 점이 차이가 있지만,  
학습 데이터가 적은 상황에서 기존에 제안된 준지도학습(Semi-Supervised Learning, SSL) 방법들보다 높은 정확도를 보임  
<br>  

<div align="center">  
<img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/17a7786b-68a6-4008-abe8-ed48557d2e1c" width="500" height="180">  
</div>  

Object Detection에서 BiT를 Backbone Network로 사용할 때 정확도가 개선됨  
<br>  

<div align="center">  
<img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/
0ec0490e-8efc-4d7e-a546-b3f4264ed716" width="500" height="180">  
</div>  
<div align="center">  
<img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/a06684a4-06e0-4d27-a54b-6922ec343d2a" width="500" height="180">  
</div>  

**모델의 크기가 작을 경우, 데이터셋의 크기가 커짐에 따라 오히려 정확도가 감소할 수 있음**  
<br>  

<div align="center">  
<img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/92fc2e45-a265-4708-8626-c3b700e10cc3" width="500" height="180">  
</div>  

**큰 데이터셋으로 학습할 경우 학습 시간을 충분히 사용하여 진행**해야하며,  
**짧은 시간마다 learning rate decay를 수행하기 시작하면 최종 성능이 떨어질 수 있음**  
**높은 weight decay를 사용했을 때 수렴 속도가 느려진 대신 최종 성능이 좋아짐**  
