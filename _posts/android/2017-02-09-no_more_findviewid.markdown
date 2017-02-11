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
 - 1. 레이아웃 파일 최상위 트리에 
  - <script src="https://gist.github.com/pyeongho/90ec3c115ae62ecc49f398f40b55e8d6.js"></script>
 
 - 2. setContentView(R.layout.activity_main); 가 아래 처럼 변경 됩니다.

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

#### 5. 리스트뷰 또는 리사이클러 뷰에서 사용 
 - ListView 어댑터나 RecyclerView 어댑터 내에서 데이터 바인딩 항목을 사용 중인 경우 다음을 선호하는 개발자도 있습니다.

```java 
  ListItemBinding binding = ListItemBinding.inflate(layoutInflater, viewGroup, false);
  //or
  ListItemBinding binding = DataBindingUtil.inflate(layoutInflater, R.layout.list_item, viewGroup, false);
```

#### 6. 위에서 사용을 레이아웃 바인딩이라고 하고 이제는 데이터 바이딩과 클릭이벤트 
 - 클래스를 사용하기 위해서 사용자 객체를 만든다.
 - <script src="https://gist.github.com/pyeongho/8133adb6428e763ae8953edc56b5a680.js"></script>
 - 레이아웃 파일에 아래 내용을 추가

```xml  
    <data>
       <variable name="user" type="com.example.User"/>
   </data>
   ... 
           <TextView
            android:id="@+id/hello"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@{user.firstName}" />
   ...         
``` 

 - 액티비티 자바 파일에

```java
   User user = new User("Test", "User");
   binding.setUser(user);
```

 - 텍스트뷰에 hello 에 Test 가 추가 된다.
 - 안드로이드 개발자 사이트에 보면 클릭이벤트 리스너등 사용하면 좋을것들이 너무 많다.
 - 심지어 import 룰 사용도 가능하다.
 - 바인딩 기능을 모두 사용하면 레이아웃 파일을 보기 힘들어 질거 같다...


#### 7. 항상 해줘야 하는 자바 코드를 추상화 클래스로 만들어 보면
 - baseactiviy 를 만듬
  - <script src="https://gist.github.com/pyeongho/ac55d87b879282327bbb60c2ab5874de.js"></script>
 - MainActivity 에서 사용하는 법

```java
    ...
    public class MainActivity extends BaseActivity<ActivityMainBinding> {
    ...    
    setBiding(R.layout.activity_main);
    getBinding().tvHello.setText("id:tv_hello");
    ...
```  

#### 8. fragment 에서 사용하기

```java
public class FancyFragment extends Fragment {
  private FragmentFancyBinding binding;

  @Override
  public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
    return inflater.inflate(R.layout.fragment_fancy, container, false);
  }

  @Override
  public void onActivityCreated(Bundle savedInstanceState) {
    super.onActivityCreated(savedInstanceState);
    binding = FragmentFancyBinding.bind(getView());
  }
}
```

#### 9. ViewHolder 에서 사용하기
 - <script src="https://gist.github.com/pyeongho/dc4aa079ab29e6d30d6092f147c6a4d7.js"></script>


#### *. 좋은점
  - 외부 라이브러리를 사용 안해도 된다.
  - findViewById 사용을 안해도 된다.
  - 심지어 변수 선언들도 안해도 된다.
