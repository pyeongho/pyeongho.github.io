---
layout: post
cover: 'assets/images/cover1.jpg'
title: 제네릭을 이용하여 샘플코드를 좀더 편하게 사용하기
date: 2017-02-19
tags: 
  - android
  - 제네릭
subclass: 'post tag-android'
categories: 'pyeongho'
navigation: True
logo: 'assets/images/ghost.png'    
---



### 최종코드의 BasePresnter의 addSubscription를 부분만 확인하면 됩니다.
  - API가 추가 될때마다 계속 추가해야하나?

#### 1. api 추가되면 어떻게 해야 하나
 - 기존 코드에서 기계번역 1개의 api만 사용 하고 있습니다.
 - 만약 api가 계속 추가 되면 응답값이 계속 바뀌면 api 콜백을 계속 추가 해야 하나?
 - addSubscription를 계속 만들어야 하나 고민을 시작함 
 - 그래서 간단하게 제네릭 타입의 클래서를 만들고 api콜백과 addSubscription는 하나만 사용하는 방법
 - 호출 할때만 제네릭에(템플릿이라고 해야 하나!@) 메소드를 호출할때, 콜백을 받을때 원하는 클래스 타입으로 받고 싶었습니다.
 - 컴파일 에러 안나게 해서 코드를 추가 했지만,,, 값이 들어 오지 않았습니다.
 - git 히스토리에 남아 있습니다. 
 - 그래서 다시 시작 하게 되었습니다.
 - 코드커밋을 잘못해서 기본 객체로 받는것도 올라가 있습니다.(withoutbutter) 

#### 2. 어떻게 접근할것인가?
 - 현재 문제점 : addSubscription 에 타입이 정해져 있어서 추가하기 귀찮게 되어 있다.
 - API콜백은 원하는 클래스로 사용가능하고 api 인터페이스 역시 샘플처럼 있고 계속 추가 하면 될거라고 생각
 
#### 3. 공부링크
 - [https://github.com/amitshekhariitbhu/RxJava2-Android-Samples](https://github.com/amitshekhariitbhu/RxJava2-Android-Samples)
 - [https://github.com/delicious-mvp/delicious](https://github.com/delicious-mvp/delicious)
  - rxjava1 이지만 공부할게 많다.

#### 4. 처음 부터 잘못 접근
 - 제넥릭 사용법을 잘 몰라서 이상한짓을 한거임
 - addSubscription 에서 제네릭으로 받으면 됩니다. 
 - RxJava2 map 과 필터 공부로 깔끔하게 마무리 

#### 5. 최종 결과를 다운 받기에 폴더 하나 더 만들었습니다.
 -  [https://github.com/pyeongho/Sample](https://github.com/pyeongho/Sample)
 - 폴더이름 Retorfit2T