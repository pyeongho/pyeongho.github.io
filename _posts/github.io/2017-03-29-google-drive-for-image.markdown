---
layout: post
cover: 'assets/images/cover4.jpg'
title: 구글 드라이브를 이용해서 쉽게 이미지를 올리자
date: 2017-03-29
tags: 
    - githubpage
    - googledrive
subclass: 'post tag-githubpage'
categories: 'pyeongho'
navigation: True
logo: 'assets/images/ghost.png'    
---



#### 맥에서 github page 이미지 쉽게 올리기
  - 깃허브 페이지를 이용중 가장 힘든건 스크린샷을 추가하는 방법이라 생각해서 찾아보니 구글드라이브를 이미지서버로 사용이 가능해서 적용해 보았습니다. 문제는 사파리에서 정상동작 안한다는 단점

[참고 : https://beomi.github.io/2017/03/27/Use-GoogleDrive-as-Image-Server/](https://beomi.github.io/2017/03/27/Use-GoogleDrive-as-Image-Server/)
#### 1. 구글 드라이브 설치
  - homebrew 를 이용해서 쉽게 설치
  - $brew install gdrive
  - 설치 완료후
  - $gdrive list 
  - 입력하면 인증해야 한다고 나오면서 아래처럼 링크가 나옵니다.
    ```
    Authentication needed
    Go to the following url in your browser:
    https://accounts.google.com/o/oauth2/auth?xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx........
    Enter verification code:
    ```
  - 권한 요청 페이지가 나오고 권한을 허용하면  인증 코드가 나온는데 복사 해서 입력란에 넣어 줍니다.
  - 다시 한번
  - $gdrive list
  - 자신의 구글드라이브의 루트의 리스트가 나옵니다.

#### 2. 캡쳐 쉘 스크립트 만들기
  - <script src="https://gist.github.com/anonymous/1582cd1b8c6afd2fa682d71779e03aa9.js"></script>
  - 쉘 파일을 적당한 위치에 만든다. 
  - 위 파일로 캡쳐하면 구글드라이브 루트에 생성되는 불편 함이 있습니다.
  - p 옵션을 사용하면 원하는 폴더에 넣을수 있다고 하는데 폴더 이름을 적어도 잘 안됨
  - 문제는 옵션 다음에 적어할 내용은 폴더 위치의 ID 를 적어야 합니다.
  - 폴더 이름이 아닌 폴더 ID 입니다.
  - 폴더 ID는 
  - $gdrive list  
  - 명령어를 입력하면 확인 할 수 있습니다.

#### 3. 여기까지가 gdrive 공부정도 입니다
  - 아래 앱을 만들때는 스크립트를 복사 해서 사용하기웨 위 내용은 사용 안하더라구요 

#### 4. 맥용 앱을 만들어서 단축키로 실행해야죠 
  - Platypus 요런 프로그램이 쉘 스크립트를 앱으로 만들어 줍니다. 
  - [다운로드 링크 : Platypus](http://sveinbjorn.org/files/software/platypus.zip)
  - 다운 받아서 실행하면 보안 어쩌고 저쩌고 하면서 실행이 안되면 설정 -> 보안 항목에서 실행 할 수 있습니다.
  - 아래 이미지중 new 를 눌러서 
  - 스크립트를 붙혀넣기 하였습니다.
  - <script src="https://gist.github.com/pyeongho/1a1ba765dcd809de71d4a7bd029d9dc6.js"></script>
  - 코드는 크게 다르지 않지만 gdrive 가 절대 경로로 변경 된게 다릅니다.
  - 아래 처럼 인터페이스 및 필요없는 옵션 체크 해제 후 앱을 만들었습니다.
  - 앱 프로그램을 애플리케이션으로 이동하여 알프레드로 호출하니 잘 동작 합니다.
  - 어차피 단축키 추가해봐야 외우기 힘들테니 그냥 알프레드를 이용하려고 합니다. 
  - ![테스트](http://drive.google.com/uc?export=view&id=0BwUadct9RzY3MlJrOERUMG15ejA)
