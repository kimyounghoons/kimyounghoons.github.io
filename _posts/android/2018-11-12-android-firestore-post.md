---
layout: post
title: index in firestore
description: "파이어스토어 색인을 이용한 복합쿼리"
modified: 2018-11-12
tags: [firestore]
categories: [android]
---

# 파이어스토어 색인을 이용한 복합쿼리

기존 Realtime Database 는 다중 쿼리를 지원하지 않아서 다중 쿼리 처럼 사용하기 위해서 필드를 하나 더 만들어야 하는 번거러움이 있었다.
하지만 firestore는 색인을 사용해서 다중 쿼리를 사용할 수 있다. 

<figure>
	<img src="/images/2018-11-12-index_in_firestore.png" alt="">
	<figcaption>FireStore 데이터 베이스의 컬렉션 ID로 구분 된다.</figcaption>
</figure>

처음 색인을 보면 answer 컬렉션 ID 를 가지는 곳에 다중 쿼리를 사용할 수 있는 것은 letterId, time 을 이용한 쿼리를 사용할 수있다.
letterId, answerId 라고 다중 쿼리를 사용하게 되면 등록된 색인이 없기 때문에 쿼리를 사용했을 때 fail로 응답이 온다.
추가적으로 더 많은 필드를 원하면 3개이상도 가능 하다.