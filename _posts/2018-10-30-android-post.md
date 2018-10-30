---
layout: post
title: 다중 매니페스트 파일 병합
description: "다중 매니페스트 파일 병합"
modified: 2018-10-30
tags: [android]
categories: [android]
---

## tools:replace="attr,..."

android:allowBackup 을 false 로 변경 시킬 때 아래와 같은 에러가 나왔다.

{% highlight css %}
Manifest merger failed : Attribute application@allowBackup value=(false) from AndroidManifest.xml:26:9-36
	is also present at [com.github.danylovolokh:hashtag-helper:1.1.0] AndroidManifest.xml:12:9-35 value=(true).
	Suggestion: add 'tools:replace="android:allowBackup"' to <application> element at AndroidManifest.xml:24:5-208:19 to override.
{% endhighlight %}

문제는 manifest 병합 문제..
hashtag 라이브러리에서도 allowBackup 속성이 설정 되어있어서 문제가 되었다.
 
해결 방법은 다음과 같이 tools:replace="android:allowBackup" 을 설정 해주면 해당 manifest android:allowBackup 속성이 우선순위가 높아진다.

{% highlight css %}
 <application
        android:name=".name"
        android:allowBackup="false"
        android:icon="${appIcon}"
        android:label="@string/app_name"
        android:supportsRtl="false"
        android:theme="@style/AppTheme"
        tools:replace="supportsRtl,allowBackup">
{% endhighlight %}

다른곳에서 allowBackup 설정이 true 가 되어 있어도 해당 manifest 가 우선순위가 높기 때문에 false 가 적용 된다.