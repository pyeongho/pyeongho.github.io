---
layout: post
cover: 'assets/images/cover1.jpg'
title: 구글에서 만든 클린 아키텍처를 이해하고 알아보자
date: 2017-04-22
tags: 
  - android
  - 클린아키텍처
subclass: 'post tag-android'
categories: 'pyeongho'
navigation: True
logo: 'assets/images/ghost.png'    
---

### 구글 클린 아키텍처
  - mvp 패턴만 적용 하더라도 구조가 이상해 보인다. 어떻게 하면 좀더 좋은 구조를 만들 수 있을까를 고민하던 중 클린아키텍처가 있었고 그중 구글이 만들 샘플 코드가 기본 라이브러리로만 만들어져서 마음에 들어서 분석하기 시작함   

#### 1. 구글 샘플 분석 하기
  - [https://github.com/googlesamples/android-architecture/tree/todo-mvp-clean/](https://github.com/googlesamples/android-architecture/tree/todo-mvp-clean/)
  - 패키지 구분을 어떻게 했는지 직관적인 확인이 어렵다.(아직 적응 못함)
  - 한눈에 Uncle Bob clean architecture 레이어 확인 가능 할 줄 알았다.
  - 처음 보이는건 테스트 유닛만 보인다.
  - 그래도 구글이니 잘 만들었을거라 예상하고 다시 확인 시작
  - 참고로 구글 프로젝트라서 그런지 구아바를 사용 중
    - [https://github.com/google/guava](https://github.com/google/guava)
    - 구글의 자바 라이브러리라고 생각하면 좋음
    - [구아바를 사용해야 하는 5가지 이유](https://blog.outsider.ne.kr/710)
    - [구아바 맛보기](http://heowc.tistory.com/61)
  - todo 앱 으로 할일을 만들고 체크해서 할일 완료 할일을 확인하는 앱이다.
  - 가장 기본이 되는 엔티티 레이어를 찾아보자
  - 패키지 이름에 없어서 구조를 확인해서
  - 아래와 같은 구조로 설계되어 있다    
  - 패키지 이름에서 데이터 소스코드가 보임
  - com.example.android.architecture.blueprints.todoapp.data.source
  - 감사하게 아래처럼 패키지 이름이 정의 되어 있습니다.  
  - 설계와 동일하게 로컬과 리모트 패키지도 보이고
  - 엔티티 개념을 보이는 내용이 없다.
  - 클린아키텍처를 이해하기로 앱을 본질은 투두를 만들려고 했으니 기본적인 할일의 제목 할일의 설명 했다, 안했다. 에 대한 클래스가 있을거라 예상 했지만 해당 패키지에는 없다.
  - 위 데이터레이어 패키지를 확인해보니 Task 라는 클래스가 보인다.
  - 패키지는 com.example.android.architecture.blueprints.todoapp.tasks.domain.model;
  - 글을 좀더 자세히 읽어보니 MVP 모델의 중간에 도메인 레이어를 추가한 개념을 사용한걸로 보인다. 
    - [https://github.com/googlesamples/android-architecture/tree/todo-mvp/](https://github.com/googlesamples/android-architecture/tree/todo-mvp/) 설명에 나와 있는데 안읽고 이제 서야 읽어봄
    - 위 샘플을 기반을 클린 아키텍처를 구현함
    - Google todo mvp 를 살짝 보고 돌아옴
  - MVP 기반의 코드와 다른 점은 중간에 도메인 레이어를 추가 한 점입니다.
  - 데이터 <- 도메인 <- 프리젠터 레이어로 구성되어 있습니다.
  - TasksDataSource(데이터레이어) <- usecase(도메인레이어) <- 프리젠테이션레이어(사용자)
  - 처음에는 별로 마음에 안들었지만 보다 보면 볼수록 마음에 듬
  - 정리중

 




