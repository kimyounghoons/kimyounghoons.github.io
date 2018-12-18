---
layout: post
title: ConstraintLayout 사용 
description: "ConstraintLayout 사용"
modified: 2018-12-18
tags: [constraintLayout]
categories: [android]
---

ConstraintLayout 의 강점을 살려보자.

<figure class="half">
	<img src="/images/2018-12-18-constraintlayout-01.png" alt="">
    <img src="/images/2018-12-18-constraintlayout-02.png" alt="">
	<figcaption>초록색 링이 꺼졌다 켜졌다 해야할 때 layout 어떻게 배치 할까!?</figcaption>
</figure>

보통 디자이너 분들은 해당 다운로드 이미지, 공유 이미지,초록색 링 이미지 이렇게 주신다.
RelativeLayout을 사용하게 되면 만들기가 쉽지 않다. 
기존에 RelativeLayout을 사용했을 때는 다운로드 이미지, 공유 이미지, 초록색 링이 포함된 다운로드 이미지 , 초록색 링이 포함된 공유 이미지 이렇게 받아서 작업을했다.
하지만 ConstraintLayout 을 사용했을 때 이런 문제점을 해결할 수 있었다. 물론 RelativeLayout 으로도 아마 가능할수도 있을지 모르지만 쉽게 떠오르지 않았다.

여기서 키워드는 GuideLine 과 ConstraintGroup 이다.

{% highlight ruby %}
<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:animateLayoutChanges="true">

    <View
        android:id="@+id/bottom_background"
        android:layout_width="match_parent"
        android:layout_height="80dp"
        android:background="@color/take_bottom_black"
        app:layout_constraintBottom_toBottomOf="parent" />

    <android.support.constraint.Guideline
        android:id="@+id/guideline_bottom_center_horizontal"
        android:layout_width="match_parent"
        android:layout_height="1dp"
        android:orientation="horizontal"
        app:layout_constraintGuide_end="40dp" />

    <android.support.constraint.Guideline
        android:id="@+id/guideline_share"
        android:layout_width="1dp"
        android:layout_height="match_parent"
        android:orientation="vertical"
        app:layout_constraintGuide_percent="0.33333" />

    <android.support.constraint.Guideline
        android:id="@+id/guideline_down"
        android:layout_width="1dp"
        android:layout_height="match_parent"
        android:orientation="vertical"
        app:layout_constraintGuide_percent="0.666666"/>

    <ImageView
        android:id="@+id/share_button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:src="@drawable/take_btn_finish_share"
        app:layout_constraintBottom_toBottomOf="@id/guideline_bottom_center_horizontal"
        app:layout_constraintEnd_toEndOf="@+id/guideline_share"
        app:layout_constraintStart_toStartOf="@+id/guideline_share"
        app:layout_constraintTop_toTopOf="@id/guideline_bottom_center_horizontal" />

    <ImageView
        android:id="@+id/down_button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:src="@drawable/take_btn_finish_save"
        app:layout_constraintBottom_toBottomOf="@id/guideline_bottom_center_horizontal"
        app:layout_constraintEnd_toEndOf="@+id/guideline_down"
        app:layout_constraintStart_toStartOf="@id/guideline_down"
        app:layout_constraintTop_toTopOf="@id/guideline_bottom_center_horizontal" />


    <ImageView
        android:id="@+id/image_share_guide"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:src="@drawable/quick_round_03"
        app:layout_constraintBottom_toBottomOf="@+id/guideline_bottom_center_horizontal"
        app:layout_constraintEnd_toEndOf="@+id/guideline_share"
        app:layout_constraintStart_toStartOf="@id/guideline_share"
        app:layout_constraintTop_toTopOf="@+id/guideline_bottom_center_horizontal" />

    <ImageView
        android:id="@+id/image_download_guide"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:src="@drawable/quick_round_03"
        app:layout_constraintBottom_toBottomOf="@+id/guideline_bottom_center_horizontal"
        app:layout_constraintEnd_toEndOf="@+id/guideline_down"
        app:layout_constraintStart_toStartOf="@id/guideline_down"
        app:layout_constraintTop_toTopOf="@+id/guideline_bottom_center_horizontal" />

    <android.support.constraint.Group
        android:id="@+id/group_quick_guide"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:visibility="gone"
        app:constraint_referenced_ids="image_share_guide,image_download_guide" />

</android.support.constraint.ConstraintLayout>
{% endhighlight %}

해당 이미지의 Layout 이다.
가로 세로의 각각 GuideLine 을 설정 한 후 각 뷰는 부모의 뷰에서 몇 dp 만큼 떨어진게 아니라 기준점이 GuideLine 이 되는 것이다.
쉽게 만들수 있다. 그리고 ConstraintGroup을 만들어서 referenceIds 에 뷰들이 GONE처리 되어야 하는 아이디들을 넣어 주면 된다.
group_quick_guide 를 찾아서 Visible Gone 처리를 해주면 참조한 뷰들이 모두 적용된다.
매우 편리하다!!ㅎㅎ