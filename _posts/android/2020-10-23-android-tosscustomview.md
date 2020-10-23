---
layout: post
title:  토스 금액 입력창 애니메이션 클론 코딩
description: 토스 금액 입력창 애니메이션 클론 코딩
modified: 2020-10-23
tags: [Android, Kotlin]
categories: [Android, Kotlin]
---
<figure>
    <p align="center">
	    <img src="/images/2020-10-23-android-toss-animation.gif" alt="" width="200" height="350"/>
	</p>
</figure>

토스 금액 입력창 애니메이션을 봤는데 생각보다 더 복잡한 애니메이션이 들어가 있었다.

애니메이션 종류는 다음과 같았다.

### 입력 할 때 애니메이션
1. 위에서 아래로 내려 감
2. 위에서 아래로 내려 갈때 투명도 변경
3. 금액이기 떄문에 콤마가 찍히는데 콤마는 생길 때 fade in

### 제거 할 때 애니메이션
1. 원래 위치에서 내려감
2. 원래 위치에서 내려갈때 투명도 변경
3. 콤마 제거 될 때 fade out

### 예정 금액 보다 많을 때 진동과 함께 좌우 흔들리는 애니메이션

### 총 7가지 애니메이션이 존재 했다.

처음 생각한 방법은 EditText 에서 onDraw 를 커스텀 해서 사용하는 방법이었다.

onDraw 에서 코드로 모두 그리려고 하니 너무 복잡해지는 것 같아 좀 더 간단한 방법이 없을까 고민을 하기 시작함...

어차피 입력은 오른쪽에서부터 쌓이니까 LinearLayout 으로 ViewGroup 을 만들고 텍스트 하나당 TextView 하나를 LinearLayout 으로 쌓아가는 방식으로 만들고 애니메이션을 각각의 뷰에 먹여주면 어떨까!?라는 생각을 하게 되었다.

이 방법을 사용해보니 좀 더 코드가 깔끔해지고 가독성도 많이 좋아 졌다.

예전부터 오픈소스 라이브러리를 만들어보고 싶었는데 도전!!

아래 링크를 타고 들어가보자~!

[https://github.com/kimyounghoons/PriceAnimationTextView](https://github.com/kimyounghoons/PriceAnimationTextView)