---
title:  "DevOps와 MLOps (Machine Learning Operations)"
excerpt: "MLOps 정리"

categories:
  - MLOps
tags:
  - [ML, DevOps, MLOps]

toc: true
toc_sticky: true

published: true

date: 2022-10-01
last_modified_at: 2023-10-13
---

## MLOps (Machine Learning Operations)  
머신러닝 모델의 개발 및 운영 프로세스를 자동화하고 효율화하기 위한 방법  
'ModelOps(모델 관리)' + 'DataOps(데이터 관리)' + 'DevOps(배포, 클라우드)'  
<br>  

### 등장 배경  
* 필요성  
  - 주피터 노트북에서 만들어 프로토타입만 사용해본 모델을 실제로 서비스에 이용  
  - 파라미터 변경 등으로 인한 모델의 버전 관리  
  - 학습에 사용된 데이터셋의 버전 관리  
  - 학습에 사용된 데이터와 실제 서비스(Production)에서 받는 데이터가 다른 경우  
  - 머신러닝 서버를 올리기 위한 인프라 관리  
  - 모델을 다시 학습해야할 필요가 있을 경우  
<br>  

* Research 단계에서 Production으로 갈 때 해야 하는 일  
  - API 서버 만들기  
  - 모델 관리  
  - 데이터셋 관리  
  - ML 인프라 관리  
<br>  

### DevOps vs MLOps  
- DevOps  
'**소프트웨어 배포**', '**인프라 운영**'을 중점으로 개발과 운영 프로세스를 통합하는 것이 목표 (CI/CD)  
(모델에 관여X, 데이터는 개발에서 운영으로 전달만하고 처리X)  
  - 버전 관리  
  - 컨테이너화  
  - 오케스트레이션  
  - 로깅  
  - 모니터링  
<br>  

- MLOps  
'**데이터 처리**'와 함께 '**모델을 개발**'하여 '**실제 환경에서 운영**'하고 모니터링 하는 것까지 목표 (CI/CD/CT)  
(모델 학습, 서빙 및 관리O, 데이터 처리O)  
(인프라쪽은 DevOps 팀에 맡기기도 함)  
  - 데이터 버전 관리
  - 모델 버전 관리  
  - 데이터 파이프라인  
  - 모델 서빙  
  - 모니터링  
<br>  

### On-Premise vs Cloud Computing  
- 온프레미스(On-Premise) 환경  
  - 자체적으로 데이터 센터를 운영하여 물리적인 서버, 스토리지, 네트워크 인프라를 직접 보유하고 유지보수 (초기 비용이 많이 발생)  
  - 운영체제, 데이터베이스, 응용 프로그램, 보안 소프트웨어 관리 및 업데이트  
  (데이터를 직접적으로 컨트롤해 데이터가 외부에 노출되는 것을 제한할 수 있음)  
<div align="center">  
<img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/fee4dae3-3adb-4ac4-a893-e7a158197e10" width="400" height="200">  
</div>  
<br>  

- 클라우드 컴퓨팅 (Cloud Computing)  
서비스 제공 업체가 하드웨어 및 인프라를 관리하고, 사용자는 원격으로 클라우드 서버에 접근하여 리소스를 이용  

  - 서비스 제공 형태  
    - Public Cloud  
    인터넷 접속이 가능한 모든 사용자를 대상으로 서비스  
    - Private Cloud  
    제한된 네트워크 상에서 특정 사용자만을 대상으로 서비스  
    (데이터가 기업 내부에 저장됨)  
    - Hybrid Cloud  
    Public 클라우드와 Private 클라우드를 병행으로 사용하거나,  
    클라우드(가상서버)와 온프레미스(물리서버)를 결합하여 사용하는 방식  
<div align="center">  
<img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/90ffaa75-f7ed-45f0-803e-ee060e91d2c6" width="400" height="310">  
</div>  

  - 서비스 모델  
    - Infrastructure as a Service (IaaS)  
    인터넷을 통해 가상화된 컴퓨팅 리소스를 제공  
    ex) AWS EC2, MS Azure 가상머신, GCE, ...  
    - Platform as a Service (PaaS)  
    서버, 스토리지, 네트워킹같은 인프라를 제공하여, 사용자는 애플리케이션 개발 및 배포만 진행  
    ex) Heroku, Google App Engine, MS Azure App Service, ...  
    - Software as a Service (SaaS)  
    인프라, 애플리케이션 코드, 데이터를 공급자가 모두 관리하여 제공  
    ex) MS Office 365, Google Workspace, ...  
