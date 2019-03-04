---
layout: post
title: 안드로이드 면접 관련 질문 
description: "안드로이드 면접 관련 질문"
modified: 2018-01-01
tags: [android,interview]
categories: [android,interview]
---

## 면접 보면서 느낀점 : 
제대로 알고 있어야 제대로 설명이 가능하다.

머리로는 아는데 말로 잘 안나오는 경우는 제대로 모르고 있는 경우인듯...

## 자바 질문 : 

### abstract 와 interface 의 차이 와 자바는 왜 다중상속이 안되는 것인가?
abstract 와 interface 는 자식 클래스 또는 인터페이스를 사용하는 곳에서 추상메소드를 강제로 사용하게끔 유도하는부분이 공통점이라고 할 수있다.
비슷비슷 하지만 다른점은 자바가 다중상속을 지원하지 않지만 다중인터페이스를 지원한다는 것에 있다. 다중 상속을 지원하지 않는 것은 모호성때문이다. 다중상속을 하게 되면 부모로부터 상속받은 method가 동일하게 되면 누구의 부모로부터 가져와야하는지 애매해진다. 그렇기 때문에 자바에서는 다중상속을 지원하지 않는다. 하지만 왜 인터페이스는 다중인터페이스가 가능할까? 그부분에서는 인터페이스는 어떠한 특정 동작을 보장하기 위해서 존재하기 때문이다.
이러한 부분이 차이점이라고 할 수 있다. 

### 쓰레드풀 설명


## 코틀린 질문 : 
### let, apply, with 차이점 
### kotlin sam 질문
### 코틀린 사용의 장점


## 안드로이드 질문 : 
### PagingLibrary 구현 질문 

### ViewModel 의 장점

### ConstraintLayout vs RelativeLayout vs LinearLayout 차이(ConstraintLayout 장점)

### CustomView 와 Fragment 의 차이점 

### Android KTX 란 무엇인가?
안드로이드 프레임워크와 서포트 라이브러리를 모두 지원하여 안드로이드를 위한 코틀린(Kotlin) 코드를 간결하고 편하게 사용할 수 있게 설계된 확장 라이브러리이다.
### mvp 패턴과 mvvm 패턴의 차이

### LiveData 는 인터럽트방식인가 폴링방식인가 

### Subscribe 와 Observer 차이

### 옵저버패턴 설명

### 어댑터패턴 설명

### 프로토콜이란?

### mvvm 구조에서 ViewModel 상속을 사용 안하게 된다면?

## Rx관련 질문 : 

### 새로운 스레드를 생성해도 되는데 왜 Schedulers.io() 를 사용하는가? 내부 동작이 어떻게 돌아가는 것인가를 묻는 질문