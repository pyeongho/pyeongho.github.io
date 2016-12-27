---
layout: post
title: APK 바이너리 분석 및 리패키징
category: security
comments: true
description: APK 바이너리 분석 및 리패키징
tags:
    - OWASP 
    - andorid
    - 보안점검
---



### 4. APK 바이너리 분석 및 리패키징
  
#### 1.첨부된 apk 의 코드를 확인 한다. 
  - 이용하는 툴은 jd-gui-windows-1.4.0.zip , dex2jar-2.0.zip  
  - iamaboy.apk
  - d2j-dex2jar.bat iamaboy.apk
  - iamaboy-dex2jar.jar  파일 생성을 확인한다.
  - 다른 툴인 jd-gui.exe 를 실행하여 jd-gui.exe 파일을 연다.
  - 코드를 확인 하면 됩니다.

#### 2.루팅 체크 앱을 무력화 하여 동작 하도록 한다.
  - 기본 안드로이드 앱에서 루팅 체크 하는 코드를 추가 한다. 
  ```java 
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        if (rootCheck()){
            Toast.makeText(this, "root", Toast.LENGTH_SHORT).show();
            finish();
        }
    }

    public boolean rootCheck(){
        String[] arrayOfString = new String[5];
        arrayOfString[0] = "/sbin/su";
        arrayOfString[1] = "/system/su";
        arrayOfString[2] = "/system/sbin/su";
        arrayOfString[3] = "/system/xbin/su";
        arrayOfString[4] = "/data/data/com.noshufou.android.su";
        int i = 0;
        while (true)
        {
            if (i >= arrayOfString.length)
                return false;
            if (new File(arrayOfString[i]).exists())
                return true;
            i += 1;
        }
    }
  ```
  - apktool_2.2.1.jar 을 이용하여 루팅 테스트 앱을 smali 코드로 변경 한다.
  - java -jar apktool_2.2.1.jar d root.apk 
  - MainActivity.smali  파일에서 루팅 검사를 무력화 시킨다
  - 변경 방법은 문자열을 아래처럼 변경 해도 되고 
      const-string v5, "/system/sbin/su1"
  - 조건문을 변경해도 됩니다. 아래는 원래 코드 입니다.
  - if-eqz v0 v0 ==0 이면 cond_0 으로 간다는 내용 정도 
    ```
      invoke-virtual {p0}, Lkr/test/hello/MainActivity;->rootCheck()Z
      move-result v0
      if-eqz v0, :cond_0
    ```
  - if-eqz v0, :cond_0  =>  if-nez v0, :cond_0 
  - 위 처럼 조건문을 변경해도 됩니다.
  - smali 코드를 잘분석해서 변경 해야 합니다.
  - java -jar apktool_2.2.1.jar b root  으로 다시 컴파일 (폴더 이름임)
  - dist 폴더에 apk 가 생성됨
  - apk 를 서명하여 앱을 실행하면 앱이 

#### 3.  로그 추가 하기 
 -  함수위에 .local 숫자 이렇게 있는데 변수 개수 입니다.
 - 만약 기존 함수에 .local 3 이 있으면 .local 5 로 변경 하고
 - 아래와 같은 코드를 추가 하면 로그를 추력 할 수 있습니다.
    ```
    const-string v3, "[Injected Message]"		
    const-string v4, "Called onCreate() Method"	
    invoke-static {v3, v4}, Landroid/util/Log;->e(Ljava/lang/String;Ljava/lang/String;)I 
    ```
- Log 클래스의 함수는 는 인자를 2개 받기 때문에 변수 v3,v4 에 각각 원하는 문자열을 넣고 함수를 호출 하는 내용입니다.
- 참고로 Log 클래스의 각 함수는 int 형을 반화하기에 위처럼 마지막 문자열에 I  가 있습니다.