<div align="center">  
<img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/c7fd12f1-50f3-4a73-b0a7-4584582f0460" width="500" height="300">  
</div>  
<br><br>  


## 데이터(Data) 소프트웨어  
### 데이터 수집 파이프라인  
데이터 수집, 전처리, 로깅 등  
- Apache Sqoop  
- Apache Flume  
- Apache Kafka  
- Flink  
- Spark Streaming  
- Airflow  
<br>  

### 데이터 저장  
- RDBMS  
  - MySQL  
- 분산 저장  
  - Hadoop      
- 오브젝트 스토리지  
  - Amazon S3  
  - MinIO  
<br>  

### 데이터 관리  
- 데이터의 벨리데이션 체크  
  - TFDV  
- 데이터 버전 관리  
  - DVC  
- 피처 스토어  
  - Feast  
  - Amundsen  
<br><br>  


## 모델(Model) 소프트웨어  
### 모델 개발  
- 모델 개발을 격리된 환경에서 수행  
  - Jupyter Hub  
  - Docker  
  - Kubeflow  
- 하이퍼파라미터 옵티마이제이션 같은 병렬 학습을 클라우드 환경에서 쉽게 할 수 있게 해주는 툴  
  - Optuna  
  - Ray  
  - katib  
<br>  

### 모델 버전 관리  
- 소스 코드 형상 관리  
  - Git  
- 모델의 소스 코드 뿐만 아니라 패키징된 형태의 모델과 사용한 하이퍼 파라미터, 모델의 성능 매트릭까지 모두 함께 기록  
  - MLflow  
- CI/CD  
  - Github Action  
  - Jenkins  
  - Travis CI  
<br>  

### 모델 학습 스케줄링 관리  
GPU 인프라 관리, 모델 학습 스케줄링  
- 모니터링  
  - Grafana  
- 스케줄러  
  - Kubernetes  
<br><br>  


## 서빙(Serving) 소프트웨어  
데이터를 받으면 ML 모델에 predict 함수를 부른 다음 그 결과값을 반환하는 행위를 사용자가 직접 코드를 돌려보는 것이 아닌 서버에서 API 형태로 제공하는 서비스  

### 모델 패키징  
OS나 특정 python 패키지에 의존성이 없도록 컨테이너 혹은 VM 기반으로 패키징  
- ML 모델에 특화된 API 서빙 프레임워크  
  - Flask  
  - FastAPI  
  - BentoML  
  - Kubeflow  
  - TFServing  
  - KFServing  
  - seldon-core  
<br>  

### 서빙 모니터링  
서빙 후 환경이 제대로 돌아가고 있는지 모델의 성능이 떨어지고 있지는 않은지와 같은 매트릭을 지속적으로 모니터링하고 문제가 생기면 알림을 받는 것을 자동화  
- Prometheus  
- Grafana  
- Thanos  
<br>  

### 파이프라인 매니징  
서빙 시 모델의 성능이 떨어지면 이전 모델로 롤백하거나 AB 테스팅을 하는 등 성능 확인을 위해서 이전 모델 학습과정 전체를 다시 돌려보고 싶은 경우를 위해 모델 개발 단계부터 단순한 파이썬 코드가 아닌 특정한 파이프라인 코드로 개발을 해야 재사용이 가능  
- Kubeflow  
- argo workflow  
- Airflow  
<br><br>  


## SaaS (Software as a Service) 제품  
- (Amazon Web Services) AWS SageMaker  
- (Google Cloud Platform) GCP Vertex AI  
- (Microsoft Azure) Azure Machine Learning  
<br><br>  





### 데이터 관리  
- Apache Hadoop  
- Apache Spark  

### 모델 개발  
- Tensorflow  
- Pytorch  

### 버전 관리  
- Git  
- Github, Gitlab, Bitbucket  

### CI/CD  
- Jenkins  
- CircleCI, Travis CI, Gitlab CI/CD  

### 컨테이너화 오케스트레이션  
- Docker  
- Kubernetes  

### 모델 서빙 및 배포  
- Kubeflow  
- Amazon SageMaker  

### 로그 및 모니터링  
- Prometheus  
- Grafana  
- ELK stack (Elasticsearch, Logstash, Kibana)  

### 보안 및 권한 관리  
- Kubeflow Identity and Access Management (IAM)  
- Vault  

### 데이터 모니터링 및 관리  
- Apache Airflow  
- Apache Kafka  

### 자동화 플랫폼  
- MLflow  
- Terraform  