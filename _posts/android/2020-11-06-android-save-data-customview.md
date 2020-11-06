---
layout: post
title: Save State for CustomView
description: 커스텀뷰 상태 저장(Save State for CustomView)
modified: 2020-11-06
tags: [Android, Kotlin]
categories: [Android, Kotlin]
---

## 커스텀뷰 상태 저장
커스텀뷰가 그려지고 메모리 부족 등으로 인해 다시 그려지는 순간 데이터를 저장한후 다시 가져와야 화면에 그대로 보여줄수 있다.

개발자 모드에서 활동 유지 안함을 활성화 한 후 테스트해야 한다.

아래는 커스텀뷰 상태 저장을 하지 않은 상태이다.

<figure>
    <p align="center">
	    <img src="/images/2020-11-06-android-save-data-customview-01.gif" alt="" width="200" height="350"/>
	</p>
</figure>

이제부터 데이터 저장을 해보자~!

이때 필요한 메소드는 onSaveInstanceState , onRestoreInstanceState 두개이다.
```java
override fun onSaveInstanceState(): Parcelable? {
        return Bundle().apply {
            putParcelable(KEY_ON_SAVE_INSTANCES_TATE, super.onSaveInstanceState())
            putStringArrayList(KEY_SAVE_DATA, ArrayList(textViewArrayList.map {
                it.text.toString()
            }))
        }
}

override fun onRestoreInstanceState(state: Parcelable?) {
        if (state is Bundle) {
            super.onRestoreInstanceState(state.get(KEY_ON_SAVE_INSTANCES_TATE) as Parcelable)
            state.getStringArrayList(KEY_SAVE_DATA)?.let {
                if (it.isNotEmpty()) {
                    removeAllViews()
                    it.filter { originContent ->
                        originContent != "," && originContent != endText
                    }.forEach { filteredContent ->
                        addText(filteredContent, false)
                    }
                }
            }
        } else {
            super.onRestoreInstanceState(state)
        }
}
```

활동 유지 안함을 하게 되면 Activity 가 종료된 후 다시 생성되게 되는데 이때 뷰의 onSaveInstanceState 가 호출 된다.

위와같이 StringArrayList 를 onSaveInstanceState 에서 저장하고 다시 생성 될 때 onRestoreInstanceState 가 호출되고 onSaveInstanceState 에서 return 해준 Bundle 이 state 로 넘어온다.

이때 주의 해야하는 점은 super.onRestoreInstanceState()를 호출 해줄 때 super.onSaveInstanceState() 를 저장한 값 그대로 넣어줘야 한다.

아래는 커스텀뷰 상태 저장을 한 상태이다.

<figure>
    <p align="center">
	    <img src="/images/2020-11-06-android-save-data-customview-02.gif" alt="" width="200" height="350"/>
	</p>
</figure>

데이터 상태를 저장 한 후 다시 불러 왔기때문에 금액 입력이 보이지 않고 원래 적었던 금액이 보이게 된다.

완성도 높은 CustomView 를 만들게 되면 이부분은 필수적인 부분이라고 볼 수 있다.