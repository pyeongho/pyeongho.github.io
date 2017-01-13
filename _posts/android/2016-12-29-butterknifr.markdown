---
layout: post
title: butterknife
category: andorid
comments: true
description: butterknife 라이브러리 사용
tags:
    - butterknife    
---



### butterknife 를 이용
[butterknife github](https://github.com/JakeWharton/butterknife)

#### 1. 안드로이드 xml 레이아웃 사용시 아래 와 같은 작업을 해야함 많아 지면 매번 해야함

```java 
    ImageButton btn_send; 
    btn_send = (ImageButton) findViewById(R.id.btn_send);
    ib_chat_photo.setOnClickListener(this);
```

#### 2. butterknife 위 작업을 편하게 해주는 라이브러리 입니다.
 - 안드로이드 스튜디오 에서 butterknife 라이브러리를 추가해준다
 - 아래 내용을 모듈의 적당한 위치 시켜야 합니다.

 ```gradle 
    apply plugin: 'com.jakewharton.butterknife'
     dependencies {
        compile 'com.jakewharton:butterknife:8.4.0'
        annotationProcessor 'com.jakewharton:butterknife-compiler:8.4.0'
    }
```
 - 프로젝트 레벨의 gradle 에 아래 내용을 추가해 준다.

 ```gradle     
    buildscript {
        repositories {
            mavenCentral()
        }
        dependencies {
            classpath 'com.jakewharton:butterknife-gradle-plugin:8.4.0'
        }
    }
 ```

#### 3. 안드로이드 스튜디오 플러그인을 설치
 - setting -> plugins -> browse repositories   에서 butterknife zelezny 찾아서 설치 (안드로이드 스튜디오 재시작함)
 - 레이아웃 xml 파일에서 마우스 오른쪽 버튼 클릭후 generate 를 선택 합니다.
 - butterknife 가 보입니다. 선택합니다.
 - 체크박스 선택 창이 나옵니다.  원하는 속성을 체크 체크 해서 완료 하면 됩니다.


