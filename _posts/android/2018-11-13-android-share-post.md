---
layout: post
title: share Intent
description: "안드로이드 외부 공유"
modified: 2018-11-13
tags: [share]
categories: [android]
---

# 안드로이드 외부 공유

외부공유 소스 
{% highlight ruby %}
String title = context.getString(R.string.title);
String plainContent = context.getString(R.string.plain_content);
String htmlContent = context.getString(R.string.html_content);

Intent shareIntent = new Intent(Intent.ACTION_SEND);
shareIntent.setType("text/plain");
shareIntent.putExtra(Intent.EXTRA_SUBJECT, title);
shareIntent.putExtra(Intent.EXTRA_TEXT, plainContent);
shareIntent.putExtra(Intent.EXTRA_HTML_TEXT, htmlContent);
context.startActivity(Intent.createChooser(shareIntent, context.getString(R.string.share_text)));
{% endhighlight %}

Intent.EXTRA_TEXT , Intent.EXTRA_HTML_TEXT  두가지 타입 모두 보내준다.
앱을 통해 공유 하게 되면 선택 되는 앱이 다양하기 때문에 그냥 TEXT 정보를 가져가는 앱이 있고 HTML_TEXT 정보를 가져가는 앱이 있다.
Content 내용에 링크를 걸지 않는 다면 Intent.EXTRA_TEXT 만 작성하더라도 괜찮다.
HTML_TEXT 가 없을 경우 EXTRA_TEXT 정보를 가져간다.
G-mail 경우는 Intent.EXTRA_TEXT 만 넣어 줬을 때도 링크가 걸리는걸 볼수 있었다. 몇몇 앱은 지원이 되지만 안되는 예외 사항이 있기 때문에 모두 지원 하려면 HTML_TEXT를 같이 넣어 주는게 좋다.