---
layout: post
title:  Decorator Pattern (디자인 패턴 3장)
description: "Decorator Pattern (디자인 패턴 3장)"
modified: 2020-04-10
tags: [Decorator Pattern]
categories: [java,kotlin]
---

#### 정의  
주어진 상황 및 용도에 따라 어떤 객체에 책임을 덧붙이는 패턴으로, 기능 확장이 필요할 때 서브클래싱 대신 쓸 수 있는 유연한 대안이 될 수 있다.

#### 디자인 원칙  
확장에는 열려있고 변경에는 닫혀 있어야 한다는 원칙이다.(OCP - Open Close Principle)

<figure>
	<img src="/images/2020-04-10-android-decorator-pattern.png" alt="">
</figure>

카페에 가면 커피를 주문하고 추가적으로 밀크나 모카 휘핑을 추가 하게 되는데 이부분에서 요금이 추가 된다고 가정한다.  
HouseBlend,DarkRoast,Espresso 의 기본 클래스에 밀크,모카,휘핑 등을 필요할때 추가 해서 요금을 정산 할 수 있다.  
아래의 코드를 보자.
```
@Test
fun decoratorPatternTest() {
    val houseBlend = HouseBlend()
    println("houseBlend 가격 : ${houseBlend.cost()}원")
    val houseBlendWithWhip = Whip(houseBlend)
    println("HouseBlend + Whip 가격 : ${houseBlendWithWhip.cost()}원")
    val houseBlendWithWhipAndMilk = Milk(houseBlendWithWhip)
    println("HouseBlend + Whip + Milk 가격 : ${houseBlendWithWhipAndMilk.cost()}원")
}

abstract class Beverage {
    abstract fun cost(): Long
}

class HouseBlend : Beverage() {
    override fun cost(): Long {
        return 5000
    }
}

abstract class BeverageDecorator(protected val beverage: Beverage) : Beverage()

class Whip(beverage: Beverage) : BeverageDecorator(beverage) {
    override fun cost(): Long {
        return beverage.cost() + 500
    }
}

class Milk(beverage: Beverage) : BeverageDecorator(beverage) {
    override fun cost(): Long {
        return beverage.cost() + 1000
    }
}

```
Whip 과 Milk 가 Beverage 를 상속 받아서 만들게 되면 Whip과 Milk 가 Beverage 인지 Decorator 인지 바로 알기 힘들다.  
BeverageDecorator 클래스를 만든건 Whip 과 Milk 가 Decorator 를 상속 받아서 장식인걸 바로 알수 있고 HouseBlend 는 Beverage 를 상속 받아 음료 종류 라는걸 바로 알 수 있다.  

테스트 결과는 아래 사진과 같다.
<figure>
	<img src="/images/2020-04-10-android-decorator-pattern-02.png" alt="">
</figure>

밀크가 두번 이상,모카 두번 이상,휘핑 두번 이상이 들어 가더라도 추가적인 객체를 생성하지 않고 기능을 확장해서 사용할 수 있는 장점이 있다.  
하지만 데코레이터가 많아지면 해당 객체를 감싸는 클래스들이 많아지고 디버깅하기가 힘들어 질 수 있는 단점이 있다.  
그리고 실제 마지막 반환 되는 객체가 원래의 HouseBlend , DarkRoast , Espresso 가 아니라 마지막 Decorator 라는 것을 알고 있어야 한다.  
이미지 참조 링크 
http://nsnotification.blogspot.com/2013/01/decorator-pattern.html