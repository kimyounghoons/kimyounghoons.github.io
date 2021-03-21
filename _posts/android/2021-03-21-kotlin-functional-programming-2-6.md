---
layout: post
title: 함수형 프로그래밍 2-6~2-7
description: 함수형 프로그래밍 2-6~2-7
modified: 2021-03-21
tags: [Android, Kotlin]
categories: [Android, Kotlin]
---

### 2-6 패턴 매칭

패턴 매칭이란 값,조건,타입 등의 패턴에 따라서 매칭 되는 동작을 수행하게 하는 기능을 말한다.

아래는 패턴 매칭의 예제 이다.
```java
import org.junit.Test

class PatternMatchingTest {

    @Test
    fun testPatternMatching() {
        println(checkValue("kotlin"))            //kotlin 출력
        println(checkValue(5))                   //1..5 출력
        println(checkValue(6))                   //6 or 8 출력
        println(checkValue(User("영훈", "주소")))  //User 출력
        println(checkValue(12))                  //else 출력
    }

    data class User(val name: String, val address: String)

    private fun checkValue(any: Any) = when (any) {
        "kotlin" -> "kotlin"
        in 1..5 -> "1..5"
        6, 8 -> "6 or 8"
        is User -> "User"
        else -> "else"
    }
}
```

#### 패턴 매칭의 한계에 대해 살펴 보자.

```java
    @Test
    fun testSecondPatternMatching() {
        println(checkValue(listOf(1,2,3)))
        println(checkValue(listOf("1","2","3")))
        println(checkValue(2))
    }

    private fun checkValue(any: Any) = when (any) {
        List<Int> -> "Int List"
        List<String> -> "String List"
        else -> "else"
    }
```
위의 코드는 컴파일 에러가 발생 된다.
리스트와 같은 매개변수를 포함하는 타입이나 함수의 타입에 대한 패턴 매칭을 지원하지 않기 때문이다.

패턴 매칭을 사용한 sum 함수를 만들려면 어떤식으로 구현 해야 할까?

```java
import org.junit.Test

class PatternMatchingTest {

    @Test
    fun testSum() {
        println(sum(listOf(1, 2, 3, 4, 5)).toString())
    }

    fun sum(numbers: List<Int>): Int = when {
        numbers.isEmpty() -> 0
        else -> numbers.first() + sum(numbers.drop(1))
    }

}
```
sum 재귀 함수를 사용하였다. List<Int> 를 비교 하지 못하기 때문에 size 를 비교해 구현 되었다.

### 2-7 객체 분해
객체 분해란 어떤 객체를 구성하는 프로퍼티를 분해하여 편리하게 변수에 할당하는 것을 말한다.

```java
class DestructuringDeclarationTest {
    @Test
    fun test(){
        val user = User("영훈", "주소")
        val (name , address) = user  //객체 분해 사용
        println("user name : "+user.name)
        println("객체분해 name : "+name)
        println("user address : "+user.address)
        println("객체분해 address : "+address)
    }

    data class User(val name: String, val address: String)
}
```
일반적으로 user 를 통해서 name , address 를 접근 하게 되는데 객체 분해는 한번 분해해서 할당하면 name , address 바로 접근 가능하다.