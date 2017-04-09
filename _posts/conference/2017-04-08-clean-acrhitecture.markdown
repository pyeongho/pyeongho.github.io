---
layout: post
title:  드로이드나이츠 클린아키텍처 세션
category: conference
comments: true
description: 드로이드 나이츠 컨퍼런스중 가장 기억에 남는 세션 입니다.
tags:
    - 클린아키텍처
    - 드로이드나이츠
---


#### 드로이드나이츠 클린아키텍처
 - [참고 : https://speakerdeck.com/sunghyunzz/clean-architecture-in-android](https://speakerdeck.com/sunghyunzz/clean-architecture-in-android/)

 - [참고 : https://realm.io/kr/news/clean-architecture-in-android/](https://realm.io/kr/news/clean-architecture-in-android//)

 - [참고 : https://8thlight.com/blog/uncle-bob/2012/08/13/the-clean-architecture.html](https://8thlight.com/blog/uncle-bob/2012/08/13/the-clean-architecture.html)

#### 1. 변화에 잘 적응하는 아키텍처
  - 이전 까지 했던 고민이 이걸로 보입니다. 어떤 변화든 잘 적응하는 코드를 만들자
  - 적응을 잘하는건 변하는 부분만 수정하자 <- 이 부분이 중요
  - 변화에 잘 대응할 수 있다.
   - = 변화에 따른 코드의 변경이 적다. 
     - 테이블 구조를 변경했는데 액티비티를 변경???
   - = 코드가 잘 분리되어있다.
     - 잘 분리되어야지 그냥 분리하면 안됨
   - = 코드가 본질에 맞게 설계되어있다.
     - 어떻게 레이아웃을 구성할까에 사용할까를 이용하면 안됨
     - 본질 
       - 도메인의 본질 : 기획자, 디자이너가, 사용자가 받아들이는 형태 일반적으로 보여지는 형태가 바뀌는거지 기본 개념이 변경되지 않음. 가계부를 예로 들어서 소비내역, 지출내역이라는 개념은 변경되지 않는다. 변경되는 어떻게 보여줄지 어떻게 필터링 할지가 변경 되는 거다. 도메인의 본질의 개념을 이해햐자.
       - 개발구조상의 본질 : 쉐어드프리퍼런스, 렘, sqlite 가 있는데 각가의 사용을 언제든 변경 없이 사용이 가능해야한다!?. 쉐어드 프리퍼런스를 수정 했는데 액티비티 수정을 해야 하면 코드가 잘못 된거다.
       - 안드로이드 구조 상의 본질 : 안드로이드 라이프사이클등

#### 2. 기본 아키텍처
  - 엉클밥의 클린아키텍처 서버
    - ![](https://drive.google.com/uc?id=0BwUadct9RzY3cmtQUjlkYlN5QTg)
    
    - 바깥쪽이 보여주는 부분
    - 가장안쪽이 개념단위
      - 프레임워크 독립적
        - 레트로핏 -> 볼리 로 변경이 쉬움
      - 테스트 가능
      - ui 독립적
      - db 독립적
        - 서버에서 사용하던지, 내부에서 사용하던지 중요치 않음

  - ![기본](https://drive.google.com/uc?id=0BwUadct9RzY3YU9CMXN5S2FTRXc)
    - 안드로이드의 계층을 4개로 사용자에게 보여지는 로직과 관련된 Presentation 레이어
    - DB, 네트워크를 포함한 데이터를 가져오는 Data 레이어 
    - 사용자의 유스케이스로 분리되는 Domain 레이어
      - get트랜잭션
    - 사용자의 개념을 정의하는 Entity 레이어 
    - 이 네 레이어 간의 의존성은 위에서 아래로 발생해야 합니다. 즉, 가장 하단부의 레이어일 수록 가장 의존성이 낮아야 합니다. 가량 프리젠테이션 레이어는 데이터 레이어를 알지만 데이터는 프리젠테이션을 몰라야 하며, 이 덕분에 맨 아래의 엔티티는 순수한 Java 내지는 Kotlin 모듈이 될 수 있습니다. 이런 레이어의 분리 덕분에 본질을 정의할 때 어떤 데이터베이스에 저장될지, 어떤 뷰에서 보일지 고민하지 않고 Entity를 작성할 수 있고, 이에 대한 유스 케이스로 Domain 레이어를 작성할 수 있습니다. 또한 트랜잭션을 가져오는 것을 Data에서, 어떻게 보여줄 것인지를 Presentation에서 고민하면 됩니다.

##### 2.1 Entity
  - 순수한 Java(Kotlin) 모듈
  - Android와의 의존성이 없음
    - 엔터티는 안드로이드의 파서러블을 사용할 수 없음
  - 파서러블의 고민은 뷰 모델   
  - 도메인(비즈니스 로직)에서 파생되는 개념을 표현
  - (같은 서비스) Android - iOS - 서버 모두 동일한 형태
  - 아래가 샘플로 보여준 코틀린 코드 입니다. 자바는 코틀린은 모르지만 느낌상으로 같다고 만들어본 코드 입니다.
  - 가계부에서 소비내역 지출 내역의 변경되지 않는다.
  - 모든 내용이 엔터티에 포함되어 있다.
  - TransactionType 엔터티로 선언한것이다.
  - 테이블간의 조인을 위해서 트랜잭션 아이디 등은 데이터베이스를 위함이기에 엔터티에서는넣으면 안됨
  - ``` kotlin
      class TransactionCategory( 
        val id: String, 
        val name: String, 
        val transactionType: TransactionType, 
        val priority: Int
      ) : Entity
    ```
  - ```java
      class TransactionCategory extends Entity( 
        String id; 
        String name;
        TransactionType transactionType;
        Int priority;
      ) : 
    ```  

##### 2.2 Domain
  - 순수한 Java(Kotlin) 모듈 
  - Android와의 의존성이 없음
  - Use Case
  - Repository Interface 정의    
  - 예
    - 뱅크레포지토리를 주입을 받고 
    - 인터페이스정의만 함
    - 하는 액션은 getAll
    - BanksRepository 는 인터페이스로 존재
    - 도메인 레이어에 존재 
    - 유스케이스는 데이터베이스를 고려하지 말자
    - 사고의 흐름
    - ```kotlin
      class GetBanks(val repository: BanksRepository) : UseCase<List<Bank>>(){ 
        override fun buildUseCaseObservable(): Observable<List<Bank>> { 
          return repository.getAll() 
        } 
      }

      interface BanksRepository : Repository {  
        fun getAll(): Observable<List<Bank>> 
        fun update(bank: Bank): Completable 
      }

      ```

##### 2.3 data
  - Repository의 실제 구현 
  - Data Source 의존성 
  - Android 의존성(어느순간 생기기 시작 가능성)
  - 데이터베이스 의존성이 생김
  - 도메인에서 정의한 인터페이스 구현이다. 렘 오브젝트를 엔터티로 변환 한다.
  - 렘이 sqlite로 변경되면 해당 레이어에서 처리 한다.
  - 똑같은 도메인의 유지하여 인테페이스를 네트워크로 변경가능하다.
  - 데이터모델은 렘이 아닌 응답값의 리스폰스로 변경하면 된다.
  - 예
    - <script src="https://gist.github.com/pyeongho/a621449874cd17a0c12041baf56fa835.js"></script>

##### 2.4 Presentation
  - UI 레벨의 처리 (mvp, mvc)
  - Android 의존성이 강함
  - View와 Presenter
  - 함수 이름도 onViewCreated 처럼 각각의 행동에 맞게
    - onNextButtonClicked 
  - 내부 로직이 변경되어도 뷰 코드는 변겨이 안됨
  - 파서러블이 필요할때 프리젠터에서도 뷰 모델이 있고 엔터티를 매핑하여 사용한다.  
  - 예
    - <script src="https://gist.github.com/pyeongho/618f625aa3f4a212f97197981ed5ea41.js"></script>
    - <script src="https://gist.github.com/pyeongho/6b325023418e98ab2d0a496da91d4087.js"></script>

#### 3. 좋은 코드가 좋은 제품이다.
  - 각각의 레이어에서 모델이 존재해서 진입 장벽이 있지만 유지보수가 쉽다.  

#### * 현장 메모
  - ![1페이지](https://drive.google.com/uc?id=0BwUadct9RzY3cG41UmxfbEN4NjQ)
  - ![2페이지](https://drive.google.com/uc?id=0BwUadct9RzY3NTNGRVpKLXNSOTg)
  