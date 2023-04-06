---
layout: post
title:  Custom Compose Horizontal Indicator
description: "Custom Compose Horizontal Indicator"
modified: 2023-04-07
tags: [Compose]
categories: [compose,kotlin]
---

Pager 를 사용하는 경우 Indicator 를 함께 보여주는 UI를 개발하는 경우가 종종 있다.

기본적인 Indicator 를 사용하는 경우 Accompanist 에서 제공되는 Indicator를 사용하면 된다.
### Accompanist Horizontal Indicator 를 사용한 gif

<figure>
    <p align="center">
	    <img src="/images/2023-04-07-IndicatorGeneral.gif" alt="" width="600" height="450"/>
	</p>
</figure>

하지만 보통 회사에서 사용하는 경우 특별한 효과가 들어가는 Indicator를 사용하는 케이스가 있다.
기존에 회사에서 라이브러리를 사용 했는데 Compose 버전은 지원하지 않아서 동일하게 적용 하기 위해 직접 만들어 보았다.
아래와 같이 드래그 좌우로 스와이프 할 때 물방울 같은 효과를 보여준다.

<figure>
    <p align="center">
	    <img src="/images/2023-04-07-IndicatorWaterDrop.gif" alt="" width="600" height="450"/>
	</p>
</figure>

CustomHorizontalPagerIndicator 라는 이름으로 Composable Canvas 를 통해 한땀한땀 그렸다.

### CustomHorizontalPagerIndicator 코드 링크
[https://github.com/kimyounghoons/CleanArchitectureCompose](https://github.com/kimyounghoons/CleanArchitectureCompose)


### accompanist indicator 라이브러리 관련 링크
[https://google.github.io/accompanist/pager](https://google.github.io/accompanist/pager/)