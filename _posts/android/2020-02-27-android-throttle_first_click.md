---
layout: post
title: 안드로이드 다중 클릭 방지(Prevent Multiple Click)
description: "안드로이드 다중 클릭 방지(Prevent Multiple Click)"
modified: 2020-02-27
tags: [android]
categories: [android]
---

화면 이동이나 특정 API 호출을 할 때 안드로이드 자체에서 다중 클릭을 막아 주지 않기 때문에 따로 처리가 필요하다.
그래서 회사 팀원들의 의견을 조금씩 반영하다 보니 좋은 결과물이 나온것 같다. ㅎㅎ
소스는 아래와 같다.

```
var isClicked = false

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


```
     textView.onThrottleFirstClick {
            //어떤 처리
        }
```


```
app:onThrottleInterval="@{불린값}"
            app:onThrottleFirstClick="@{(v) -> 처리할 것들}"
```