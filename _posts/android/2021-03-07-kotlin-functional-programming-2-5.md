---
layout: post
title: 함수형 프로그래밍 2-5장
description: 함수형 프로그래밍 2-5장
modified: 2021-03-07
tags: [Android, Kotlin]
categories: [Android, Kotlin]
---

### 클래스와 프로퍼티

```java
class User(var name: String, val age: Int = 18)

val user = User("yh", 31)
println(user.name) //"yh" 출력

user.name = "yh2"
println(user.name) //"yh2" 출력

val user2 = User("yh")
println(user2.age) //18 출력
```
User class 는 age 가 default value 18로 세팅 되어 있어서 user2 객체의 age는 18 값이 나온다.

### data 클래스
기본적으로 게터 세터 함수를 생성해주고 hashCode, equals, toString 함수와 같은 자바 Object 클래스에 정의된 함수들을 자동으로 생성한다.

```java
data class Person(val name : String, val age : Int)
```

### enum 클래스
특정 상수에 이름을 붙여 주는 클래스다.
```java
enum class Error(val num : Int){
    WARN(2){
        override fun getErrorName(): String {
            return "WARN"
        }
    },
    ERROR(3){
        override fun getErrorName(): String {
            return "ERROR"
        }
    },
    FAULT(2){
        override fun getErrorName(): String {
             return "FAULT"
        }
    };
    abstract fin getErrorName(): String
}
```
Error 가 가진 WARN , ERROR , FAULT  모두 Int 형 num 프로퍼티와 getErrorName 함수를 가지고 있다.

### sealed 클래스
sealed 클래스는 새로운 타입을 확장할 수 있고 확장 형태로 클래스를 묶은 클래스이다.

```java
sealed class Color {
    object Red : Color()
    object Green : Color()
    object Blue : Color()

    fun print() {
        when (this) {
            Red -> {
                println("빨간색")
            }
            Green -> {
                println("초록색")
            }
            Blue -> {
                println("파란색")
            }
            //sealed class 인 경우 else 를 구현하지 않아도 됨.
        }
    }
}

@Test
fun test() {
    var color: Color = Color.Red
    color.print() //빨간색 출력
    color = Color.Blue
    color.print() //파란색 출력
}
```
