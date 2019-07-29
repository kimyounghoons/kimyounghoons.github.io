---
layout: post
title: resConfig(Build.Gradle)
description: "resConfig(Build.Gradle)"
modified: 2019-05-12
tags: [resConfig]
categories: [android]
---

### build.gradle 에서 resConfig

빌드 시간을 빠르게 하기 위해 Build.Gradle 에서 resConfig 셋팅 하는 경우가 있다.
{% highlight ruby %}
dev {
   resConfigs "ko", "xxxhdpi"
}
{% endhighlight %}

주의 해야 할 점은 ko xxxhdpi 리소스를 제외한 모든 resource를 지우고 빌드 하기 때문에 실행 하고 나서 앱 내에서 언어를 변경 하게 되면 ko 리소스 빼고는 이미 지워진 상태이기 때문에 
locale 셋팅을 바꾸더라도 다른언어로 변경 되지 않는다.

이 부분을 인지하고 있지 않은 상태에서 다국어 지원을 할 경우 원인을 찾기 쉽지 않다.