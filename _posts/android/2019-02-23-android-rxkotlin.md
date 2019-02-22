---
layout: post
title: RxKotlin Single
description: "RxKotlin Single"
modified: 2019-02-22
tags: [RxKotlin,Single]
categories: [RxKotlin,Single]
---

Rx 공부한지 별로 되진않았지만 확실히 매력있는 친구다.ㅎㅎ

Single 에 대해 궁금증이 생겼다. 

Single은 onSuccess 나 onError 중 하나가 불리게 되면 dispose 가 된다.
하지만 오픈 소스들을 보면 CompositeDisposible 에 차곡차곡 쌓고 dispose 시켜준다.
이렇게 되면 중복으로 처리하는게 아닌가 ?? 
왜 그러지 라는생각이 듬.. 

이유는 구독중 onSuccess 또는 onError 가 불리기 전에 이슈가 발생할 우려가 높기 때문에 추가해주고 dispose시켜주는것 같음.ㅎㅎ