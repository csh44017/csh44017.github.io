---
title:  "모델 추론(Inference)"
excerpt: ""

categories:
  - ML
tags:
  - [Inference]

toc: true
toc_sticky: true

published: true

date: 2023-03-02
last_modified_at: 2023-09-24
---

## 추론(Inference)  
모델을 학습시키고 나서는 실제 적용할 곳에 
런타임에서 돌려줘야 함

### ONNX(Open Neural Network Exchange)  
Pytorch나 Tensorflow 등의 다른 프레임워크 간의 상호 운용을 허용하여 원활하게 교환하고 배포할 수 있게 해주는 표준 파일 형식  
사전 훈련된 모델을 ONNX 형식으로 내보내면 ONNX를 지원하는 다른 프레임워크에서 가져와 다양한 환경에서 사용할 수 있다.  

### Inference Runtime  
#### TFLite-RT  

#### ONNX-RT(ONNX Runtime)  
ONNX가 모델을 표현하고 교환하는 것을 처리 동안에 ONNX 연산자의 최적화된 구현을 제공하고 계산을 병렬화하여 효율적인 ONNX 모델을 실행 및 추론하는 엔진  

### TVM compiler  
Apache에서 개발한 딥러닝 컴파일러로 모델을 타겟 디바이스에서 최적의 속도와 정확도를 낼 수 있는 코드 변환 작업 도구  
다양한 프레임워크로 부터 모델을 통합된 형태로 컴파일  
통합된 모델을 타겟 하드웨어에 최적화된 형태로 실행  




## 모델 컴파일러  
### Tensorflow  
Tensorflow에서는 그래프 기반 컴파일러를 사용한다.  
모델을 그래프 형태로 변환하고 최적화하여 실행 속도를 향상시키는 역할을 한다.  

### Pytorch  
Pytorch는 동적 그래프 컴파일러를 사용한다.  
모델을 실행하면서 그래프를 생성하고 최적화하는 방식으로 동작한다.  
디버깅과 실험하기에 더 편리하다.  

### Caffe  


### ONNX(Open Neural Network Exchange)  
모델의 컴파일이나 서로 다른 딥러닝 프레임워크 간에 모델 변환을 위한 표준으로 다양한 딥러닝 프레임워크 간에 모델을 공유하고 실행하기 위한 중간 표현을 정의한다.  
ONNX를 지원하는 프레임워크에서는 ONNX 모델을 해당 프레임워크의 네이티브 형식으로 컴파일할 수 있다.  

### TVM(Apache TVM)  
오픈 소스 컴파일러 및 실행 엔진으로 딥러닝 모델을 최적화된 코드로 컴파일하고 다양한 하드웨어에서 실행할 수 있도록 지원한다.  

### CoreML(Apple Core Machine Learning)  
iOS나 macOS에서 머신러닝 모델을 실행하기 위한 프레임워크인 CoreML은 모델을 컴파일하고 모바일에서 실행하기 위한 도구를 제공한다.  

## 
### ONNX(Open Neural Network Exchange)  
Pytorch나 Tensorflow 등의 다른 프레임워크 간의 상호 운용을 허용하여 원활하게 교환하고 배포할 수 있게 해주는 표준 파일 형식  
사전 훈련된 모델을 ONNX 형식으로 내보내면 ONNX를 지원하는 다른 프레임워크에서 가져와 다양한 환경에서 사용할 수 있다.  

### ONNX-RT(ONNX Runtime)  
ONNX가 모델을 표현하고 교환하는 것을 처리 동안에 ONNX 연산자의 최적화된 구현을 제공하고 계산을 병렬화하여 효율적인 ONNX 모델을 실행 및 추론하는 엔진  

### TVM compiler  
Apache에서 개발한 딥러닝 컴파일러로 모델을 타겟 디바이스에서 최적의 속도와 정확도를 낼 수 있는 코드 변환 작업 도구  
다양한 프레임워크로 부터 모델을 통합된 형태로 컴파일  
통합된 모델을 타겟 하드웨어에 최적화된 형태로 실행  
