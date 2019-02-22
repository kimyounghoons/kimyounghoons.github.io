---
layout: post
title: Lifecycle
description: "AAC lifecycle"
modified: 2019-02-22
tags: [lifecycle,aac]
categories: [android,aac]
---

문제점 : 뎁스가 깊어질때 CustomVideoView가 가지고 있는 mediaplayer 메모리가 제대로 해제 되지 않는 문제

해결 : life cycle 사용해서 Stop 시 mediaplayer release 처리

VideoView 는 LifecycleObserver 인터페이스를 가지고 있다.
우선 뷰 생성시 라이프 사이클 옵저버 등록

{% highlight ruby %}
     public VideoView(Context context) {
        super(context);
        initialize(context);
    }

    private void initialize(Context context) {
        if (context instanceof AppCompatActivity) {
            ((AppCompatActivity) context).getLifecycle().addObserver(this);
        }
    }
{% endhighlight %}


@OnLifecycleEvent 종류에 따라 life cycle 콜백이 불리게 되는데 stop 되었을때 release 
destroy 될때 등록되어 있던 observer를 제거 시켜준다.

{% highlight ruby %}
    @OnLifecycleEvent(Lifecycle.Event.ON_STOP)
    public void onLifecycleStop() {
        stop();
    }

    @OnLifecycleEvent(Lifecycle.Event.ON_DESTROY)
    public void onLifecycleDestroy() {
        if (getContext() != null) {
            if (getContext() instanceof AppCompatActivity) {
                ((AppCompatActivity) getContext()).getLifecycle().removeObserver(this);
            }
        }
    }
{% endhighlight %}

사용법 참 쉽구먼 ㅎㅎ