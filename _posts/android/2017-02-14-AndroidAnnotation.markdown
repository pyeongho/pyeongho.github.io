---
layout: post
title: AndroidAnnotation을 공부 하자
category: andorid
comments: true
description: 안드로이드 
tags:
    - AndroidAnnotation
---



### 안드로이드에서 AndroidAnnotation을 이용해서 앱을 개발해 보자
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
