---
title:  "Kubernetes"
excerpt: "쿠버네티스 배경지식 정리"

categories:
  - MLOps
tags:
  - [Kubernetes]

toc: true
toc_sticky: true

published: true

date: 2022-11-19
last_modified_at: 2023-06-01
---

- 참고  
  - [MLOps 시스템 구축해보기](https://mlops-for-all.github.io/docs/setup-kubernetes/intro){:target="_blank"}  
  - [프로덕션 수준의 Kubernetes](https://kubernetes.io/ko/docs/setup/production-environment/){:target="_blank"}  
  - [Kubernetes 표준 아키텍처 소개](https://yozm.wishket.com/magazine/detail/1998/){:target="_blank"}  
  - [Kubernetes Container Rumtime 배경지식](https://www.mirantis.com/blog/cri-dockerd-faq-blog/){:target="_blank"}  
<br><br>  


### 쿠버네티스 (Kubernetes, K8s)  
컨테이너화된 애플리케이션(워크로드)과 서비스 관리를 자동화하는 확장 및 이식 가능한 오픈 소스 컨테이너 오케스트레이션 플랫폼  
<br>  

기존에는 보통 도커 이미지 당 하나의 컨테이너만 생성하고,  
문제가 발생하면 로그를 확인하고 수동으로 재실행하여 즉각적인 대처가 쉽지 않았지만,  
쿠버네티스를 사용하면서 컨테이너화된 애플리케이션의 배포, 확장 및 운영을 자동화할 수 있게 되었다.  

- 주요 특징  
  - 스토리지 프로비저닝  
  - 오토 스케일링  
  - 로드 밸런싱  
  - 컨테이너 문제 발생 시 자가 복구  
  - 여러 클라우드 공급업체에서 지원하며 이식성이 좋음  
    (AWS, MS Azure 등 클라우드 업체마다 서비스 사용 방법이 다 다름)  
<br>  

<div align="center">  
<img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/53eae38e-0950-4c89-8fb4-cf51e19785d3" width="500" height="270">  
</div>  
<br>  

쿠버네티스를 구축할 때는  
필수 요소들 사이에 버전 문제가 발생하지 않도록 [버전 차이(skew) 정책](https://kubernetes.io/ko/releases/version-skew-policy/){:target="_blank"}에 주의해야 하며,  
노드로 사용할 머신의 CPU, RAM, Storage 등 조건이 충족되는지도 확인해야 고생하지 않는다.  
(검증된 버전을 사용하는 것을 추천)  
<div align="center">  
<img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/aff05d60-a252-4274-b6a6-1ef3ce4fad1b" width="500" height="250">  
</div>  
<br><br>  


#### 오케스트레이션 (Orchestration)  
여러 개의 컴퓨터 시스템, 애플리케이션 및 서비스를 조율하고 관리하는 것  

빈도가 높고 반복할 수 있는 프로세스의 실행을 간소화 및 최적화하여 복잡한 작업과 워크플로를 간편하게 관리하도록 돕는다.  

- 오케스트레이션의 예  
  - 프로세스 오케스트레이션  
  - 애플리케이션 오케스트레이션  
  - 서비스 오케스트레이션  
  - 컨테이너 오케스트레이션  
  - 클라우드 오케스트레이션  
  - 보안 오케스트레이션  
<br>  
- 오케스트레이션 툴  
  - Kubernetes  
  - Docker Swarm  
  - Apache Mesos  
  - OpenShift  
  - [Amazon ECS (Elastic Container Service)](https://docs.aws.amazon.com/ko_kr/AmazonECS/latest/developerguide/Welcome.html){:target="_blank"}  
  - AKS (Azure Kubernetes Service)  
  - Google Kubernetes Engine (GKE)  
<br><br>  


#### [참고] end-to-end vs kubernetes  
- end-to-end  
  - 워크 프로세스가 연속적인 시퀀스로 작동하도록 설계되어 순차적인 흐름을 가짐  
  - 프로세스 구성 요소 간에 상호의존성이 강하여 때문에 이전 단계의 출력이나 완료에 의존하는 것이 많다.  
  - 하나의 요소에서 발생한 문제가 전체 프로세스를 중단시킬 수 있다.  
  - API나 내부 통신으로 커뮤니케이션  
  - 중간에 상호작용이 간단하므로 모듈화했을 때보다 속도가 더 빠름  
  <br>  

  (머신러닝에서 말하는 end-to-end learning은 데이터 입력부터 출력까지 파이프라인 네트워크 없이 신경망으로 한 번에 처리하도록 구현하는 것을 말한다.)  
<br>  
- kubernetes  
  - 특정 기능을 수행하는 작은 모듈로 나누어서 설계하여 유연성과 확장성이 뛰어나다.  
  - 모듈끼리 상호작용은 하지만 각 모듈은 독립적으로 수행될 수 있다.  
  - 서로의 내부 작동에 크게 의존하지 않아 하나의 모듈이 중지되어도 다른 모듈들은 계속 작동하는 경우가 많다.  
  - API를 통해 커뮤니케이션  
<br><br>  


### kubectl  
쿠버네티스 클러스터와 통신하기 위해 쿠버네티스에서 제공하는 명령 줄 도구  

개발자는 kubectl을 통해 클러스터의 apiserver에 요청을 전달할 수 있다.  

kubectl이 쿠버네티스 클러스터 아키텍처에 포함되지는 않지만 클러스터의 마스터 노드에도 설치해야하며,  
클러스터의 마스터 노드에서 포트포워딩을 해두면 클라이언트 노드에 kubectl을 설치한 후 마스터 노드의 kubeconfig를 복사해와서 원격으로 요청을 보낼 수 있다.  
(클라이언트 노드에서 원격으로 하지 않고 마스터 노드에서 직접 요청을 보내는 것도 가능)  

<div align="center">  
<img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/5f2d999c-9473-446b-a93e-ebdedd58d05f" width="500" height="300">  
</div>  
<br><br>  


### 쿠버네티스 클러스터 (Kubernetes Cluster)  
컨테이너화된 애플리케이션을 실행하고 관리하는 가상 or 물리적인 실제 노드(워커 머신)의 집합  

쿠버네티스를 배포하면 클러스터를 얻게 되는 것이며, 모든 클러스터는 최소 1개의 노드를 갖는다.  
노드 1개는 머신 1개를 의미하고 VM을 이용하여 가상으로 구축하거나, 물리적인 PC에 직접 구축할 수 있다.  
(PC가 많이 필요하므로 VM을 이용하여 자원을 할당해 구축하는 것이 좋을 것 같다.)  

노드의 종류에는 Control Plane의 마스터 노드와 Data Plane의 워커 노드가 있다.  
<div align="center">  
<img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/150e0824-a992-45a1-8fd6-98797d741782" width="500" height="240">  
</div>  

보통은 클라이언트 > 클러스터의 마스터 노드 > 클러스터의 워커 노드 > 엔드 유저로 생각하고 구축하는 것이 일반적이지만,  
마스터 노드에 모든 종류의 작업이 실행될 수 있도록 taint를 설정하여 하나의 노드로 간단하게 구축할 수도 있다.  
<br><br>  


### Control Plane - master node  
클러스터의 상태를 관리하고 명령어를 처리하는 역할  

<div align="center">  
<img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/30b0c699-6542-4a0c-ab71-fa13ab0772ba" width="500" height="300">  
</div>  
<br>  

- Scheduler  
각 노드의 자원 사용 상태를 관리  
- Controller manager  
여러 컨트롤러 프로세스를 관리  
- Cloud controller manager  
Cloud provider API를 위한 
- kube API server  
kubernetes의 리소스와 클러스터의 상태 관리 및 동기화를 위한 API 제공  
- etcd  
분산 key-value 저장소로 클러스터의 상태를 저장  
<br><br>  


### Node - worker node  
애플리케이션 컨테이너를 실행하는 역할  

<div align="center">  
<img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/72ca4a65-6570-4dd7-8c3c-89b49092949d" width="500" height="300">  
</div>  
<br>  

- kubelet  
  - 모든 노드에 기본적으로 설치되는 컴포넌트로, 각 노드에서 실행되는 기본 '노드 에이전트(Node Agent)' 역할을 함  
  - kube API server와 통신하며, 노드의 리소스 상태를 보고하고 관리  
  - CRI와 통신하며, 해당 노드에 있는 컨테이너의 수명주기를 관리  
- kube proxy  
- CRI (Container Runtime Interface)  
kubelet이 docker, containerd, cri-o, 등 다양한 컨테이너 런타임과 통신할 수 있도록 도와주는 인터페이스  
- pod  
단일 노드에 배포되는 1개 이상의 컨테이너 집합  
<br>  


### Container Runtime  
pod가 node에서 실행될 수 있도록 클러스터의 각 노드에 컨테이너 런타임을 설치해야 한다.  
<br>  

- dockershim  


  - deprecation (구식화)  
  프로그래머들이 새로운 표준을 준수하기 위해 코드를 변경할 시간을 가질 수 있도록 구식화된 소프트웨어 기능이 지금 당장은 제거되지 않고 남아있지만 나중에 삭제될 예정임을 나타낸다.  
- containerd  
- cri-o  

<br><br>  


### Kubernetes 볼륨  

https://easyitwanner.tistory.com/231

<br><br>  


### 쿠버네티스 클러스터 설정  
 필요한 컴포넌트(구성 요소)를 설치하여 쿠버네티스 클러스터를 설정하고 사용할 수 있다.  

하이 레벨의 설정까지는 원하지 않는다면 빠르게 쿠버네티스 클러스터를 설정하기 위해서 요구 사항에 맞게 도구를 선택해 사용하면 된다.  
<div align="center">  
<img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/8a08fc68-bac8-40f1-9d57-bda60d6679c0" width="500" height="300">  
</div>  
<br><br>  


#### minikube  
사용자가 개발 및 테스트 목적으로 단일 노드 클러스터에서 로컬로 Kubernetes를 쉽게 실행할 수 있도록 해줌  

- DNS, NodePort, ConfigMap, 대시보드 등 다양한 Kubernetes 기능을 지원  
- 여러 VM을 실행하여 노드 클러스터를 에뮬레이션할 수 있음  
<br><br>  


#### k3s  
IoT 및 Edge 컴퓨팅용으로 구축된 매우 가볍고 인증된 Kubernetes 배포판  

- 리소스가 제한된 엣지 컴퓨팅, IoT, CI/CD 환경 및 개발 시나리오에 이상적  
- 기본이 아닌 기능 및 선택적 구성 요소를 제거하여 설치 공간을 최소화  
- 기본 스토리지 메커니즘으로 SQLite3을 기반으로 하는 경량 스토리지 백엔드 내장  
<br><br>  


#### microk8s  
경량화된 소규모 엔터프라이즈 K8s 클러스터  

- 단일 노드 버전을 디폴트로 설정하며, 노드를 추가하여 다중 노드로 확장도 가능  
- IoT, 엣지 컴퓨팅, 소규모 프로덕션 환경에 적합  
<br><br>  


#### kubeadm  
Kubernetes 클러스터를 빠르게 설정하기 위한 도구  

[Kubeadm 설치하기](https://kubernetes.io/ko/docs/setup/production-environment/tools/kubeadm/install-kubeadm/){:target="_blank"}  

- Kubernetes 프로젝트의 일부  
<br><br>  


### 쿠버네티스 아키텍처  
- 2022 쿠버네티스를 위한 제품  
<div align="center">  
<img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/9df64cd6-488b-44b9-b366-acd11394e9ff" width="500" height="300">  
</div>  
<br>  

- 2023 쿠버네티스 표준 구성  
<div align="center">  
<img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/bd4b1268-ab06-4935-9e24-f8f76e2b7b6b" width="500" height="700">  
</div>  
