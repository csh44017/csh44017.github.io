---
title:  "Pytorch 익숙해지기"
excerpt: "Pytorch 연습"

categories:
  - ML
tags:
  - [Pytorch]

toc: true
toc_sticky: true

date: 2023-02-10
last_modified_at: 2023-09-27
---

## Pytorch  
[Pytorch DL 튜토리얼](https://uvadlc-notebooks.readthedocs.io/en/latest/tutorial_notebooks/tutorial1/Lisa_Cluster.html){:target="_blank"}  
- 참고  
  - [파이토치 공식 튜토리얼](https://pytorch.org/tutorials/beginner/basics/quickstart_tutorial.html){:target="_blank"}  
  - [파이토치 한국 튜토리얼](https://tutorials.pytorch.kr/beginner/basics/quickstart_tutorial.html){:target="_blank"}  

### 도메인 라이브러리  
- TorchText  
- TorchVision  
- TorchAudio  

파이토치에서는 도메인에 특화된 라이브러리를 구분해놓고 이를 import해서 각각의 모듈과 데이터셋을 사용할 수 있도록 기능을 제공해준다.  

예를 들어 TorchVision에서 datasets 모듈을 통해 CIFAR, COCO 등의 다양한 비전 데이터셋을 불러올 수 있다.  

```python  
import torch
from torch import nn
```  

#### 공개 데이터 다운로드 받기  
[TorchVision의 데이터셋 목록](https://pytorch.org/vision/stable/datasets.html){:target="_blank"}  
```python
from torchvision import datasets
from torchvision.transforms import ToTensor

# 학습 데이터 다운로드
training_data = datasets.FashionMNIST(
    root="data",
    train=True,  # train
    download=True,
    transform=ToTensor(),
)

# 테스트 데이터 다운로드
test_data = datasets.FashionMNIST(
    root="data",
    train=False,  # test
    download=True,
    transform=ToTensor(),
)
```
#### 데이터셋을 DataLoader 객체로 만들기  
```python
from torch.utils.data import DataLoader

batch_size = 64

# 데이터로더 생성
train_dataloader = DataLoader(training_data, batch_size=batch_size)
test_dataloader = DataLoader(test_data, batch_size=batch_size)

for X, y in test_dataloader:
    print(f"Shape of X [N, C, H, W]: {X.shape}")
    print(f"Shape of y: {y.shape} {y.dtype}")
    break
```
