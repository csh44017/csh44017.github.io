---
title:  "머신러닝 벤치마크(Benchmark)"
excerpt: "머신러닝에서 자주 등장하는 밴치마크 관련 정리"

categories:
  - ML
tags:
  - [ML, Benchmark]

toc: true
toc_sticky: true

published: false

date: 2023-02-22
last_modified_at: 2023-09-19
---

## 벤치마크(Benchmark)  
다양한 분야에서 사용되고 있는 '벤치마크'는 일반적으로 무언가를 **측정, 평가, 비교할 수 있는 표준**을 의미한다.  

'벤치마크'는 머신러닝에서도 자주 등장하는데,  
**알고리즘이나 모델의 성능을 평가하고 비교하는 데 일반적으로 사용되는 표준화된 데이터 세트나 평가 지표**를 말할 때 사용한다.  
그리고 이렇게 모범 사례로 확립된 **벤치마크와 머신러닝 모델 또는 알고리즘 기술을 비교하는 행위**를 '벤치마킹'한다고 말한다.  
<br><br>  

### 벤치마킹의 목적  
- 객관적인 성능 평가  
여러 사람들이 같은 목적에 대해 설계한 다양한 머신러닝 모델의 성능을 객관적으로 측정하고 비교할 수 있다.  
- 모델 선택  
사용할 머신러닝 모델을 선정할 때 특정 상황에 적합한 모델이 어떤 것인지 판단하는데 도움이 될 수 있다.  
- 진행 상황 추적  
빠르게 지속적으로 개발되는 알고리즘과 기술이 과거에서 어떻게 진행되고 있는지 상황을 추적할 수 있다.  
<br><br>  

### 벤치마크 데이터셋  
[Kaggle](https://www.kaggle.com/){:target="_blank"}이나 [Github](https://github.com/){:target="_blank"} 등에서 원하는 데이터셋을 검색하여 사용할 수 있다.  
<br>  

#### 이미지 분류  
- [ImageNet](http://www.image-net.org/){:target="_blank"}  
  매년 열리는 ILSVRC(ImageNet Large Scale Visual Recognition Challenge) 대회에서 사용되는 데이터셋이다.  
  이미지 분류를 위한 데이터셋으로 유명하고 널리 사용됨  
  수천 개의 클래스에 수백만 개의 라벨링된 이미지가 들어있음  

- [CIFAR-10](https://www.cs.toronto.edu/~kriz/cifar.html){:target="_blank"}  
  10개 클래스에 각각 6000개씩 총 60000개의 32x32 컬러 이미지가 들어있음  

- [CIFAR-100](https://www.cs.toronto.edu/~kriz/cifar.html)  
  100개 클래스에 각각 600개의 이미지가 들어있음  

- [STL-10](https://cs.stanford.edu/~acoates/stl10/)  

- [MNIST](http://yann.lecun.com/exdb/mnist/){:target="_blank"}  
  숫자 인식 작업에 사용되는 데이터셋으로 머신러닝에 입문하는 사람들이 많이 사용  
  손으로 쓴 0~9 숫자의 28x28 흑백 이미지가 들어있음  

- [Fashion-MNIST](https://github.com/zalandoresearch/fashion-mnist){:target="_blank"}  
  의류, 액세서리 등 패션 아이템 데이터셋  
  10개 클래스로 28x28 흑백 이미지가 들어있음  

- [SVHN (Street View House Numbers)](http://ufldl.stanford.edu/housenumbers/){:target="_blank"}  

- [Caltech-101](http://www.vision.caltech.edu/datasets/){:target="_blank"}  

- [Oxford Flowers](http://www.robots.ox.ac.uk/~vgg/data/flowers/102/){:target="_blank"}  

- [Oxford Pets](http://www.robots.ox.ac.uk/~vgg/data/pets/){:target="_blank"}  

- [Stanford Dogs](http://vision.stanford.edu/aditya86/ImageNetDogs/){:target="_blank"}  

|데이터셋 |해상도 |클래스 수 |학습 데이터 수 |테스트 데이터 수 |용량 |  
|---------|------|----------|--------------|-----------------|-----|  
|ImageNet |224 x 224 (color),<br>256 x 256 (color),<br>등 |1,000 |1,200,000 이상 |50,000 이상 |150 GB 이상 |  
|CIFAR-10 |32 x 32 (color) |10 |50,000 (클래스당 5000) |10,000 (클래스당 5000) |약 163 MB |  
|CIFAR-100 |32 x 32 (color) |100 |50,000 (클래스당 500) |10,000 (클래스당 500) |약 161 MB |  
|STL-10 |96 x 96 (color) |10 |5,000 (클래스당 500) labeled,<br>100,000 unlabeled |8,000 (클래스당 800) ||  
|MNIST|28 x 28 (gray) |10 |60,000 |10,000 ||  
|Fashion-MNIST|28 x 28 (gray) |10 |60,000 |10,000 ||  
|SVHN (Street View House Numbers)|32 x 32 (color) |10 |73,257<br>(+ 531,131 less difficult) |26,032 ||  
|Caltech-101|약 300 x 200 |101 |약 6,000 |약 3,000 |약 137.4 MB (.zip) |  
|Oxford Flowers||102 |1,664 |6,520 ||  
|Oxford Pets||37 |약 3,680 |약 3,680 ||  
|Stanford Dogs||120 |약 12,000 |약 8,000 ||  

<br>  

#### Object Detection  
- [PASCAL VOC](http://host.robots.ox.ac.uk/pascal/VOC/){:target="_blank"}  

- [COCO (Common Objects in Context)](https://cocodataset.org/){:target="_blank"}  
  객체 감지 및 분할을 위한 데이터셋  

- [KITTI](http://www.cvlibs.net/datasets/kitti/){:target="_blank"}  

<br>  


#### 자연어 처리(NLP)  
- [IMDb](https://www.imdb.com/interfaces/){:target="_blank"}  

- [Named Entity Recognition (NER)](https://www.clips.uantwerpen.be/conll2003/ner/){:target="_blank"}  

- [SQuAD (Stanford Question Answering Dataset)](https://rajpurkar.github.io/SQuAD-explorer/){:target="_blank"}  
