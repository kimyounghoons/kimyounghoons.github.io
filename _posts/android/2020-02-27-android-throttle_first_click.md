---
layout: post
title: 안드로이드 다중 클릭 방지(Prevent Multiple Click)
description: "안드로이드 다중 클릭 방지(Prevent Multiple Click)"
modified: 2020-02-27
tags: [android]
categories: [android]
---

화면 이동이나 특정 API 호출을 할 때 안드로이드 자체에서 다중 클릭을 막아 주지 않기 때문에 따로 처리가 필요하다.  
그래서 회사 팀원들과 같이 의견을 조금씩 반영하다 보니 좋은 결과물이 나온것 같다. ㅎㅎ  
소스는 아래와 같다.  

```
var isClicked = false

//데이터 바인딩에서 사용하기 위한 fun
@BindingAdapter("app:onThrottleFirstClick", "app:onThrottleInterval", requireAll = false)
fun onThrottleFirstClick(
    view: View,
    onClickListener: View.OnClickListener,
    isWithoutInterval: Boolean = false
) {
    view.setOnClickListener { v ->
        if (isClicked.not()) {
            isClicked = true
            v?.run {
                if (isWithoutInterval) {
                    isClicked = false
                    onClickListener.onClick(v)
                } else {
                    postDelayed({
                        isClicked = false
                    }, 350L)
                    onClickListener.onClick(v)
                }
            }
        }
    }
}

//일반 뷰에서 사용할 수 있는 Kotlin Extension
fun View.onThrottleFirstClick(interval: Long = 350L, action: (v: View) -> Unit) {
    setOnClickListener { v ->
        if (isClicked.not()) {
            isClicked = true
            v?.run {
                postDelayed({
                    isClicked = false
                }, interval)
                action(v)
            }
        }
    }
}

```

### xml Layout 코드(Before)
기존 onClick 구현 방식

```
<Button
    android:id="@+id/button"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:onClick="@{(v)-> 어떤 처리}"
    android:text="일반 클릭"
/>
```

위와 같이 onClick을 사용하면 다중 클릭을 막을 수 없고 버튼을 여러번 클릭 하는 경우 어떤 처리가 여러번 처리 된다.  
아래 다중클릭이 방지된 BindingAdapter 를 사용하게 되면 여러번 클릭을 막을 수 있다.  
onThrottleInterval 은 싱글 클릭이 필요 없는 경우에만 넣어주면 된다.  
예를 들어 옵션의 카운트 갯수를 올려주는 행위에 사용하면 된다.  

### xml Layout 코드(After)
변경된 방식
```
<Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="싱글 클릭"
        app:onThrottleInterval="@{불린값}"
        app:onThrottleFirstClick="@{(v) -> 어떤 처리}"
        />
```

### button을 가지고 있는 클래스(Before)
```
    button.setOnClickListener {
        //어떤 처리
    }
```

### button을 가지고 있는 클래스(After)
```
    button.onThrottleFirstClick {
        //어떤 처리
    }
```

사용법도 크게 차이가 없어서 편하게 사용할 수 있게 만들어진 것 같다.