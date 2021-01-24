---
layout: post
title: 함수형 프로그래밍 1장
description: 함수형 프로그래밍 1장
modified: 2021-01-24
tags: [Android, Kotlin]
categories: [Android, Kotlin]
---

함수형 프로그래밍 특징
1. 불변성(immutable)
2. 참조 투명성(referential transparency)
3. 일급 함수(first-class-fuction)
4. 게으른 평가(lazy evaluation)

#### 불변성(immutable) 순수 함수에 대해 알아보자.
순수한 함수는 동일한 입력에 동일한 결과를 돌려 준다.

```java
class Test {
    var z = 10

    @Test
    fun fucTest() {
        println("plus : " + plus(2, 3)) //5 출력
        println("plus2 : " + plus2(2, 3).toString()) //15출력
    }

    fun plus(x: Int, y: Int): Int {
        return x + y
    }

    fun plus2(x: Int, y: Int): Int {
        return x + y + z
    }
}
```

첫번째 plus 함수는 어떤 값이 와도 두개의 값을 더해서 값을 돌려줘서 순수 함수라고 볼수 있다.

두번째 plus2 함수는 z 값이 중간에 변경되면 동일 입력 값이라도 결과 값이 변경될 수 있기 때문에 순수한 함수라고 볼 수 없다.

#### 참조 투명성에 대해 알아보자.

위의 plus 함수는 x,y 참조 되어 있기 때문에 참조 투명한 함수라고 볼 수 있고 plus2는 z가 추가적으로 참조 되어 있기 때문에 불투명하다.

plus2 를 투명하게 변경한다면 아래와 같이 변경 되어야 한다.

```java
class Test {

    @Test
    fun fucTest() {
        println("plus2 : " + plus2(2, 3, 5).toString()) //10출력
    }

    fun plus2(x: Int, y: Int, z: Int): Int {
        return x + y + z
    }
}
```

#### 일급 함수에 대해 알아보자.
일급 함수를 알아보기 전에 일급 객체라 뭔지 알고 가자.

일급 객체란?
1. 객체를 함수의 매개변수로 넘길 수 있다.
2. 갹체를 함수의 반환값으로 돌려 줄 수 있다.
3. 객체를 변수나 자료구조에 담을 수 있다.

코틀린 최상위 개념인 Any는 일급 객체이다.

일급 함수란?
1. 함수를 함수의 매개변수로 넘길 수 있다.
2. 함수를 함수의 반환값으로 돌려 줄 수 있다.
3. 함수를 변수나 자료구조에 담을 수 있다.

```java
class Test {

    var funcList: List<(Int) -> String> = listOf { value -> value.toString() }//함수를 자료구조에 담음

    fun doSomthing(func: (Int) -> String) {//함수를 함수의 매개변수로 넘김
        //doSomething
    }

    fun doSomething(): (Int) -> String {//함수를 함수의 반환값으로 돌려줌
        return { value -> value.toString() }
    }
}
```

#### 게으른 평가(lazy evaluation) 에 대해 알아보자

```java
class Test {
    val lazyValue: String by lazy {
        println("시간 오래 걸리는 작업")
        "hi"
    }


    @Test
    fun test(){
        println(lazyValue)
        println(lazyValue)
    }
}
```

<figure>
	<img src="/images/2021-01-24-functinal-programming-01.png" alt="">
</figure>

lazy 는 기본적으로 값이 필요한 시점에 평가되기 때문에 프로그래머가 평가 시점을 지정할 수 있다.

시간이 오래 걸리는 작업은 한번만 평가 되고 그다음 호출 되지 않는다.

값이 지속적으로 변경 되는 값인데 lazy를 착각해서 잘못사용하게 되는 경우 잘못된 값을 리턴 받을 수 있다.

함수형 프로그래밍의 기본적인 개념을 알아보는 시간을 가졌다.

11장까지 있으니 올해도 화이팅 ㅎ