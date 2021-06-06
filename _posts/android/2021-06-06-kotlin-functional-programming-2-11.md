---
layout: post
title: Kotlin 변성(Variance)
description: Kotlin 변성(Variance)
modified: 2021-06-06
tags: [Android, Kotlin]
categories: [Android, Kotlin]
---

변성(Variance)은 자바나 코틀린뿐 아니라 다른 언어에서도 존재하는 개념이다.  
변성을 제대로 이해하려면 "타입 S가 T의 하위 타입일 때, Box[S]가 Box[T]의 하위 타입인가? 라는 질문에서 시작 해야 한다.

Box[S]와 Box[T] 는 상속 관계가 없다. -> 무공변
Box[S]는 Box[T]의 하위 타입 이다. -> 공변
Box[T]는 Box[S]의 하위 타입 이다. -> 반공변

타입 S와 T의 관계가 동일한 방향의 상하위 관계이면 공변 아니면 반공변 관계가 없으면 무공변으로 이해하면 된다.

공통으로 사용할 클래스들

```java
interface Box<T>
open class Language
open class JVM: Language()
class Kotlin: JVM()
```

Kotlin < JVM < Language 상속 관계로 볼 수 있다.

Box 인스턴스 생성

```java
val languageBox = object : Box<Language> {}
val jvmBox = object : Box<JVM>{}
val kotlinBox = object : Box<Kotlin>{}
```

모든 박스는 상속 관계가 존재 하지 않는다.

### 무공변의 의미와 예시

```java 
fun main(args: Array<String>){
    invariant(lanuageBox) //컴파일 오류
    invariant(jvmBox)
    invariant(kotlinBox) //컴파일 오류
}
fun invariant(value: Box<JVM>){}
```
타입이 Box<JVM> 으로 선언 되었기 때문에 jvmBox 외에 매개변수를 받을 수 없다.  
invariant의 매개변수는 무공변이다.

### 공변의 의미와 예시
```java 
fun main(args: Array<String>){
    invariant(lanuageBox) //컴파일 오류
    invariant(jvmBox)
    invariant(kotlinBox)
}
fun invariant(value: Box<out JVM>){}
```
타입이 Box<out JVM> 으로 선언 되었기 때문에 jvmBox,kotlinBox 매개변수를 받을 수 있다.
invariant의 매개변수는 공변이다.

### 반공변의 의미와 예시
```java 
fun main(args: Array<String>){
    contravariant(lanuageBox)
    contravariant(jvmBox)
    contravariant(kotlinBox)//컴파일 오류
}
fun contravariant(value: Box<in JVM>){}
```
타입이 Box<out JVM> 으로 선언 되었기 때문에 lanuageBox,jvmBox 매개변수를 받을 수 있다.
contravariant 매개변수는 반공변이다.  

### in out 키워드를 사용할 때 주의할 점이 있다.

out 부터 보자.
```java 
interface Box2<out T> {
    fun read(): T
    fun write(value: T) // 컴파일 오류
}
```
T를 읽어서 반환할 때는 어떤 상위 타입에 하위 타입을 할당하는 것이 가능하기 떄문에 문제가 없지만 write()함수는 value 값을 받아서 처리 할때 어떤 하위 타입이 들어올지
알 수 없기 때문에 런타임 오류가 발생할 가능성이 있다.

in 예제를 보자.
```java 
interface Box2<in T> {
    fun read(): T //컴파일 오류
    fun write(value: T)
}
```
반공변으로 선언 했을 때는 read함수에서 컴파일 오류가 발생 한다.
그 이유는 호출자가 선언한 T 타입의 변수에 T보다 상위 타입의 값을 할당하면 런타임 오류가 발생하기 때문이다.

in, out 키워드의 의미는 문자 그대로 이해하면 쉽다.  
in 으로 선언된 타입은 입력값에만 활용(읽기 전용)할 수 있고 
out으로 선언된 타입은 반환값(출력)의 타입으로만 사용할 수 있다.
그렇게 되면 프로그래머의 의도치 않은 실수를 컴파일 타임에 막을 수 있다.

 Kotlin 변성(Variance)에 대해 알아 보았다.