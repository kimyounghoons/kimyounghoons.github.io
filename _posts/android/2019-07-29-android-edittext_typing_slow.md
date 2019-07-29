---
layout: post
title: EditText 금액 콤마 표시 Typing 할 때 느려지는 현상
description: "EditText 금액 콤마 표시 Typing 할 때 느려지는 현상"
modified: 2019-05-12
tags: [editText,textWatcher]
categories: [android]
---

### EditText 금액 콤마 표시 Typing 할 때 느려지는 현상

EditText 금액 적을 때 3자리 마다 , 표시를 해야하는 상황이 생겼다.
TextWatcher 에서 처리 하였고 정상 작동은 했지만 버벅거리는 모습이 보였다.
해당 이슈를 찾기 위해 소요시간이 걸릴만한곳에 로그를 찍으면 소요시간을 측정 했다.
EditText.setText() 부분에서 소요시간이 조금 길어서 TextWatcher 에서 주는 Editable s 을 s.clear 와 s.append 를 통해 추가 해 주었고 
{% highlight ruby %}
android:inputType="number|textNoSuggestions"
{% endhighlight %}
inputType 을 number 에서 위와 같이 변경 해 준뒤 해당 이슈를 수정 할 수 있었고 변경 시 빠르게 변경할 수 있었다.