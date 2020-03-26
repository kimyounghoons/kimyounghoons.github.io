---
layout: post
title: ActionBar Menu in Fragment 
description: "ActionBar Menu in Fragment"
modified: 2018-11-06
tags: [android]
categories: [android]
---

메뉴 아이템을 res/menu 아래에 만들어 주면 된다. menu는 기본적으로 없어서 만들어 줘야함.

```
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto">
    <item
        android:id="@+id/menu_answer_letter"
        android:title="@string/answer"
        app:showAsAction="always"/>
</menu>
```

Fragment 에서 우선 Override 할 메소드는 onCreateOptionMenu , opOptionsItemSelected 두가지 !!

Activity 에서는 onCreateOptionsMenu 메뉴가 불리지만 Fragment 에서는 setHasOptionsMenu(true) 셋팅을 해줘야 불리게 된다.

위치는 OnCreateView 안에 넣어주면 됨.

그럼 정상적으로 onCreateOptionsMenu 메뉴가 불리게 된다.

```
 override fun onCreateOptionsMenu(menu: Menu?, inflater: MenuInflater) {
        inflater.inflate(R.menu.menu_detail_letter, menu)
        super.onCreateOptionsMenu(menu, inflater)
    }
```

다음은 메뉴 아이템이 선택 되면 불리는 메소드

```
override fun onOptionsItemSelected(item: MenuItem): Boolean = when (item.itemId){
       R.id.menu_answer_letter ->{
           locateListener?.openAnswer(letter)
           true
       }
        else ->{
            false
        }
}
```

확실히 자바 쓰다가 코틀린으로 쓰니까 간결하긴 하네 ㅎㅎ 