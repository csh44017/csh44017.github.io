---
title:  "깃허브 블로그 시작하기"
excerpt: "깃허브 블로그 생성 방법"

categories:
  - Note
tags:
  - [Github, Blog, Markdown]

toc: true
toc_sticky: true
 
date: 2022-10-19
last_modified_at: 2022-10-27
---

  
**<GH_USERNAME>.github.io** 리포지토리를 생성하여 블로그를 만들면,  
테마를 적용하기 위해 jekyll-theme-chirpy에서 다운받은 코드를 clone한 폴더에 압축해제한 후, push했을 때  
Actions에서 빌드하는 도중에 계속 오류가 발생하였다.<br>  
  - .travis.yml, _posts 삭제  
    - 현재 버전에서 Gemfile.lock은 존재X (.gitignore에 해당 파일명이 추가되어 있음)  
  - .github/workflows/pages-deploy.yml.hook 파일의 **.hook**을 지우고, 해당 폴더의 나머지 파일 삭제  
  - Settings > Pages에서 Source와 Branch를 변경  
  - _config.yml에서 url 등을 변경  
  - .github/workflows/pages-deploy.yml 파일에서 버전을 수정  

위의 방법을 시도해보았으나 오류는 해결되지 않았다.<br>  
인터넷에서 검색해본 방법들로는 각자 환경에 차이가 있는 것 같아  
jekyll-theme-chirpy/_posts/20xx-xx-xx-getting-started.md 파일을 참조하여 해결하였다.  

# 준비  
여러 테마들 중 **chirpy** 테마를 선택,  
PC에 **Git**이 설치되어 있고 깃허브를 사용중인 상태로 가정<br>  
1. **Ruby**, **RubyGem**, **Jekyll**, **Bundler** 를 설치  

# 사이트 생성하기  
깃허브에서 jekyll-theme-chirpy를 검색 후 최신 소스를 자신의 깃허브에 Fork 한다.  


https://github.com/cotes2020/jekyll-theme-chirpy  
