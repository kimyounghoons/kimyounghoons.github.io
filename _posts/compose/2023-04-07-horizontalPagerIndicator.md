---
layout: post
title:  Compose Horizontal Indicator
description: "Compose Horizontal Indicator"
modified: 2023-04-07
tags: [Compose]
categories: [compose,kotlin]
---

### Horizontal Indicator 를 커스텀 해서 만들어 보았다.
Compose 로 Pager 를 구현하면 Indicator 를 사용하게 될 때가 있다.

Accompanist 에서 제공되는 라이브러리를 사용하면 아래와 같은 일반적인 Indicator 를 쉽게 사용 할 수 있다.

<figure>
    <p align="center">
	    <img src="/images/2023-04-07-IndicatorGeneral.gif" alt="" width="600" height="450"/>
	</p>
</figure>

기존에 회사에서 라이브러리를 사용 했는데 Compose 버전은 지원하지 않아서 동일하게 적용 하기 위해 직접 만들어 보았다.
IndicatorType 을 GeneralType(Accompanist에서 지원되는 Type) 과 WaterDropType 을 만들어서 분기 처리 했다.
아래와 같이 드래그 좌우로 스와이프 할 때 물방울 같은 효과를 보여준다.

<figure>
    <p align="center">
	    <img src="/images/2023-04-07-IndicatorWaterDrop.gif" alt="" width="600" height="450"/>
	</p>
</figure>

CustomHorizontalPagerIndicator 라는 이름으로 Composable Canvas 를 통해 한땀한땀 그렸다.
코드는 아래 링크를 통해 들어가서 볼 수 있다.

https://github.com/kimyounghoons/CleanArchitectureCompose

accompanist indicator 라이브러리
https://google.github.io/accompanist/pager/