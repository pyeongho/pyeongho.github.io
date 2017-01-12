---
layout: post
title: 안드로이드 스튜디오 한글 깨짐
category: andorid
comments: true
description: 안드로이드 스튜디오 최신버전에서 레이아웃 미리보기에서 한글깨짐 현상 수정
tags:
    - android-studio    
    - 한글깨짐    
---



#### 1. 안드로이드 스튜디오 2.X 버전에서 한글깨짐 수정
 - 안드로이드 스튜디오가 설치된 경로 내의 다음 파일을 확인합니다. (OS X의 경우 안드로이드 스튜디오 실행파일을 오른쪽 선택한 후 ‘Show Package contents’로 하위 폴더 탐색 가능)
 

 ```
    // Windows
    {Android Studio 설치경로}/plugins/android/lib/layoutlib/data/fonts/fonts.xml

    // OS X
    {Android Studio 설치경로}/Contents/plugins/android/lib/layoutlib/data/fonts/fonts.xml
```
 
  - 해당 xml 파일을 열어서 아래 내용을 추가 합니다.
  - ( + ) 부분을 추가 합니다.


  ```
      <family lang="ko">
         <font weight="400" style="normal" index="1">NotoSansCJK-Regular.ttc</font>
     </family>
+    <family lang="ko">
+        <font weight="400" style="normal" index="1">NanumGothic.ttf</font>
+    </family>
     <family lang="und-Zsye">
         <font weight="400" style="normal">NotoColorEmoji.ttf</font>
     </family>
  ```