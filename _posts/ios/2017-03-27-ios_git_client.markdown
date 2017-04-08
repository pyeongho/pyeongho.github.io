---
layout: post
title: github.io  사진 업로드용 앱 개발 1회차
category: iOS
comments: true
description: gitugb.io 지킬에 사진업로드 쉽게 하는 앱개발
tags:
    - iOS
    - 스위프트
    - carthage
---

### iOS 에서 github.io에 사진을 첨부한 글을 포스팅한다.
 
#### 1. 공부할 자료 수집
  - 깃 라이브러리 
    - [https://github.com/libgit2/objective-git#carthage](https://github.com/libgit2/objective-git#carthage)
  - [https://github.com/SwiftGit2/SwiftGit2](https://github.com/SwiftGit2/SwiftGit2)
  -  carthage 사용법을 알아야 한다.
    - [https://swifter.kr/2016/04/24/carthage-카르테지-설치-방법/](https://swifter.kr/2016/04/24/carthage-카르테지-설치-방법/)


 ####2. carthage 사용법 
  - homebrew로 설치 하기
     - 설치 안되어 있으면
       - $ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
  
  - $ brew update
  - $ brew install carthage 
  - Cartfile 파일 생성하기
    - 프로젝트 루트에 Cartfile 파일을 만들어서
    - 라이브러리 사용 설명서에 나온 주소를 입력 한다.
    - 정확한 문법은 인터넷에서 확인해 주세요
    - 예)  github "libgit2/objective-git"
  - 프로젝트 루트에서 명령어 실행
    - $ carthage update 
  - 라이브러리에 따라서 헤더 위치를 추가 해야합니다.
    - 프로젝트 세팅 -> 빌드 세팅 -> Header Search Path (all,level)  
  - import ObjectiveGit 추가후 컴파일 완료  
