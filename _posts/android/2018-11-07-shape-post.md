---
layout: post
title: Shape
description: "Shape in xml"
modified: 2018-11-07
tags: [shape]
categories: [android]
---

꼭지점 두루뭉실한 사각형 만들기

drawable/파일명.xml 만들어 준다.

{% highlight ruby %}
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android">

    <solid android:color="@color/white"/>

    <stroke android:width="1dp"
        android:color="@color/gray"/>

    <padding android:bottom="10dp"
        android:left="10dp"
        android:right="10dp"
        android:top="10dp"/>

    <corners android:radius="7dp"/>
</shape>
{% endhighlight %}

solid 안에 컬러는 도형의 내부 색상 

stroke 바깥 선 색상과 두께

padding 은 도형 안쪽 여백 

corners radius 속성을 먹이면 사각형은 꼭지점 부분이 두루뭉실하게 변함 값이 높을수록 더 심해짐