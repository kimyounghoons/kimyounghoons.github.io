---
layout: post
title: singleTask Activity 사용 시 이슈
description: "singleTask Activity 사용 시 이슈"
modified: 2019-03-18
tags: [SingleTask,Flag]
categories: [android,singleTask,flag]
---

문제점 : A(Standard) Activity -> B(Single Task) Activity ->A(Standard)Activity Intent.FLAG_ACTIVITY_NEW_TASK | Intent.FLAG_ACTIVITY_CLEAR_TASK  FLAG로 startActivity 할 때 앱 강제 종료되는 문제 

해결 : 
SingleTask 는 하나의 Activity만 생성되고 다중 인스턴스를 가질 수 없다. 그리고 이미 존재하는 경우 시스템은 새 인스턴스 대신 onNewIntent() 메서드를 호출하여 인텐트를 기존 인스턴스로 라우팅한다.

플로우가 수정 되면서 해당 이슈가 발생 되었는데 해결 방법은 생각 해본 건 대충 3가지로 정리

첫번째는 액티비티 외에 다른 곳에서 호출 될 일이 있을까 하는 부분. Intent.FLAG_ACTIVITY_NEW_TASK 을 제거 하고 사용해도 무관한지 찾아 보는 것이다. 회원가입을 하거나 했을 때 뒤로가기 시 그대로 Task에 남게 되는 이슈가 있다. 이런 경우가 없을 시에는 FLAG_ACTIVITY_NEW_TASK를 제거하면 해결 가능하다.

두번째는 singleTask 액티비티에서만 Intent.FLAG_ACTIVITY_CLEAR_TASK 사용하고 NEW_TASK 는 사용하지 않는다.

{% highlight ruby %}
    if (context instanceof Bactivity) {
            intent.setFlags(Intent.FLAG_ACTIVITY_CLEAR_TASK);
        } else {
            intent.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK | Intent.FLAG_ACTIVITY_CLEAR_TASK);
        }
{% endhighlight %}

세번째는 굳이 singleTask로 하지 않아도 된다면 Bactivity를 singleInstance로 변경 시키는 것도 고려할 수 있겠다.

현재 처리 된 부분에서 적합한 건 두번째 방법인것 같아 두번째 방법으로 적용 시켰다.