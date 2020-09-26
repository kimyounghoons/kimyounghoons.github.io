---
layout: post
title:  Activity 와 Fragment 간의 ViewModel 을 통한 데이터 공유
description: Activity 와 Fragment 간의 ViewModel 을 통한 데이터 공유
modified: 2020-09-26
tags: [Android]
categories: [Android]
---

안드로이드 앱 개발을 하다 보면 Activity 와 Fragment 간의 데이터를 주고 받아야 할 때가 있다.
기존 방식은 어떤 방식을 사용했고 새로운 방식은 또 어떤 방법으로 하는지 알아보자.  

#### 기존 방식

1. #### Activity → Fragment  
데이터를 넘길 때 Fragment newInstance fuction 을 만들어 Bundle로 데이터를 넘김  
Fragment TAG 를 사용해서 Fragment 를 찾은 다음 Fragment 의 데이터 set  

2. #### Fragment ←→ Activity
Fragment 가 Activity 에 attach 되는 순간 context 를 통해서 Listener 구현해서 데이터 공유

3. #### Activity 내부 Fragments 간의 데이터 공유
One Fragment → Activity → Two Fragment  
이렇게 공유 했었다.

#### 새로운 방식
ViewModel 을 사용하면 데이터 공유가 더 쉬워 진다.

Activity  
```java
ViewModelProvider(this(액티비티)).get(액티비티 뷰모델명::class.java)
```

Fragment   
```java
private val activityViewModel by lazy {
        //Fragment에서 ViewModelProvider을 생성 할 때 Fragment 를 넣는게 아니라 Activity 가 들어 가야 한다.  
        ViewModelProvider(requireActivity()).get(액티비티 뷰모델명::class.java)
}
```
Fragment 에서 ActivityViewModel 을 사용하기 때문에 데이터 공유가 훨씬 편해진다.  
LiveData 를 사용하면 더 사용하기 좋다.  
LiveData Observe 할 때는  ViewModelProvider 을 생성 할 때와 달리 각각의 라이프사이클에 맞춰서 셋팅 해주면 된다.  

Activity → ActivityViewModel  
Fragment in Activity→ ActivityViewModel  
OtherFragment in Activity → ActivityViewModel  

위와 같이 ActivityViewModel을 공유하기 때문에 데이터 공유가 자유로워 진다.