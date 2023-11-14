---
title:  "신경망 (Neural Network)"
excerpt: "신경망의 흐름 이해하기"

categories:
  - ML
tags:
  - [ML, NearalNetwork]

toc: true
toc_sticky: true

published: false

date: 2023-02-27
last_modified_at: 2023-09-09
---

## 신경망 (Nearal Network)  
입력 데이터  
feature map필터_(레이어에 들어오는 값 x 레이어의 가중치 행렬) > 활성화 함수  

채널 수 = 필터 수  


### CNN  
Convolution 연산을 통해 위치별로 가지고 있는 패턴을 찾아 특징을 추출  
이미지 전체 사이즈의 conv 연산을 하는 것이 아니라, 어떤 특징을 잘 추출하기 위한 3x3 등의 사이즈를 가진 필터로 이미지의 작은 부분에 대해 연산을 하고 바라본 부분의 엣지와 같은 특성을 파악  
- 3x3 conv  
  9개의 weight  
  receptive field가 좁음  
- 5x5 conv  
  25개의 weight  
- 3x3 conv + 3x3 conv  
  18개의 weight  
  5x5 conv와 같은 크기의 receptive field를 봄  

###
1. input data  
2. data custom  
3. input layer  
