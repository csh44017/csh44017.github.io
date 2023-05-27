---
title:  "git 명령어 정리"
excerpt: "깃과 깃허브를 사용하기 위한 명령어 정리"

categories:
  - Git
tags:
  - [Git, Github, Command]

toc: true
toc_sticky: true
 
date: 2023-05-25
last_modified_at: 2023-05-25
---

## Git  
컴퓨터 파일의 변경사항을 추적하고 여러 명의 사용자들 간에 해당 파일들의 작업을 조율하기 위한 분산 버전 관리 시스템  
(자신의 환경에 맞게 별도로 설치 필요)<br><br>  
  
## Github  
원격 저장소<br><br>  
  
## Git 명령어  
### Check  
- 현재 상태 확인  
  ```bash  
  git status
  ```  
  수정 사항 및 상태를 확인할 수 있다.<br><br>  
- 전체 로그 확인  
  ```bash  
  git log
  ```  
  커밋 로그들과 각 해시 값을 확인할 수 있다.  
  로그가 많은 경우 enter를 통해 다음 라인을 확인할 수 있으며, q로 빠져나올 수 있다.<br><br>  
  
### Config  
- config 리스트 확인  
  ```bash  
  git config --list
  ```  
  <br>  
- push용 이름 및 이메일 설정  
  ```bash  
  git config --global user.name "{username}"
  ```  
  ```bash  
  git config --global user.email "{id@email.com}"
  ```  
  깃을 처음 설치한 상태에서는 username과 email이 세팅되어 있지 않으므로 push할 때 사용할 이름과 이메일을 알려주어야 한다.  
  '--global' 옵션을 통해 특정 프로젝트가 아닌 전체 깃에 적용하면 한번만 작업해주면 된다.<br><br>  
- config 삭제  
  ```bash  
  git config --unset --global {username}
  git config --unser --global {user.email}
  ```  
  