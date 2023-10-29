---
title:  "데이터 증대(Augmentation)"
excerpt: "데이터 증대 방법 알아보기"

categories:
  - ML
tags:
  - [ML, Augmentation]

toc: true
toc_sticky: true

date: 2023-03-27
last_modified_at: 2023-10-13
---

## 데이터 증대  
- Pytorch 라이브러리  

    ```python
    import torch
    import torchvision
    import torch.nn as nn
    import torch.optim as optim
    import torch.nn.functional as F

    from torch.optim import lr_scheduler
    from torchvision import models, transforms
    from torchvision.datasets import ImageFolder
    from torch.utils.data import TensorDataset, DataLoader, Dataset
    ```
<br>  

- 데이터 transforms 정의  

  ```python
  train_transforms = transforms.Compose([
      transforms.RandomResizedCrop(size=256, scale=(0.8, 1.0)),
      transforms.RandomRotation(degrees=15),
      transforms.RandomHorizontalFlip(),
      transforms.CenterCrop(size=224),
      transforms.ToTensor(),
      transforms.Normalize(mean=[0.485, 0.456, 0.406],
                          std=[0.229, 0.224, 0.225])
  ])

  test_transforms = transforms.Compose([
      transforms.Resize(size=256),
      transforms.CenterCrop(size=224),
      transforms.ToTensor(),
      transforms.Normalize(mean=[0.485, 0.456, 0.406],
                          std=[0.229, 0.224, 0.225])
  ])
  ```
  위의 코드는 이미지 분류 모델을 학습하기 전에 데이터를 증강하기 위한 transform을 정의한 것이다.  
  (Test 데이터에서는 Rotation이나 Flip을 수행하지 않았다.)  
<br>  

- 데이터 로드  

  ```python
  batch_size = 32

  train_dataset = CustomDataset(train_df, transform=train_transforms)
  train_loader = DataLoader(train_dataset, batch_size=batch_size, shuffle=True)

  test_dataset = CustomDataset(test_df, transform=test_transforms)
  test_loader = DataLoader(test_dataset, batch_size=batch_size)
  ```
  transform은 원본 이미지에 영향을 주지 않고 수행되며,  
  이 기법을 사용해서 1회 epoch에 입력되는 이미지 수가 변하는 것은 아니지만,  

  훈련 중에 epoch마다 원본 이미지를 랜덤하게 변환시킴으로써 전체 학습 과정에서는 마치 원래 가지고 있던 이미지들 보다 더 다양한 이미지로 학습한 것과 같은 효과를 얻을 수 있어 모델의 일반화 성능에 도움을 준다.  
<br><br>  


- Transfer Learning 모델  

  ```python
  num_classes = 5

  model = models.resnet50(weights='DEFAULT')
  num_ftrs = model.fc.in_features

  model.fc = nn.Sequential(
      nn.Linear(num_ftrs, 512),
      nn.BatchNorm1d(512),
      nn.ReLU(inplace=True),
      nn.Dropout(),
      nn.Linear(512, 256),
      nn.BatchNorm1d(256),
      nn.ReLU(inplace=True),
      nn.Dropout(),
      nn.Linear(256, num_classes)
  )
  model.to(device)

  # define optimizer
  optimizer = optim.Adam(model.parameters(), lr=learning_rate)
  scheduler = torch.optim.lr_scheduler.ReduceLROnPlateau(optimizer, mode='min', factor=0.1, patience=3)
  ```
  사전 훈련된 'ResNet50'에 추가로 레이어를 이어 모델 정의  

  - Dropout  
  과적합을 방지해서 모델의 일반화 성능을 향상시키기 위해 사용하는 dropout은  
  기본적으로 Pytorch와 같은 프레임워크에서 model.train()으로 training 과정에서는 수행하고, model.eval()으로 inference 과정에서는 수행되지 않도록 동작한다.  
<br>  

- Fine-Tunning  

  ```python
  for i, data in enumerate(train_loader):
      inputs, labels = data
      inputs, labels = inputs.to(device), labels.to(device)
        
      # zero the parameter gradients
      optimizer.zero_grad()

      # forward + backward + optimize
      outputs = model(inputs)
      loss = criterion(outputs, labels)
      loss.backward()
      nn.utils.clip_grad_norm_(model.parameters(), max_norm=1.0)
      optimizer.step()
  ```
