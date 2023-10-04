---
title:  "모델 추론(Inference)"
excerpt: ""

categories:
  - ML
tags:
  - [Inference]

toc: true
toc_sticky: true

date: 2023-08-20
last_modified_at: 2023-08-20
---

## 추론(Inference)  

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
