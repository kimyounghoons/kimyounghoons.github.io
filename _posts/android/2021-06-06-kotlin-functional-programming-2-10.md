---
layout: post
title: Kotlin Scope functions
description: Kotlin Scope functions
modified: 2021-06-06
tags: [Android, Kotlin]
categories: [Android, Kotlin]
---

1. apply
2. also
3. with
4. run
5. let

### apply

```java
fun <T>.apply(block: T.() -> Unit) : T
//T의 확장 함수 로서 block 함수의 입력을 람다 리시버로 받았으므로 block  내에서 this 없이 객체에 접근할 수 있다.  
//apply 는 객체 내부 프로퍼티를 변경할 때 사용한다.

Person().apply {
    name = "홍길동"
}
```

### also

```java
fun <T> T.also(block: (T) -> Unit) : T
//블럭의 반환 값이 없고 자기 자신을 반환한다.
//어떤 fun 호출 후 추가적으로 궁금한 부분이나 변경할 부분에 대해 사용한다.
fun getRandomInt(): Int {
    return Random.nextInt(100).also {
        Log.d("getRandomInt() generated value $it")
    }
}

val i = getRandomInt()
```

### with
```java
fun <T, R> with(receiver: T, block: T.() -> R) : R
//일반적인 함수로 선언 되어 있다. 
//receiver 받은 객체에 this 를 사용하지 않고 접근 가능 하고 block 함수에서 반환한 값을 그대로 반환한다.
//프로퍼티에 직접적으로 접근 하면서 return 값을 변경해서 사용해야 할때 사용한다.

val numbers = mutableListOf("one", "two", "three")
val firstAndLast = with(numbers) {
    "The first element is ${first()}," +
    " the last element is ${last()}"
}
println(firstAndLast)
```

### run
```java
fun <T,R> T.run(block: T.()->R) : R
//T의 확장 함수로 선언되었고 block 함수에 this 가 람다 리시버로 전달 된다.
//run은 람다에 객체 초기화와 반환 값 계산이 모두 포함되어있을 때 유용합니다.

val service = Service("url", 8888)

val result = service.run {
    port = 8080
    query(port)
}

//let 으로 사용했을때와의 차이를 보면 확실히 편하게 사용한걸 볼 수 있다.
val letResult = service.let {
    it.port = 8080
    it.query(it.port)
}
```

### let
```java
fun <T,R> T.let(block: (T)->R): R
//T의 확장 함수 이고 block 의 반환 값을 리턴한다.
// processNonNullString str 이 널이 아니어야 하는 상황 그리고 return 값이 필요할때 사용 한다.
val str: String? = "Hello"   
val length = str?.let { 
    processNonNullString(it)      // OK: 'it' is not null inside '?.let { }'
    it.length
}
```

5가지 Scopefunctions 에 대해 알아 보았다.  
앞으로 각각의 상황에 맞게 잘 사용하면 좋을 것 같다.