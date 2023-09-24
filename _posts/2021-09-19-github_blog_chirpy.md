---
title:  "깃허브 블로그 시작하기"
excerpt: "깃허브 블로그 생성 방법"

categories:
  - Blog
tags:
  - [Git, Github, Blog]

toc: true
toc_sticky: true
 
date: 2021-09-19
last_modified_at: 2021-09-27
---

  
<code>{GH_USERNAME}.github.io</code> 리포지토리를 생성하여 블로그를 만들면,  
테마를 적용하기 위해 <code>jekyll-theme-chirpy</code>에서 다운받은 코드를 clone한 폴더에 압축해제한 후, push했을 때  
Actions에서 빌드하는 도중에 계속 오류가 발생하였다.<br>  
  - .travis.yml, _posts 삭제  
    - 현재 버전에서 Gemfile.lock은 존재X (.gitignore에 해당 파일명이 추가되어 있음)  
  - .github/workflows/pages-deploy.yml.hook 파일의 **.hook**을 지우고, 해당 폴더의 나머지 파일 삭제  
  - Settings > Pages에서 Source와 Branch를 변경  
  - _config.yml에서 url 등을 변경  
  - .github/workflows/pages-deploy.yml 파일에서 버전을 수정  

위의 방법을 시도해보았으나 오류는 해결되지 않았다.<br>  
인터넷에서 검색해본 방법들로는 각자 환경에 차이가 있는 것 같아  
**jekyll-theme-chirpy/_posts/20xx-xx-xx-getting-started.md** 파일을 참조하여 해결하였다.  

## 준비  
여러 테마들 중 **chirpy** 테마를 선택,  
PC에 **Git**이 설치되어 있고 깃허브를 사용중인 상태로 가정<br>  
1. <code>Ruby</code>, <code>RubyGem</code>, <code>Jekyll</code>, <code>Bundler</code> 설치  
    - https://rubyinstaller.org/downloads/ 에서 윈도우용 Ruby Installer 다운로드  
    (https://jekyllrb.com/docs/installation/ 참고)  
    - Installer 실행 후 next를 눌러 Ruby 설치를 완료하고 <code>ruby -v</code> 명령어로 설치 확인  

## 사이트 생성하기  
1. 깃허브에서 <code>jekyll-theme-chirpy</code>를 검색 후 최신 소스를 자신의 깃허브에 Fork  
2. fork 해온 리포지토리의 이름을 <code>{GH_USERNAME}.github.io</code>로 변경  
3. 리포지토리를 로컬로 clone  
4. Ruby prompt를 관리자 권한으로 실행 후 로컬 리포지토리 경로로 이동,  
   테마에 필요한 라이브러리 설치  
    ```ruby  
    cd {로컬 repo 폴더 경로}
    gem install jekyll bundler  
    bundle install  
    ```  
5. 초기화 (<code>bash tools/init.sh</code> 명령어는 윈도우 환경에서 사용 불가 → 수동으로 진행)  
    - <code>.github/workflows/pages-deploy.yml.hook</code> 에서 <code>.hook</code>확장자를 제거  
    - <code>.travis.tml</code> 파일 삭제  
    - <code>Gemfile.lock</code> 파일 삭제 (.gitignore에 들어있음)  
6. _config.yml 수정  
    - <code>timezone</code>  
    - <code>title</code>  
    - <code>tagline</code>  
    - github <code>username</code>  
    - social <code>name</code>, <code>email</code>, <code>links</code>  
    - <code>avatar</code>  
7. 배포 전 로컬 서버에서 적용 확인  
    - 서버 실행  
      ```ruby  
      bundle exec jekyll serve  
      ```  
    - <code>localhost:4000</code>로 접속하여 결과 화면 확인  
8. 배포  
    ```bash  
    git add *  
    git commit -m "커밋 메시지"  
    git push  
    ```  

아바타, 배포 기본 설정, Actions  
