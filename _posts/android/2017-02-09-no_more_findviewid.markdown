---
layout: post
title: 버터 나이프 없이 데이터 바인딩 사용하기
category: andorid
comments: true
description: findViewById 없이 사용하기
tags:
    - no more findViewById
---



### 참고 사이트 
 - [안드로이드 개발자 사이트 링크](https://developer.android.com/topic/libraries/data-binding/index.html#studio_support)
 - [GsBob 블로그](http://gogorchg.tistory.com/entry/Android-DataBinding-findViewById-이제-안녕)


#### 1. 버터 나이프 사용하면 좋긴 하지만 라이브러리 추가가 싫어 질때가 있어서 찾아보고 적용해 보았습니다.
 - gradle 추가 내용
```grldle
    android {
        …
        dataBinding {
            enabled = true
        }
    }
```
 
#### 2. 기존과 달라지는 방법
 
 ##### 1. 레이아웃 파일 최상위 트리에 

  - <script src="https://gist.github.com/pyeongho/90ec3c115ae62ecc49f398f40b55e8d6.js"></script>


 ##### 2. setContentView(R.layout.activity_main); 가 아래 처럼 변경 됩니다.

```java
     ActivityMainBinding binding = DataBindingUtil.setContentView(this, R.layout.activity_main);
```

#### 3. 사용방법

```java
    ActivityMainBinding binding = DataBindingUtil.setContentView(this, R.layout.activity_main);
    binding.hello.setText("id:hello ");
    binding.tvHello.setText("id:tv_hello");
``` 

#### 4. 그래들에 추가 하고 싱크 리빌드 한번 해주세요.
 - ActivityMainBinding 자동으로 생성 됩니다. 
 - 처음에는 이걸 못찾아서 당황 했지만 그래들 싱크 해지고 리빌드 해주면 자동 생성 됩니다.
 - 생성 규칙은 아래 처럼 입니다. _ 자동으로 사라지고 카멜코딩룰을 적용해 줍니다.
   - 중요한 점은  activity_main - > ActivityMainBinding 형태로 변형 됩니다.
   - 변수 이름도 해당 규칙이 적용 됩니다. 
   - tv_hello -> tvHello 로 변경 되면서 자동으로 카멜코딩도 적용줍니다.

#### 5. 좋은점
  - 외부 라이브러리를 사용 안해도 된다.
  - findViewById 사용을 안해도 된다.
  - 심지어 변수 선언들도 안해도 된다.
