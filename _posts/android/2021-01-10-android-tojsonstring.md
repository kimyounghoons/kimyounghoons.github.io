---
layout: post
title: gson 라이브러리 활용
description: gson 라이브러리 활용
modified: 2021-01-10
tags: [Android, Kotlin]
categories: [Android, Kotlin]
---

gson 라이브러리는 json object 를 json string 으로 변환 시켜주거나 json string 을 json object 로 쉽게 변환 할 수 있게 만들어 주는 라이브러리이다.

라이브러리 추가는 app gradle dependency에 다음과 같이 추가해주면 사용 가능하다.

```java
dependencies {
  implementation 'com.google.code.gson:gson:2.8.6'
}
```

우리는 작업 하다보면 특정 Object 내부 필드를 모두 로그로 보고 싶을 때가 있다.

그렇게 되면 object 의 toString 를 override 해서 모든 필드에 대해 다시 정의를 해줘야 모두 노출이 가능하다.

그리고 필요한 object 마다 toString 을 재정의 해서 번거롭게 사용 해야 한다.

```java
class ToJsonStringTest {

    @Test
    fun toStringTest() {
        val order = Order(
                id = 1,
                goods = listOf(
                        Goods(id = 1, name = "나나", price = 3000L),
                        Goods(id = 2, name = "다다", price = 4000L)
                )
        )
        println(order.toString())
    }

    class Order(val id: Long, val goods: List<Goods>) {
        override fun toString(): String { //return 을 재정의 해줘야 함.
            return "id : $id , goods : $goods"
        }
    }

    class Goods(val id: Long, val name: String, val price: Long) {
        override fun toString(): String {//return 을 재정의 해줘야 함.
            return "id : $id , name : $name , price : $price"
        }
    }
}
```
아래는 order.toString 의 결과이다.
<figure>
	<img src="/images/2021-01-10-tojsonstring-01.png" alt="">
</figure>

이부분을 gson 라이브러리를 사용해서 다음과 같이 해결 하였다.
```java
fun Any.toJsonString(withNull: Boolean = false): String {
    return try {
        val builder = GsonBuilder().disableHtmlEscaping().setPrettyPrinting()
        if(withNull) builder.serializeNulls()
        builder.create().toJson(this)
    } catch (e: Exception) {
        e.printStackTrace()
        "Fail to print toJsonString()"
    }
}

class ToJsonStringTest {

    @Test
    fun toStringTest() {
        val order = Order(
                id = 1,
                goods = listOf(
                        Goods(id = 1, name = "나나", price = 3000L),
                        Goods(id = 2, name = "다다", price = 4000L)
                )
        )
        println(order.toJsonString())
    }

    class Order(val id: Long, val goods: List<Goods>)

    class Goods(val id: Long, val name: String, val price: Long)
}

```
아래는 order.toJsonString 의 결과이다.
<figure>
	<img src="/images/2021-01-10-tojsonstring-02.png" alt="">
</figure>

toString 을 override 하지않고 Gson 을 사용해 Kotlin Extension 으로 만들어 둔 다음 사용하면 된다.