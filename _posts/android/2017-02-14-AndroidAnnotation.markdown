---
layout: post
cover: 'assets/images/cover1.jpg'
title: AndroidAnnotation을 공부 하자
date: 2017-02-14
tags: 
    - android
    - AndroidAnnotation
subclass: 'post tag-android'
categories: 'pyeongho'
navigation: True
logo: 'assets/images/ghost.png'    
---




### 안드로이드에서 AndroidAnnotation을 이용해서 앱을 개발해 보자
 - 어노테이션 기능으로 좀금더 편하게 의존성을 주입해서 앱을 개발하는 방법 입니다.
 - [http://androidannotations.org/ 공식사이트](http://androidannotations.org/)

#### 1. 환경 설정
 - [https://github.com/androidannotations/androidannotations/wiki/Building-Project-Gradle](https://github.com/androidannotations/androidannotations/wiki/Building-Project-Gradle)
 - 그래들 환경이라서 편하게 추가 할 수 있습니다.
 - 프로젝트 레벨 그래들
  
  - <script src="https://gist.github.com/pyeongho/47dbeb046afeee449fbe52f00cc43efc.js"></script>

 - app 레벨 그래들 변경내용 

```gradle
apply plugin: 'com.android.application'
apply plugin: 'android-apt'

def AAVersion = '4.1.0'

...
dependencies {
 ...
    apt "org.androidannotations:androidannotations:$AAVersion"
    compile "org.androidannotations:androidannotations-api:$AAVersion"
 ...   
}

```

#### 2. databinding 과 함께 사용하기
 - databinding 의 BaseActivity 를 사용한다.
 - @EActivity 에 레이아웃 파일을 추가하지 않는다.
 - 기본 Activity 의 onCreate 에서 레이아웃을 처리 한다.


#### 3. 네이버 기계번역을 안드로이드 어노테이션으로 샘플코들르 만들어 보았습니다.
 -  [https://github.com/pyeongho/Sample](https://github.com/pyeongho/Sample)
 - AndoroidAnnotation 폴더에 있습니다.