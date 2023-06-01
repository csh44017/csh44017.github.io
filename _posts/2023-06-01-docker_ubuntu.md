---
title:  "우분투에서 Docker 사용하기"
excerpt: "우분투에서 도커를 통해 Kafka 메시지를 InfluxDB에 저장"

categories:
  - [Ubuntu, Docker]
tags:
  - [Docker, Docker-Compose, Ubuntu, Kafka, InfluxDB]

toc: true
toc_sticky: true

date: 2023-06-01
last_modified_at: 2023-06-01
---

## 우분투에서 도커 사용하기  
- 운영 체제  
  Ubuntu 20.04 LTS  
- 컨테이너  
  - Zookeeper  
  - Kafka  
  - InfluxDB  
  - Kafka-Consumer & InfluxDB write 

### 1. docker 설치  
(docker 설치 방법)  
<br><br>  

### 2. docker-compose 설치  
(docker-compose 설치 방법)  
<br><br>  

### 3. sudo 권한 없이 docker 명령어 실행하기  
(방법)  
<br><br>  

### 4. Kafka, Influxdb 서버 구축  
zookeeper, kafka, influxdb는 공개되어 있는 이미지를 다운받아 베이스 이미지로 사용  

- docker-compose.yml  
  ```yml  
  version: '2.5.0'
  services:
    zookeeper:
      image: wurstmeister/zookeeper
      container_name: zookeeper
      ports:
        - "2181:2181"
      restart: always
    kafka:
      image: wurstmeister/kafka
      container_name: kafka
      ports:
        - "9092:9092"
      environment:
        KAFKA_ADVERTISED_HOST_NAME: 211.197.16.44
        KAFKA_ZOOKEEPER_CONNECT: zookeeper:2181
      volumes:
        - /var/run/docker.sock:/var/run/docker.sock
      restart: always
    influxdb:
      image: influxdb:2.0
      container_name: influxdb
      ports:
        - "8086:8086"
      volumes:
        - type : bind
          source: /media/petabrew/hdd1/var/lib/influxdb/data
          target: /var/lib/influxdb2
        - type: bind
          source: /media/petabrew/hdd1/var/lib/influxdb/config
          target: /etc/influxdb2
      restart: always
  ```  
  [컨테이너 백그라운드 실행]  
  ```bash  
  docker-compose up -d
  ```  
<br><br>  

### 5. 소스코드 실행  
- run.sh  
  ```bash  
  python3 /app/kafka_consumer_raw/main.py &
  python3 /app/kafka_consumer_target/main.py
  ```  
  '&'를 붙이면 파이썬 파일을 백그라운드로 실행하여 컨테이너에서 2개의 파이썬 코드를 동시에 실행시킬 수 있다.  
- Dockerfile  
  ```Dockerfile
  FROM python:3.8

  COPY /kafka_consumer_raw/ /app/kafka_consumer_raw/
  COPY /kafka_consumer_target/ /app/kafka_consumer_target/
  COPY run.sh /app/

  RUN pip install kafka-python==2.0.2 influxdb-client==1.35

  WORKDIR /app
  CMD ["/bin/bash", "/app/run.sh"]
  ```  
  [도커 이미지 빌드]  
  ```bash  
  docker build -t {image_name} .
  ```  
  [빌드한 이미지로 컨테이너 생성 및 실행]
  ```bash  
  docker run --name {container_name} -p 5001:80 -d --restart always {image_name}
  ```  
  호스트 머신의 5001 포트가 컨테이너 내부의 80 포트로 매핑되도록 지정  
  컨테이너가 중지되면 백그라운드로 항상 재시작  
