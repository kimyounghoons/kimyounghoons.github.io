---
layout: post
title: DataBidning , BindingAdapter
description: "DataBidning , BindingAdapter"
modified: 2019-12-26
tags: [DataBinding,BindingAdapter]
categories: [android]
---

데이터 바인딩라이브러리는 레이아웃의 UI 구성 요소를 프로그래밍 방식이 아닌 선언적 형식을 사용하여 앱의 데이터 소스에 바인딩 할 수있는 지원 라이브러리다.  

```
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:bind="http://schemas.android.com/tools"
    xmlns:tools="http://schemas.android.com/tools">
    <data>
        <import type="android.view.View" />
        <variable
            name="viewModel"
            type="패키지경로.ExampleViewModel" />
    </data>
    <androidx.constraintlayout.widget.ConstraintLayout
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:background="@{viewModel.isRedBackground ? @color/red : @color/white}">
    </androidx.constraintlayout.widget.ConstraintLayout>
</layout>

```

예를 들어 API 호출을 하고 받아온 값이 빨간색 배경색을 가져야 한다면 viewModel.isRedBackground (ObservableField Boolean 값) 데이터 변경 만으로 뷰가 빨간색 또는 흰색으로 변경 된다. 하지만 상태값이 빨간색 흰색 그외 초록색이 필요하다면 BindingAdapter를 통해서 구현할 수 있다. 


android:background="@{viewModel.isRedBackground ? @color/red : @color/white}"  
--> bind:status="@{viewModel.status}"  

위의 코드에서 해당 줄만 변경한다. 그 다음 BindingAdapter를 만들어준다.  

```
@BindingAdapter("bind:status")
fun bindBuyingStatusText(imageView: ImageView, status: String?) {
    when (status) {
        "빨간색" -> {
            imageView.setBackgroundColor(imageView.context.getColor(R.color.color_red))
        }
        "흰색" -> {
            imageView.setBackgroundColor(imageView.context.getColor(R.color.color_white))
        }
        else -> {  //초록색
            imageView.setBackgroundColor(imageView.context.getColor(R.color.color_green))
        }
    }
}

```

상태값이 2가지만 있는경우에도 BindingAdapter 사용해서 처리할수 있지만 굳이 사용할 필요는 없을것 같다.