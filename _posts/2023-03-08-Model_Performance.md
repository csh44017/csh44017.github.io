---
title:  "모델의 성능 확인"
excerpt: "머신러닝 모델의 성능을 확인하는 방법과 개념 정리"

categories:
  - ML
tags:
  - [ML, Model]

toc: true
toc_sticky: true

date: 2023-03-08
last_modified_at: 2023-09-13
---

- 모델  
  - 선형(Linear) 모델  
  그래프의 형태가 1개의 직선(1차 방정식)으로 표현되는 모델  
  (1개의 일정한 기울기를 가짐)  

  - 비선형(NonLinear) 모델  
  그래프의 형태가 1개의 직선(1차 방정식)으로 표현되지 않는 모든 형태의 모델  
  (1개 보다 많은 기울기를 가져 임의의 조건에 의해 기울기가 변함)  

- 평가  
'손실함수'와 '척도'의 개념은 유사하지만 경우에 따라 선택해서 모델을 최적화하는데 사용한다.  
  - 손실 함수(Loss Function)  
  학습 중에 모델의 성능을 향상시키기 위해 참조하는 값  
  '목적 함수(Objective Function)' 또는 '비용 함수(Cost Function)', '에너지 함수(Energy Function)'라고 부르기도 함<br>  
  옵티마이저에서 가중치를 업데이트할 때, 모델이 얼마나 잘못 예측하는가를 나타내는 손실 점수로 학습(training)의 방향을 모니터링 해볼 수 있으며, 이 값을 최소화하는 것을 목적으로 모델을 최적화 한다.  
  ex) 평균제곱오차(MSE), 평균절대오차(MAE)  

  - 척도(Metrics)  
  학습된 모델을 평가하기 위해서 사용하는 값  
  모델의 성능을 숫자로 표현하여 다른 모델과 비교한다.  
  ex) 정확도(Accuracy), 정밀도(Precision), 재현율(Recall), F1-Score  
<br><br>  

