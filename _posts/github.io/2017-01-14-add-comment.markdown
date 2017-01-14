---
layout: post
title: jekyll 댓글 추가 하기
category: github.io 사용법
comments: true
description: jekyll 에서 댓글 기능 추가 하기
tags:
    - jekyll    
    - 댓글
    - disqus
---



#### 1. Disqus 를 이용하여 댓글 기능을 추가, 순서는 크게 3단계
 - Disqus 가입하기
 - Disqus 설정하기
 - github 연동하기

#### 2. Disqus 가입하기
 - [Disqus 홈페이지](https://disqus.com/) 에서 회원 가입을 한다.
 - 계정은 페이스북 트위터 구글을 사용 가능 합니다.
 - 회원 가입이 완료 되면 아래와 같은 이미지 가나옵니다.
   ![follow]({{ site.url }}assets/images/post/comment_follow.PNG)
 - 적당히 잘 선택해 줍니다.
 - 3개를 선택하면 상단에 cotinue 
 ![continue](https://github.com/pyeongho/pyeongho.github.io/assets/images/post/comment_continue.png) 

 - disqus 홈이 표시 됩니다.
 
#### 2. Disqus 설정하기
 - 회원 가입이 완료 되었으니 Disqus 설정을 추가합니다. 
 - [설정하기](https://disqus.com/profile/signup/intent/) 로 이동합니다. 
 - 위 링클르 클릭하면 아래와 같이 나옵니다.
  ![choose]({{ site.url }}assets/images/post/comment_sel.png)

 - 댓글을 자신의 사이트에 추가 하기위해서는 아래 버튼을 눌러주세요
   - i want to install Disqus on my site
 - site 이름과 카테고리 선택을 해주세요.
 ![name](img/comment_name.png)

 - 하단의 create 버튼은 누르고 렛츠 스타트를 선택 합니다.
 - 플랫폼은 jekyll 을 선택합니다.
 - 추가하는 방법이 나옵니다.

#### 3. github.io jekyll 과 연동하기
 - 중간에 Universal Code 를 선택하면 해당 코드가 표시 되고 이를 복사해서 
 - _layout/post.html 의 disqus 위치에 추가 시켰습니다.
 - 결과는 성공입니다.

