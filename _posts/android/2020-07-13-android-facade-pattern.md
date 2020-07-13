---
layout: post
title:  Facade Pattern (디자인 패턴 9장)
description: "Facade Pattern (디자인 패턴 9장)"
modified: 2020-07-13
tags: [Facade Pattern]
categories: [java,kotlin]
---

### 퍼사드 패턴 정의
어떤 서브시스템의 일련의 인터페이스에 대한 통합된 인터페이스를 제공합니다.  
퍼사드에서 고수준 인터페이스를 정의하기 때문에 서브시스템을 더 쉽게 사용할 수 있습니다.

디자인 원칙 
최소 지식 원칙 - 정말 친한 친구하고만 얘기하라.

### 퍼사드 패턴 언제 사용할까!?

아래의 코드를 보자.  
```java

class FacadePattern {

    @Test
    fun test() {
        val computerFacade = ComputerFacade(Monitor(), Speaker())
        computerFacade.on()
        println("----------------------------------")
        computerFacade.off()
    }

    class ComputerFacade(private val monitor: Monitor, private val speaker: Speaker) {
        fun on() {
            monitor.on()
            speaker.on()
            speaker.volume = 5
        }

        fun off() {
            speaker.volume = 0
            speaker.off()
            monitor.off()
        }
    }

    class Monitor {
        fun on() {
            println("Monitor on")
        }

        fun off() {
            println("Monitor off")
        }
    }

    class Speaker {
        var volume = 0
            set(value) {
                println("volume set : $value")
                field = value
            }

        fun on() {
            println("Speaker on")
        }

        fun off() {
            println("Speaker off")
        }
    }

}

```
위의 코드를 보면 클라이언트 입장에서 ComputerFacade 만 알고 있으면 컴퓨터를 키고 끄는데 문제가 없다.  
위의 클래스들은 간단하게 구현 했기때문에 쉽게 보일 수 있지만 컴퓨터가 부팅 되는 과정 까지 모두 포함된 내용을 클라이언트에서 알아야 한다면!?  
클라이언트 입장에서 컴퓨터를 키는 도중에 지쳐서 그만둘지도 모른다.  
이런 과정들을 최소한으로만 알게 하기 위해 퍼사드 패턴을 사용한다.  
아래는 테스트 결과 이다.

<figure>
	<img src="/images/2020-07-13-android-facade-pattern.png" alt="">
</figure>