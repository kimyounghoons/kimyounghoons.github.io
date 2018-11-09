---
layout: post
title: view setAlpha in OnTouchListener
description: "view setAlpha in OnTouchListener"
modified: 2018-11-09
tags: [android]
categories: [android]
---

view pressed 상태 되었을 때 알파 50% 먹이기

커스텀 뷰에서는 onTouch 메소드 오버라이드 해서 사용

{% highlight ruby %}
   @Override
    public boolean onTouch(View v, MotionEvent event) {

        switch (event.getAction()) {
            case MotionEvent.ACTION_DOWN:
                v.setAlpha((float) 0.5);
                break;
            case MotionEvent.ACTION_UP:
            case MotionEvent.ACTION_CANCEL:
                v.setAlpha((float) 1);
                break;
        }
        return false;
    }
{% endhighlight %}

ACTION_DOWN 상태에서는 Pressed 상태 
ACTION_UP 상태에서는 그 뷰에서 뗀 상태 
ACTION_CANCEL 프레스 상태에서 그 뷰를 벗어난 곳에서 뗀 상태
에서 각각 Action을 가진다.

프레스 되면 0.5 알파 값 주고 떼면 다시 원상태 복구