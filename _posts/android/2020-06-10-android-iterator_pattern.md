---
layout: post
title:  Iterator Pattern (디자인 패턴 7장)
description: "Iterator Pattern (디자인 패턴 7장)"
modified: 2020-06-10
tags: [Iterator Pattern]
categories: [java,kotlin]
---

### 이터레이터 패턴 정의
컬렉션 구현 방법을 노출시키지 않으면서도 그 집합체 안에 들어있는 모든 항목에 접글할 수 있는 방법을 제공한다.  

컬렉션 종류가 다양하고 항목을 접근하는 방식도 조금씩 다르기 때문에 대응하기가 까다롭거나 코드가 지저분해질 수 있다.  
Iterator 패턴을 사용해서 해결해보자.  
CustomDialogView 라는 임의의 클래스를 만들어서 iterator 매개변수로 받는 생성자를 하나 만들고 printAll 이라는 함수를 만들어서 
모든 항목에 접근해 print 하도록 만들었다.  

아래의 코드를 보자.  
```java
class IteratorPatternTest {
    @Test
    fun test() {
        val array = arrayOf("first", "second", "third")
        var listDialog = CustomDialogView(array.iterator())
        listDialog.printAll()

        val arrayList = arrayListOf(1, 2, 3)
        listDialog = CustomDialogView(arrayList.iterator())
        listDialog.printAll()

        val hashMap = hashMapOf(
            Pair(1, "one"),
            Pair(2, "two"),
            Pair(3, "three")
        )
        listDialog = CustomDialogView(hashMap.iterator())
        listDialog.printAll()
    }

    class CustomDialogView(private val iterator: Iterator<Any>) {
        fun printAll() {
            while (iterator.hasNext()) {
                val any = iterator.next()
                if (any is Map.Entry<*, *>) {
                    println(any.value)
                } else {
                    println(any)
                }
            }
        }
    }
}
```
hasNext 는 더 꺼낼 항목 여부에 따라 boolean 을 return 하고 next 는 해당 항목을 return 한다.
array, arrayList, hashMap 3가지를 사용하지만 iterator 패턴을 통해 쉽게 구현할 수 있다.

아래 사진을 보면 모두 이상 없이 프린트 된 결과를 볼 수 있다.
<figure>
	<img src="/images/2020-06-10-android-iterator_pattern.png" alt="">
</figure>