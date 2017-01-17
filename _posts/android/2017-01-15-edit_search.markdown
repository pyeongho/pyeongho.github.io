---
layout: post
title: EditText 를 이용한 RecyclerView 검색 기능
category: andorid
comments: true
description: RecyclerView 에 검색 기능을 추가하여 한두 글자를 입력으로 원하는 결과만 확인 하는 방법
tags:
    - android
    - RecyclerView     
---



#### 1. EditText 를 이요한 리스트 검색 기능

  - RecyclerView에 원하는 내용만 출력하기
  - 기본 내용은 [http://jizard.tistory.com/53](http://jizard.tistory.com/53) 블로그를 참조 했습니다.
  - 리스트2개를 가지고  contain 에 포함 된것만 보여주는 형태 입니다.
  - 중요한 코드는 아래와 같습니다.
    <script src="https://gist.github.com/pyeongho/fbe79093772cfa1b62fc3f97659bdaed.js"></script>

  - EditText 의 afterTextChanged 에서 filter 함수를 호출해 주면 됩니다.  
  - 전체 코드 첨부 합니다.
      -[구글](/assets/zip/post/ListViewSearch.zip)

