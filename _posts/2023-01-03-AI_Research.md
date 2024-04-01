---
title:  "AI 카테고리 살펴보기"
excerpt: "여러 분야로 나눠지는 AI의 방향을 정하는데 참고할 수 있는 카테고리 정리"

categories:
  - ML
tags:
  - [AI, ML]

toc: true
toc_sticky: true

published: true

date: 2023-01-03
last_modified_at: 2023-09-07
---

## AI 직업  
크게 보았을 때 AI 관련 직업은 '데이터 분석가', '데이터 엔지니어', '데이터 사이언티스트', 'AI Reseaucher(모델러)'이다.  
- 데이터 분석가  
  데이터를 비즈니스에 활용할 수 있도록 분석  
  (Tableau, Power BI, Python, R, SQL, 시각화 도구)  
- 데이터 엔지니어  
  데이터를 수집하고 저장, 처리할 수 있는 아키텍처(데이터 파이프라인, DB, ETL)를 관리  
  (Hadoop, Spark, Kafka, Java, Python, SQL)  
- 데이터 사이언티스트  
  데이터를 분석 및 시각화하면서 의미있는 데이터로 정제하고 예측 모델을 구현  
  (Python, R, SQL, Tensorflow, PyTorch)  
- AI Researcher  
  모델 고도화, 경량화, 최신 기법 적용  
  (Python, C, C++, Tensorflow, PyTorch)  
<br><br>  

### 인공지능 소개  
- 인공지능(AI, Artificial Intelligence)  
  인간의 학습 능력, 추론(Inference) 능력, 지각 능력을 가진 컴퓨터 시스템을 만드는 기술로 인간의 사고를 모방하는 모든 것  
- 머신러닝(ML, Machine Learning)  
  인공지능의 한 분야로 사람이 정한 특징 추출 방법과 모델을 통해 데이터를 기반으로 스스로 규칙을 학습해서 추론할 수 있도록 하는 기술  
- 딥러닝(DL, Deep Learning)  
  머신러닝 알고리즘 중 인공 신경망을 기반으로 한 방법들을 통칭하는 것으로 빅데이터 학습에 적합하다.  
<div align="center">  
  <img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/ca9c75a5-856e-418c-9f13-0fd155ca8a2d" width="400" height="300">  
</div>  
<br>  

### 인공지능의 단계  
- 약인공지능  
특정 분야에 유용한 도구로만 활용 가능한 인공지능  
- 강인공지능  
인간을 완벽하게 모방하여 다양한 분야에서 활용 가능한 인공지능  
- 초인공지능  
강인공지능에서 자아를 가진 인공지능  
<br>  

### 학습 방법  
- 지도 학습  
  레이블된 데이터를 기반으로 모델을 훈련  
- 준지도 학습  
  레이블이 지정된 데이터와 레이블이 지정되지 않은 데이터를 함께 사용하여 모델을 훈련  
- 비지도 학습  
  레이블이 지정되지 않은 데이터를 기반으로 모델을 훈련  
- 강화 학습  
  에이전트가 환경과 상호작용하여 시행착오를 겪으면서 받는 보상 신호를 통해 학습  
<br>  

### 프레임워크  
- Scikit-Learn  
  파이썬 머신러닝 라이브러리  
- Tensorflow  
  구글에서 개발한 오픈 소스 딥러닝 라이브러리  
  Keras API와 함께 사용  
  (tf.Tensor 데이터 유형을 사용하여 다차원 배열로 표현)  
- Pytorch  
  Facebook의 AI Research lab(FAIR)에서 개발한 오픈 소스 딥러닝 프레임워크  
  (torch.Tensor 데이터 유형을 사용하여 다차원 배열로 표현)  
- Caffe (Convolutional Architecture for Fast Feature Embedding)  
  이미지 관련 작업에 많이 사용되는 프레임워크  
- Theano  
  행렬같은 수학 계산을 위한 라이브러리  
<br>  

### 학습 모델  
- 회귀  
- 분류  
- 이미지 처리  
- 자연어 처리(NLP)  
- 음성 모델  
- 거대언어 모델(LLM)  
- 생성형 모델  
<br>  

### 학습 데이터  
- 정형 데이터 (Structured Data)  
  미리 정해놓은 형식에 따라 고정된 필드에 저장된 데이터  
- 비정형 데이터  
  정의된 구조가 없는 동영상, 오디오, 사진, 문서, 등  
