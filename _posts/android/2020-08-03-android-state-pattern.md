---
layout: post
title:  State Pattern (디자인 패턴 12장)
description: "State Pattern (디자인 패턴 12장)"
modified: 2020-08-03
tags: [State Pattern]
categories: [java,kotlin]
---

### 스테이트 패턴 정의
객체의 내부 상태가 바뀜에 따라서 객체의 행동을 바꿀 수 있다.  
객체의 클래스가 바뀌는 것과 같은 결과를 얻을 수 있다.  

<figure>
	<img src="/images/2020-08-03-android-state-pattern.png" alt="">
</figure>

### 스테이트 패턴 언제 사용할까!?
현재 기분 상태를 예를 들어 보자 
Good , Bad , Normal 상태가 있다.  
그리고 rain (비가 옴), clear (맑음) , vacation (휴가)  메소드가 있다.  
비가 옴 , 맑음 , 휴가 일때 상태가 변경 된다.  
아래의 코드를 보자.  

```java
class StatePattern {
    @Test
    fun test() {
        val stateContext = StateContext()
        stateContext.state = BadState(stateContext)
        stateContext.clear()
        stateContext.rain()
        stateContext.vacation()
        stateContext.rain()
    }

    interface State {
        fun rain()
        fun clear()
        fun vacation()
    }

    class GoodState(private val stateContext: StateContext) : State {
        override fun rain() {
            println("비가와서 기분 좋음에서 보통으로")
            stateContext.state = stateContext.normalState
        }
        override fun clear() {
            println("날씨 맑아서 기분 좋지만 최상의 상태")
        }
        override fun vacation() {
            println("휴가다~~! ^^ 하지만 최상의 상태")
        }
    }

    class NormalState(private val stateContext: StateContext) : State {
        override fun rain() {
            println("비가와서 기분 보통에서 나쁨으로")
            stateContext.state = stateContext.badState
        }
        override fun clear() {
            println("날씨 맑아서 기분 보통에서 좋음 변경")
            stateContext.state = stateContext.goodState
        }
        override fun vacation() {
            println("휴가다~~! ^^ 기분 보통에서 좋음 변경")
            stateContext.state = stateContext.goodState
        }
    }

    class BadState(private val stateContext: StateContext) : State {
        override fun rain() {
            println("비가 오지만 이보다 더 안좋을 수 없다.")
        }
        override fun clear() {
            println("날씨 맑아서 기분 안좋음에서 보통으로 변경")
            stateContext.state = stateContext.normalState
        }
        override fun vacation() {
            println("휴가다~~! ^^ 기분 안좋음에서 좋음으로 두단계 업!!")
            stateContext.state = stateContext.goodState
        }
    }


    class StateContext {
        var state: State? = null
        var goodState = GoodState(this)
        var normalState = NormalState(this)
        var badState = BadState(this)
        fun rain() {
            state?.rain()
        }
        fun clear() {
            state?.clear()
        }
        fun vacation() {
            state?.vacation()
        }
    }
}
```

얼핏 보면 스트레티지 패턴과 동일하다고 볼 수 있겠지만 차이점이 있다.  
스트레티지 패턴의 경우는 어떤 알고리즘의 행동 자체를 건네주는 반면 스테이트 패턴의 경우는 비슷하긴 하지만 미리 정해진 상태 전환 규칙을 바탕으로 알아서 자기 상태를 변경하는 점이 다르다.  
구조가 비슷하긴 하지만 용도나 목적이 완전 다르다.  

아래는 테스트 결과이다.
<figure>
	<img src="/images/2020-08-03-android-state-pattern-02.png" alt="">
</figure>

참조 링크
https://en.wikipedia.org/wiki/State_pattern