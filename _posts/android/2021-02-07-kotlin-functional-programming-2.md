---
layout: post
title: 함수형 프로그래밍 2-1장
description: 함수형 프로그래밍 2-1장
modified: 2021-02-07
tags: [Android, Kotlin]
categories: [Android, Kotlin]
---

### 코틀린으로 함수형 프로그래밍 시작하기

#### 2.1 프로퍼티 선언과 안전한 널 처리
```java
val a: String = "a" // 읽기 전용 프로퍼티(값을 변경 할 수 없음)
var a: String ="a" //가변 프로퍼티 (값을 다른 값으로 변경 할 수 있다.)
```

#### 타입 추론을 사용한 선언
```java
val a ="a"
var b ="b"
```

a 가 String 타입을 코틀린 컴파일러가 추론하기 때문에 String 을 생략할 수 있다.

#### 안전한 널처리
```java
val a: String = null //컴파일 오류 발생!!
val a: String? = null
```

#### 2.2 함수와 람다

함수를 선언 하는 방법 3가지

```java
fun sum(a:Int, b: Int): Int {
    return a + b
}

fun sum(a:Int, b: Int): Int  = a + b 

fun sum(a:Int, b: Int) = a + b 
```

#### 파라미터 디폴트 값 셋팅
```java
fun sum(a:Int = 0, b: Int = 0) = a + b //sum() 호출 하면 return 0+0 과 같다.
```

#### 익명 함수와 람다 표현식
```java
fun calculate(x: Int, y: Int, calculate: (Int,Int)-> Int): Int {
    return calculate(x,y)
}

사용 calculate(1,2, {x,y->x+y})
return 1+2 
결과값 3
```

#### 확장 함수
```java
fun Int.sum(value: Int): Int {
    return this + value
}
이미 작성된 Int 클래스에 sum 함수를 생성 할 수 있다. 
```

### 2.3 제어 구문
#### if문
자바와 달리 if문이 구문으로 쓰이기도 하고 표현식으로 사용되기도 한다.

#### if 구문 사용
```java
val isTrue : Boolean
if(x > y){
    isTrue = true
}else{
    isTrue = false
}
```

#### if 표현식
```java
val isTrue = if(x>y){ true } else { false }
true , false 라는 값을 isTrue 변수에 반환
```

#### when문
자바의 switch와 비슷한 기능
```java
when(value){
    1 ->{println("value==1")} //value 가 1인 경우
    2,3 ->{println("value 2 or value 3")} //value 가 2 또는 3인 경우
    else ->{println("else")} //그 외의 경우
}
```

#### when 표현식
```java
val value = when {
    x == 1 -> 1
    x == 2 -> 2
    else -> 3 
}
```

#### for문
```java 
val list = listOf(1,2,3,4,5)

for(item in list){
    print(item) //12345 출력
}
for((index, item) in list.withIndex()){
    println("index: $index item: $item")
    // println("index: 0 item: 1")
    // println("index: 1 item: 2")
    // println("index: 2 item: 3")
    // println("index: 3 item: 4")
    // println("index: 4 item: 5")
} 

for(i in 1..5){
    print(i) 12345 출력
}
for(i until 1..5){
    print(i) 1234 출력
}
for(i in 6 downTo 0 step 2){
    print(i) // 6420 출력
}
```

### 2.4 인터페이스
#### 인터페이스 특징
1. 다중 상속이 가능하다
2. 추상 함수를 가질 수 있다
3. 함수의 본문을 구현할 수 있다
4. 여러 인터페이스에서 같은 이름의 함수를 가질 수 있다
5. 추상 프로퍼티를 가질 수 있다

#### 1번, 2번 예제 (다중 상속이 가능하다, 추상 함수를 가질 수 있다)
```java
interface A {
    printA() //추상 함수
}
interface B {
    printB() // 추상 함수
}
class Model : A,B { //A 와 B 다중 상속
    override fun printA(){
        //하고 싶은 구현
    }
    override fun printB(){
        //하고 싶은 구현
    }
}
```
#### 3번 예제 (함수의 본문을 구현할 수 있다)
```java
interface A {
    printA(){
        print("A")
    }
}
```
#### 4번 예제 (여러 인터페이스에서 같은 이름의 함수를 가질 수 있다)
```java
interface A {
    printTest(){
        print("A")
    }
}
interface B {
    printTest(){
        print("B")
    }
}
class Model : A,B{
    //Model 클래스의 printTest 사용 시 A 의 printTest 구현부 , B 의 printTest 구현부 둘다 호출
    override fun printTest(){
        super<A>.printTest()
        super<B>.printTest()
    }
}   
}
```
#### 5번 예제 (추상 프로퍼티를 가질 수 있다)
```java
interface A {
    val a : Int
}
class Model {
    override val a: Int = 1 //val ,var 둘다 가능하다.
}
```