- 반정형 데이터  
  데이터의 구조 정보가 데이터와 함께 제공되어 형식이 변경될 수 있는 데이터  
  (XML, HTML, JSON, ...)  
- 시계열 데이터  
  시간 간격으로 수집된 데이터  
- 이미지 데이터  
  그리드에 배열된 픽셀 형태의 정보  
- 텍스트 데이터  
  텍스트 정보
- 오디오 데이터  
  음성 정보  
<br><br>  

## 이미지 처리 태스크  
- 이미지 분류(Image Classification)  
  이미지가 무엇인지 분류  
- 이미지 분류 + Localiztion  
  이미지 내에 어떤 객체가 있는지 분류하고 어디에 위치하는지 파악  
- 이미지 분할(Image Segmentation)  
  - Semantic Segmentation  
    이미지에서 개체에 대한 클래스만 식별 (label에서 개체간에 구분X)  
  - Instance Segmentation  
    이미지에서 여러 개체들을 식별하고 각 개체의 픽셀을 분할 (클래스 식별X)  
  - Panoptic Segmentation  
    이미지에서 픽셀의 클래스를 구분하고 인스턴스 분할을 동시에 수행  
<div align="center">  
<img src="https://github.com/csh44017/csh44017.github.io/assets/77605589/81178074-0267-4476-9e53-9839c983fbfa" width="400" height="300">  
</div>  

- 객체 탐지(Object Detection)  
  객체의 위치를 식별하고 분류  
- 객체 추적(Object Tracking)  
  객체를 식별하고 추적  
<br>  
- 얼굴 인식(Face Recognition)  
  얼굴 특징을 기반으로 개인을 식별  
- 얼굴 표정 인식(Facial Expression Recognition)  
  얼굴 표정을 감지하고 감정을 분석  
- 나이 성별 추정(Age and Gender Estimation)  
  개인의 나이와 성별을 추정  
<br>  
- 이미지 캡션(Image Captioning)  
  이미지에 대한 자연어 설명 또는 캡션 생성  
<br>  
- 초해상도(Image Super-Resolution)  
  의료 영상이나 위성 영상 등에서 이미지의 품질과 해상도를 향상시키는 것  
- 이미지 노이즈 제거  
  이미지에서 노이즈와 아티팩트를 제거  
- Image Inpainting  
  이미지에서 누락되거나 손상된 부분을 채움  
<br>  
- Style Transfer  
  한 이미지의 예술적 스타일을 다른 이미지의 콘텐츠에 적용  
<br>  
- 이미지 등록(Image Registration)  
  이미지를 정렬하고 오버레이하여 비교하거나 결합  
- Panorama Stitching  
  여러 이미지를 하나의 광각 이미지나 파노라마 이미지로 결합  
<br>  
- 필기 인식  
  필기한 문자를 기계가 읽을 수 있는 텍스트로 변환  
- 문서 레이아웃 분석  
  문서를 텍스트, 이미지, 표 등의 영역으로 분할하고 구조화된 정보를 추출  
  OCR(광학 문자 인식)에 필요  
- 컨텐츠 기반 이미지 검색(Content-Based Image Retrieval, CBIR)  
  시각적 컨테츠를 기반으로 DB에서 이미지를 검색  
<br>  
- 이상 감지(Anomaly Detection)  
  불규칙하거나 예상치 못한 패턴을 식별  
- 깊이 추정(Depth Estimation)  
  이미지의 각 픽셀에 대한 깊이나 거리를 예측  
<br><br>  

## 자연어 처리 태스크  
- 감정 분석 (Sentiment Analysis)  
  텍스트 데이터에서 감정을 분석  
- 텍스트 분류  
  다양한 유형의 게시물에서 스팸 필터링, 언어 감지, 주제 분류  
- 텍스트 생성  
  시나리오, 소설, 기사, 등의 텍스트를 생성  
- 번역 (Translation)  
  언어간에 정보를 번역하고 이해  
- 자동 요약  
  긴 텍스트 문서를 자동으로 요약  
- 텍스트 마이닝  
  대량의 텍스트 데이터에서 유용한 정보를 추출  
- 대화형 AI  
  챗봇이나 가상 비서를 만들어 사용자와 대화  
- 개체명 인식 (Named Entity Recognition, NER)  
  텍스트에서 특정 개체명(사람, 장소, 날짜, 등)을 식별하고 추출  
- 검색  
  검색 엔진, 문서 검색 및 추천 시스템 개발  