## 모델 성능 확인  
### 평균제곱오차(MSE, Mean Squared Error)  
실제 값과 모델이 예측한 값의 차이를 제곱한 값들의 평균  
E = (1/n) * n_sigma_i=1(y_i - y'_i)^2  
Accuracy의 개념을 적용하지 않는 회귀 모델에 대한 손실 함수로 많이 사용된다.  
<br>  

### 평균절대오차(MSE, Mean Absolute Error)  
실제 값과 모델이 예측한 값의 절대 오차  
E = (1/n) * n_sigma_i=1|y_i - y'_i|  
Accuracy의 개념을 적용하지 않는 회귀 모델에 대한 지표로 많이 사용된다.  
<br>  

### 혼동 행렬(Confusion Matrix)  
Confusion Matrix에 작성된 숫자는 변하지 않지만 클래스마다 TP, TN, FP, FN은 다시 정해진다.  
<div align="center">  
<img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/b086f63f-8cd5-42bf-a0e5-b042d7c1b5e8" width="500" height="300">  
</div>  
Confusion Matrix의 (1,1)을 Class 1의 관점에서 보면 정답과 모델의 예측 결과가 같으므로 TP가 되지만, Class 2의 관점에서 보면 정답에도 속하지 않고 모델의 예측에도 속하지 않아 해당 클래스와 무관하므로 TN이 된다.  
<br>  
  
- TP(True Positive) - 정탐  
실제로 양성 클래스이고, 분류 모델에서 양성으로 올바르게 예측한 수  
- TN(True Negative) - 일반  
실제로 음성 클래스이고, 분류 모델에서 음성으로 올바르게 예측한 수  
- FP(False Positive) - 오탐  
실제로는 음성 클래스이고, 분류 모델에서 양성으로 잘못 예측한 수  
- FN(False Negative) - 미탐  
실제로는 양성 클래스이고, 분류 모델에서 음성으로 잘못 예측한 수  
```python  
from sklearn.metrics import confusion_matrix

label = [1, 2, 1, 2, 3, 2, 4, 2, 3, 4, 1]
pred = [1, 2, 1, 3, 3, 2, 1, 1, 2, 2, 3]
print(confusion_matrix(label, pred))
```  
```shell  
[[2 0 1 0]
 [1 2 1 0]
 [0 1 1 0]
 [1 1 0 0]]
```  
<br>  

### 정확도(Accuracy)  
Accuracy = number of correct predictions(올바르게 예측한 수) / number of total predictions(전체 예측 수) = (각 클래스의 TP) / (TP + TN + FP + FN)  

데이터셋의 클래스들이 동일한 분포를 갖고 있다면 **Accuracy(정확도)**를 유용한 평가지표로 사용할 수 있다.  
(100장의 이미지 데이터셋에 고양이 이미지 50장, 호랑이 이미지 50장이 있다면 equal distribution을 갖고 있는 데이터셋)  
<br>  

### 정밀도(Precision)  
분류 모델이 특정 클래스로 예측한 것 중에서 실제로 해당 클래스인 것의 비율  
(Confusion Matrix의 각 열을 통해 특정 클래스에서 분류 모델이 얼마나 올바르게 예측하는지를 볼 수 있다.)  

Precision = TP / (TP + FP)  
(0.0 ~ 1.0 사이의 값을 가지며 높을수록 좋음)  
FP(오탐)가 0일 경우 정밀도는 1.0이 된다.  
<br>  

### 재현율(Recall)(Sensitivity)  
(Confusion Matrix의 각 행을 통해 특정 클래스일 때 분류 모델이 주로 어떤 클래스로 예측하는지를 볼 수 있다.)  

Recall = TP / (TP + FN)  
(0.0 ~ 1.0 사이의 값을 가지며 높을수록 좋음)  
FN(미탐)이 0일 경우 재현율은 1.0이 된다.  
<br>  

### 불균형 데이터(Imbalanced data)  
데이터셋의 클래스들이 불균형한 경우에 Accuracy는 좋은 평가지표(Metrics)로 작용하지 못할 수 있다.  

만약 데이터셋이 고양이 이미지 5장, 호랑이 95장으로 이루어져 있고, output을 무조건 호랑이 클래스로 분류하도록 모델을 개발한 후, Accuracy를 계산해보면 이 모델은 95%의 좋은 성능을 가진 것으로 평가된다. 하지만 이 모델을 실제로 배포하여 사용해보면 길고양이들도 전부 호랑이로 판단해버리는 문제가 발생할 것이다.  

<br>  

### F1 Score  
Precision과 Recall의 조화평균  
데이터가 불균형할 때 분류 모델에서 사용되는 머신러닝 평가지표(Metrics)이다.  
F1-Score = 2 * (Precision * Recall) / (Precision + Recall)  
<br>  


### Hanley & McNeil's auc comparison method  
DeLong 방법으로도 알려졌으며, 두 가지 다른 진단 또는 예측 모델의 ROC 곡선(AUC) 아래 영역을 비교하는데 사용되는 통계적 접근 방식  
AUC 값은 두 모델을 비교할 때 식별력의 척도를 보여주지만 통계적 불확실성(분산)이 해석에 영향을 줄 수 있으므로 AUC 값을 직접 비교하는 것만으로는 충분하지 않을 수 있기 때문에 두 AUC 값의 차이가 통계적으로 유의한지 확인하기 위한 통계 테스트이다.  
- AUC 계산  
  성능 메트릭을 사용하여 두 모델의 AUC 값 계산  
- 분산 계산  
  AUC에 대한 분산 계산
- 공분산 계산  
  AUC에 대한 공분산 계산  
- Z-통계량 계산  
  AUC, 분산, 공분산을 통해 계산하여 두 AUC 간의 차이가 0에서 얼마나 많은 표준 편차가 있는지를 나타낸다.  

Z-statistic이 0.05처럼 0에 가까울 경우 두 모델 간의 AUC 차이가 임의의 기회로 인한 것이 아닐 가능성이 높고 -4.3처럼 0에서 상당한 차이가 있는 경우 음의 값이므로 첫번째 모델이 두번째 모델보다 성능이 훨씬 나쁘며 