---
title:  "Docker 명령어 정리"
excerpt: "도커 명령어 정리"

categories:
  - MLOps
tags:
  - [Docker, Docker-Compose, Command]

toc: true
toc_sticky: true

published: true

date: 2022-11-05
last_modified_at: 2023-06-01
---

## 도커 명령어  
### Docker command-line reference  
https://docs.docker.com/engine/reference/commandline/cli/  
<br>  

### Docker  
- 도커 버전 확인  
  ```bash  
  docker --version
  ```  
<br>  

### Docker Image  
- 도커 이미지 검색  
  ```bash  
  docker search {image}
  ```  
- 도커 이미지 다운로드  
  ```bash  
  docker pull {image_name:tag}
  ```  
  (tag의 default 값은 latest)  
  docker hub에 공개되어 있는 이미지를 다운받는다. (레지스트리 서버의 default 도메인 : dockerhub)  

- 현재 Host 머신에 존재하는 이미지 목록 확인  
  ```bash  
  docker images
  ```  
- 특정 이미지 삭제  
  ```bash  
  docker rmi {image_repository:tag}
  ```  
  ```bash  
  docker rmi {image_id}
  ```  
  image_id를 통해 이미지를 삭제할 때 같은 image_id를 가진 리포지토리들이 여러 개 태그되면 오류가 발생한다. (force 옵션을 통해 강제로 해결 가능)  

- 특정 이미지 강제 삭제  
  ```bash  
  docker rmi -f {image_id}
  ```  
  해당 image_id를 가진 이미지들이 모두 삭제된다.  
<br>  

### Dockerfile  
- Dockerfile을 통한 이미지 빌드  
  ```bash  
  docker build -t {image_name} .
  ```  
- Dockerfile을 다른 파일명으로 작성 후 이미지 빌드  
  ```bash  
  docker build -t {image_name} -f {Dockerfile_name} .
  ```  
  명령어 마지막에 '.' 은 Dockerfile이 있는 현재 디렉토리로 빌드 컨텍스트가 된다.  
  빌드 컨텍스트를 기준으로 상위 폴더로 올라가게 되면 빌드 컨텍스트에서 벗어나는 것이므로 오류 발생의 원인이 된다.  
  (ex. Dockerfile 내 COPY 명령문에서 상대경로 작성 시 현재 디렉토리나 하위 디렉토리만 사용)  
<br>  

### Docker Container  
- 실행중인 컨테이너 목록 조회  
  ```bash  
  docker ps
  ```  
- 컨테이너 전체 목록 조회  
  ```bash  
  docker ps -a
  ```  
- 컨테이너 생성  
  ```bash  
  docker create -it --name {container_name} {image_repository}
  ```  

  | Option | Description |
  | ------ | ----------- |
  | -i     | 사용자가 입출력을 할 수 있는 상태로 설정 (interactive) |
  | -t     | 가상 터미널 환경을 에뮬레이션 |

- 컨테이너 삭제  
   ```bash  
   docker rm {container_name}
   ```  
- 컨테이너 실행  
  ```bash  
  docker start {container_name}
  ```  
- 컨테이너 종료  
  ```bash  
  docker stop {container_name}
  ```  
- 컨테이너 생성 및 실행 시작, 포트포워딩  
  ```bash  
  docker run -itd --name {container_name} -p {host_port}:{container_port} {image_repository} /bin/bash
  ```  
  호스트 IP의 포트로 접근하면 컨테이너의 포트로 연결된다.  

  | Option | Description |
  | ------ | ----------- |
  | -d     | 일반 프로세스가 아닌 데몬 프로세스(백그라운드) 형태로 실행해 프로세스가 끝나도 컨테이너 유지 |

- 호스트 머신의 파일을 컨테이너 안으로 복사  
  ```bash  
  docker cp {host_file_path} {container_name}:{container_path}
  ```  
- 컨테이너 안에 있는 파일을 호스트 머신으로 복사  
  ```bash  
  docker cp {container_name}:{container_path} {host_file_path}
  ```  
- 특정 컨테이너 접속  
  ```bash  
  docker exec -it {container_name} /bin/bash
  ```  
  /bin/bash 프로세스가 생성된다.  

- 컨테이너 유지 및 접속 종료  
  ```bash  
  ctrl + P, ctrl + Q
  ```  
- 컨테이너 종료 및 접속 종료  
  ```bash  
  exit
  ```  
  ```bash  
  ctrl + D
  ```  
<br>  

#### 포트가 이미 바인딩 되어 있어서 프로그램을 실행할 수 없는 경우  
- 특정 포트를 사용하는 프로세스 찾기  
  ```bash  
  sudo lsof -i :{port}
  ```  
- 강제 종료  
  ```bash  
  sudo kill -9 {pid}
  ```  
<br>  

### Docker-Compose  
- 도커 컴포즈 버전 확인  
  ```bash  
  docker-compose --version
  ```  
<br>  

### docker-compose.yml  
- docker-compose.yml 파일에 작성된 컨테이너 생성 및 백그라운드 실행  
  ```bash  
  docker-compose up -d
  ```  
- 사용자 지정 파일명.yml 파일에 작성된 컨테이너 생성 및 백그라운드 실행  
  ```bash  
  docker-compose -f {file_name.yml} up -d
  ```  
  docker-compose.yml 파일이 존재하는 경로로 이동하여 실행한다.  
