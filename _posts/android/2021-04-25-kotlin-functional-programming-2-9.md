---
layout: post
title: 함수형 프로그래밍 2-9
description: 함수형 프로그래밍 2-9
modified: 2021-04-25
tags: [Android, Kotlin]
categories: [Android, Kotlin]
---

### 제네릭
제네릭은 객체 내부에서 사용할 데이터 타입을 외부에서 정하는 기법이다.

제네릭을 사용하면 클래스를 선언할 때 타입을 확정 짓지 않고, 클래스가 객체화되는 시점에 타입이 결정된다.

다음은 예제를 보자.
```java
class GenericTest {

    @Test
    fun test() {
        val box  = Box (1)
    }

    class Box(var value : Int)

    @Test
    fun test2(){
        val secondBox = SecondBox("secondBox")
    }

    class SecondBox<T>(var value : T)
}
```

Box 클래스는 타입이 Int 인 value 를 가지고 있다.

제네릭을 사용하지 않고 클래스를 선언한 예이다.

여기서는 클래스가 가진 값이 Int 타입으로 고정되어 있어서 Int 값만 받을 수 있다.

다른 타입도 담고 싶은 경우에 SecondBox 처럼 제네릭을 사용하면 해결할 수 있다.

### 제네릭 함수 선언
리스트에 포함된 숫자의 합을 구하는 함수라면 타입이 Int나 Double 등이 사용될 것이다.

이럴때는 제네릭의 사용이 적합하지 않지만 모든 타입에 잘 작동하는 함수라면 다르다.

예를 들어 리스트의 첫번째 값을 꺼내오는 함수라면 타입에 관계없이 동작하므로 제네릭을 활용해 일반화 하기 적합하다.

구현 코드는 아래와 같다.

```java
fun <T> head(list: List<T>): T{
        if(list.isEmpty()){
            throw NoSuchElementException()
        }
        return list[0]
}
```


