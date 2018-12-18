---
layout: post
title: SourceTree에서 Pull,Push,Fetch 할 때 계속 계정 요청 하는 경우
description: "SourceTree에서 Pull,Push,Fetch 할 때 계속 계정 요청 하는 경우"
modified: 2018-12-18
tags: [Git,SourceTree]
categories: [Git,SourceTree]
---
Window 사용할때는 이런일 없었는데 맥북 쓰면서 SourceTree 에서 계정 입력 요청이 계속 떠서 구글링 해서 해결 방법을 찾았다.
{% highlight ruby %}
git config credential.helper store    
{% endhighlight %}
입력해주면 해결완료!
그다음부터는 계정 요청을 하지않는다.