---
title:  "배치 정규화(Batch Normalization)"
excerpt: "배치 정규화 이해하기"

categories:
  - ML
tags:
  - [ML, Batch, Normalization]

toc: true
toc_sticky: true

math: true

published: true

date: 2023-03-17
last_modified_at: 2023-10-13
---

- 참고자료  
  - [나동빈님 배치 정규화 설명 유튜브](https://www.youtube.com/watch?v=58fuWVu5DVU&list=PLRx0vPvlEmdADpce8aoBhNnDaaHQN1Typ&index=10){:target="_blank"}  
  딥러닝 아키텍처에서 BN의 동작에 대해 관련 논문과 함께 꼼꼼히 들을 수 있다.  
<br><br>  


## 배치 정규화 (Batch Normalization)  
초기 Input Layer에 들어오는 데이터 외에 **Hidden Layer에 입력되는 데이터에도 정규화**를 진행하여 성능을 개선  

- 논문  
  - [Batch Normalization: Accelerating Deep Network Training by Reducing Internal Covariate Shift](https://arxiv.org/abs/1502.03167){:target="_blank"}  
    배치 정규화 제안  
  - [How Does Batch Normalization Help Optimization?](https://arxiv.org/abs/1805.11604){:target="_blank"}  
    배치 정규화가 도움이 되는 진짜 이유  
<br><br>  


## 입력 데이터 전처리 방법  
### 표준화 (Standardization)  
입력 데이터가 mean = 0, variance = 1인 **표준 정규분포 $N(0, 1)$를 따르도록 표준화**하는 것  

<div align=center><blockquote><p>  

$$ x_{std} = \frac{x - μ}{σ} $$  
( $μ$ : 평균, $σ$ : 표준편차 )  
</p></blockquote></div>  

<div align="center">  
<img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/7f5f5df6-0255-4e89-a15c-d240c510d519" width="500" height="160">  
</div>  
<br>  

최댓값과 최솟값에 제한이 없으므로 특정 범위를 벗어난 데이터를 outlier로 보고 제거하는 데에도 사용할 수 있다.  

<div align="center">  
<img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/685e6919-3c6f-4884-b66b-86c21ed6c8a8" width="400" height="250">  
</div>  
<br><br>  


### 정규화 (Normalization)  
입력 데이터의 값들을 0~1 또는 -1~1과 같은 **범위의 값으로 정규화**하는 것  
(ex. 이미지가 가지고 있는 0~255의 픽셀 값을 0~1의 값으로 스케일링)  

<div align=center><blockquote><p>  

$$ x_{norm} = \frac{x - x_{min}}{x_{max} - x_{min}} $$  
</p></blockquote></div>  

<div align="center">  
<img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/709c7090-2e4c-4163-b7a2-f6042fd22971" width="500" height="160">  
</div>  
<br>  

입력 **데이터의 값들이 특정 방향으로 치우쳐진 분포**를 가지고 있으면  
큰 learning rate을 사용할 경우 분산이 작은 방향에서 **학습이 제대로 이루어지지 않을 수 있고**,  
learning rate을 작게 설정해도 **학습 과정에서 많은 단계**를 거쳐야 하기 때문에 시간이 오래 걸린다.  

이를 개선하기 위해 **각 차원에 있는 값들의 범위가 비슷해지도록 '정규화'를 수행**하면,  
데이터의 분산이 고르게 변화하여 상대적으로 **큰 learning rate을 사용해도 최적의 성능에 쉽게 도달**할 수 있다.  

결과적으로, 학습률을 높일 수 있으므로 **학습 속도가 빨라지는 효과**를 볼 수 있다.  

<div align="center">  
<img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/a14d1bd0-8034-4d8a-aca6-5138aa352377" width="400" height="250">  
</div>  
<br><br>  


### 화이트닝 (Whitening)  
**평균이 0이고, 공분산이 단위 행렬인 정규분포 형태**의 데이터로 변환하는 기법  
(공분산이 단위 행렬 : 서로 다른 **feature들 사이에 연결성이 없는 Decorrelated 형태**로 데이터를 전처리)  

1. 특이값 분해(Singular Value Decomposition, SVD)와 같은 기법을 통해 **전체 데이터에 대해 새로운 축을 찾음**  
2. **새로운 축으로 전체 데이터를 transformation**하여 각각의 축에 대해 correlation이 적은 형태로 만듦  
3. 특정 범위의 값을 가지면서 **평균 값이 0이 되도록 전처리**  

<div align="center">  
<img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/9b72a79a-c4f8-4760-a440-a39dd8612623" width="500" height="180">  
</div>  

(PCA나 화이트닝 보다 간단하게 좋은 성능을 낼 수 있는 정규화가 많이 사용됨)  
<br><br>  


배치 사이즈 개수 만큼의 x를 입력 받음  
