---
layout: post
title: python 모듈 만들기
category: python
comments: true
description: python C 모듈 원형
tags:
    - c
    - python   
---

### python 사용 할 C 라이브러리 만들기

#### 1. mylib.c 원형 함수를 만든다.  
  - python 에서 호출 하는 함수는 wlog 입니다. 
  - PyMethodDef를 보면 jni 와 비슷합니다.  

#### 2. setup.py 
  - 컴파일 과 install 하는 python 코드 원형
  - python setup.py install (sudo 가 필요 할 경우가 많습니다.)

#### 3. test.py
  - python test.py 를 실행 하면 pylog.txt 파일을 만들고 I love U 문구넣습니다.
   
#### 4. 참고 코드
  - <script src="https://gist.github.com/pyeongho/09b03c195395e2f8879c5f97686735a5.js"></script>
  - <script src="https://gist.github.com/pyeongho/abcad87180768a30091457630d38dc40.js"></script>
  - <script src="https://gist.github.com/pyeongho/7053f0fb7f15931a82ab927d11a5e206.js"></script>