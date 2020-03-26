---
layout: post
title: selector drawable 
description: "selector drawable"
modified: 2018-10-31
tags: [android]
categories: [android]
---

## selector

image selector
image_selector.xml 파일 만들기

```
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:drawable="@drawable/image_pressed" android:state_pressed="true"/>
    <item android:drawable="@drawable/image_default" />
</selector>
```

사용 : android:src="@drawable/image_selector"


text selector

text_selector.xml 파일 만들기
```
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:color="@color/color_pressed" android:state_pressed="true"/>
    <item android:color="@color/color_default" />
</selector>
```

사용 : android:textColor="@drawable/text_selector"