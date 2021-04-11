---
layout: post
title: 함수형 프로그래밍 2-8
description: 함수형 프로그래밍 2-8
modified: 2021-04-11
tags: [Android, Kotlin]
categories: [Android, Kotlin]
---

### 컬렉션

함수형 프로그래밍에서는 불변 자료구조를 사용한다. 불변 자료구조는 객체의 상태 변화를 미연에 방지해서 부수효과를 근본적으로 방지한다.
코틀린에서는 mutable 과 immutable 를 구분해서 사용한다.

#### 리스트
비어 있거나 동일한 타입의 값들을 여러 개 넣을 수 있는 자료구조다.

```java
class ListTest {

    @Test
    fun plusListItem(){
        val list = listOf(1,2,3,4,5)
        val newList = list.plus(6)

        println(list)    //[1, 2, 3, 4, 5]
        println(newList) //[1, 2, 3, 4, 5, 6]
    }

    @Test
    fun addListItem(){
        val list = mutableListOf(1,2,3,4,5)
        println(list) //[1, 2, 3, 4, 5]
        list.add(6)
        println(list) //[1, 2, 3, 4, 5, 6]
    }

}
```
위의 차이점은 plus는 새로운 리스트를 반환 하지만 add는 해당 리스트에 추가로 아이템을 더해 준다.

#### 세트
리스트와 동일한 타입의 값들을 여러 개 넣을 수 있다는 점에서 리스트와 유사하나 중복값이 들어갈 수 없다는 점이 리스트와 다르다.

```java
class SetTest {
    @Test
    fun testSet() {
        val set = setOf("1", "2", "3")
        println(set) //[1, 2, 3]
        val newSet = set.plus("4")
        println(newSet) //[1, 2, 3, 4]

        val mutableSet = mutableSetOf("1", "2", "3")
        println(mutableSet) //[1, 2, 3]
        mutableSet.add("4")
        println(mutableSet) //[1, 2, 3, 4]
        mutableSet.add("4")
        println(mutableSet) //[1, 2, 3, 4]
    }
}
```
4를 두번 더했지만 4는 한번만 들어 간다.

#### 맵
코틀린에서는 키와 값을 가진 자료구조인 Pair 를 제공한다.
맵은 키와 값인 Pair 를 여러개 가진 자료구조이다.

```java
class MapTest {

    @Test
    fun testMap() {
        val map1 = mapOf(1 to "One", 2 to "Two")
        val map2 = map1.plus(3 to "Three")

        println(map1) //{1=One, 2=Two}
        println(map2) //{1=One, 2=Two, 3=Three}

        val mutableMap = mutableMapOf(1 to "One", 2 to "Two")
        println(mutableMap)  //{1=One, 2=Two}
        mutableMap[3] = "Three" // mutableMap.put(3,"Three") 와 같다.
        println(mutableMap)  //{1=One, 2=Two, 3=Three}
    }
}
```

맵도 마찬가지로 plus는 새로운 맵을 반환 하지만 put은 해당 맵에 추가로 아이템을 더해 준다.

