---
title:  "Git 명령어 정리"
excerpt: "깃과 깃허브를 사용하기 위한 명령어 정리"

categories:
  - MLOps
tags:
  - [Git, Github, Command]

toc: true
toc_sticky: true

published: true
 
date: 2022-10-07
last_modified_at: 2022-10-15
---

## 개념  
### Git  
컴퓨터 파일의 변경사항을 추적하고 여러 명의 사용자들 간에 해당 파일들의 작업을 조율하기 위한 분산 버전 관리 시스템  
(자신의 환경에 맞게 별도로 설치 필요)  
  
### Github  
원격 저장소  
<br><br>  

## Git 명령어  
### Check  
- 현재 상태 확인  
  ```bash  
  git status
  ```  
  수정 사항 및 상태를 확인할 수 있다.  
- 전체 로그 확인  
  ```bash  
  git log
  ```  
  커밋 로그들과 각 해시 값을 확인할 수 있다.  
  로그가 많은 경우에는 'enter'를 통해 다음 라인을 확인할 수 있으며, 'q'로 빠져나올 수 있다.  
  
### Config  
- config 리스트 확인  
  ```bash  
  git config --list
  ```  

- push용 사용자 이름 설정  
  ```bash  
  git config --global user.name "{username}"
  ```  
- push용 이메일 설정
  ```bash  
  git config --global user.email "{id@email.com}"
  ```  
  깃을 처음 설치한 상태에서는 username과 email이 세팅되어 있지 않으므로 push할 때 사용할 이름과 이메일을 알려주어야 한다.  
  '--global' 옵션을 통해 특정 프로젝트가 아닌 전체 깃에 적용하면 한번만 작업해주면 된다.  
- config의 사용자 이름 삭제  
  ```bash  
  git config --unset --global {username}
  ```  
- config의 이메일 삭제  
  ```bash  
  git config --unser --global {user.email}
  ```  
  
### Remote Repository  
- 등록된 원격 저장소 이름 확인  
  ```bash  
  git remote
  ```  
- 원격 저장소 확인  
  ```bash  
  git remote -v
  ```  
  push용과 fetch용 주소도 알려준다.  
- 특정 원격 저장소의 자세한 정보 확인  
  ```bash  
  git remote show {name}
  ```
- 원격 저장소 등록  
  ```bash  
  git remote add {name} {HTTPS or SSH}
  ```  
- 등록된 원격 저장소의 이름 변경  
  ```bash  
  git remote rename {name} {modified name}
  ```  
- 등록된 특정 원경 저장소 제거  
  ```bash  
  git remote rm {name}
  ```  
  
### Branch check  
- 로컬 저장소의 브랜치 확인  
  ```bash  
  git branch
  ```  
- 원격 저장소의 브랜치 확인  
  ```bash  
  git branch -r
  ```  
- 로컬 및 원격 저장소의 브랜치 확인  
  ```bash  
  git branch -a
  ```  
  
### Branch  
- 브랜치 생성하기  
  ```bash  
  git branch {branch}
  ```  
- 원격 브랜치 업데이트  
  ```bash  
  git remote update
  ```  
- 원격 브랜치 가져오기  
  ```bash  
  git checkout -t {origin/branch}
  ```  
- 브랜치 이동하기  
  ```bash  
  git switch {branch}
  ```  
  git 2.23 버전 이후에는 'git checkout' 대신 'git switch'를 사용한다.  
- 브랜치를 만들면서 브랜치 변경  
  ```bash  
  git switch -c {branch}
  ```  

### Branch delete  
- 브랜치 삭제  
  ```bash  
  git branch -d {branch}
  ```  
- 브랜치 강제 삭제  
  ```bash  
  git branch -D {branch}
  ```  

### Download  
- 깃 저장소 생성하기  
  ```bash  
  git init
  ```  
  '.git' 파일이 생성되면서 해당 폴더가 로컬 리포지토리가 된다.  
- 저장소 복제 및 다운로드  
  ```bash  
  git clone {HTTPS or SSH}
  ```  
  원격 저장소를 '리포지토리 명' 폴더로 통째로 복제하고 원격 주소를 연결한다.  
- 원격 저장소의 변경 내용을 현재 디렉토리로 가져오기  
  ```bash  
  git pull {HTTPS or SSH}
  ```  
  세팅되어 있는 원격 주소를 사용해서 가져온다.  

### Upload  
- 저장소에 변경사항 추가 (파일 지정)  
  ```bash  
  git add {file} {file} ...
  ```  
- 저장소에 변경사항 모두 추가 (.gitignore에 기재된 것을 고려)  
  ```bash  
  git add .
  ```  
- 저장소에 변경사항 모두 추가 (.gitignore에 기재된 것을 고려하지 않음)  
  ```bash  
  git add *
  ```  
- Staging Area에 add한 파일을 내리기  
  ```bash  
  git reset {file}
  ```  
  '--mixed' 옵션이 default로 되어있다.  
- 파일의 수정 내역을 이전 버전으로 되돌리기  
  ```bash  
  git checkout --{file}
  ```  
  저장해놓은 파일의 수정 내역이 사라진다.  
- 커밋 생성  
  ```bash  
  git commit -m "{message}"
  ```  
- 커밋 메시지 수정  
  ```bash  
  git commit --amend
  ```  
  unix 편집기를 통해 수정할 수 있다.  
  ('a' : insert)  
  ('esc + wq!' : 저장 후 나가기)  
- 변경 사항 원격 서버 업로드  
  ```bash  
  git push origin {branch}
  ```  
  origin 주소의 리포지토리로 업로드한다.  

### Merge  
- 마지막 pull 이후 원격 저장소 또는 브랜치에 적용된 변경사항 확인  
  ```bash  
  git fetch
  ```  
- merge 하기 전에 변경 내용 비교  
  ```bash  
  git diff {branch} {another branch}
  ```  
- 현재 브랜치에 다른 브랜치 수정사항 병합  
  ```bash  
  git merge {another branch}
  ```  

### Version  
- 특정 커밋 버전으로 이동  
  ```bash  
  git reset --soft {hash}
  ```  
- 특정 커밋 버전으로 되돌리며 이후 커밋 삭제  
  ```bash  
  git reset --hard {hash}
  ```  
  커밋 로그들도 같이 사라진다.  
- 원격 저장소에 로컬 저장소 버전을 강제로 저장  
  ```bash  
  git push -f
  ```  
  버전 이동 시 로컬 저장소와 원격 저장소의 내용이 달라 그냥 push 되지 않을 경우 사용한다.  
