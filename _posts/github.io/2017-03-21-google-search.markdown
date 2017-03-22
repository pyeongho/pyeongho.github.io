---
layout: post
title: jekyll 구글 검색 허용하기
category: github.io
comments: true
description: jekyll github.io 를 만들면 구글 검색을 허용해야 한다.
tags:
    - jekyll    
    - google
    - search
---


#### 지킬로 만들면 구글 검색이 안됩니다.  

#### 1. 구글 검색 허용하기
  - 구글 웹마스터 도구(Search Console)에 속성 추가 및 인증
    - [구글 웹 마스터 도구 접속](https://www.google.com/webmasters/tools/home?hl=ko)
    - 속성추가 버튼을 선택
    - 자신의 jekyll 블로그 주소를 입력하여 속성 추가 (https://pyeongho.github.io/)
    - 구글에서 제공하는 html 다운로드(ex googlee420f3ff29beb995.html) 하여 루트 폴더에 올리고 푸시 
      - 테스트는 https://pyeongho.github.io/googlee420f3ff29beb995.html 하여 내용이 나오는지 확인
    - 구글 확인 버튼 클릭

  - sitemap.xml 등록하기 
    - _config.yml 에 url 을 추가 합니다.
    ```
    url: https://pyeongho.github.io
    ```
    - sitemap.xml 을 만들어서 루트 폴더에 올리고 푸시
      - [sitemap.xml 예제](https://github.com/pyeongho/pyeongho.github.io/blob/master/sitemap.xml)
    - 구글 웹마스터 콘솔에 sitemap.xml 을 테스트후 등록 하여 문제 없도록 합니다.
    - 등록하면 상태가 접수 중으로 나옵니다. 시간이 지나면 구글 검색으로 테스트 하겠습니다. 


##### 참고
[초보몽키의 개발로그](https://wayhome25.github.io/etc/2017/02/20/google-search-sitemap-jekyll/)
