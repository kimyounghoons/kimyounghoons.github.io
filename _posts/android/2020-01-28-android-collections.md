---
layout: post
title: 코틀린 람다 콜렉션(kotlin lambda collection)
description: "코틀린 람다 콜렉션 사용(kotlin lambda collection)"
modified: 2020-01-28
tags: [intent,kotlin lambda collection]
categories: [android]
---

### TODO : 선택된 아이템 알파벳들을 콤마로 구분해서 String 으로 만들기

사용 된 함수는 filterIsInstance , filter , map , joinToString  
선언형 프로그래밍 -> 함수형 프로그래밍 방식으로 변경 하는 과정을 코딩  

```
/**
 * Created by kimyounghoon on 2020-01-29.
 */
class KotlinLambdaCollectionTest {
    open class BaseItem(var id: String)

    class Item(id: String, val alphabet: String, val isSelected: Boolean) : BaseItem(id)

    lateinit var items: ArrayList<BaseItem>

    @Before
    fun before() {  //테스트 시작 되기 전에 불린다. 리스트 셋팅!!
        items =
            arrayListOf(
                Item("1", "a", false),
                Item("2", "b", true),
                Item("3", "c", true),
                Item("4", "d", true),
                Item("5", "e", false),
                Item("6", "f", false),
                Item("7", "g", false)
            )
    }


   /*
    *  test1 은 명령형 프로그래밍 2,3,4 함수형 프로그래밍으로 변환하는 작업입니다.
    * */
    @Test
    fun test1() {
        var alphabets = ""                  
        val filterItems = arrayListOf<Item>()
        for (item in items) {
            if (item is Item && item.isSelected) {
                filterItems.add(item)
            }
        }

        for (index in 0 until filterItems.size) {
            if (index == filterItems.size - 1) {
                alphabets += filterItems[index].alphabet
            } else {
                alphabets += filterItems[index].alphabet + ","
            }
        }

        Assert.assertEquals("b,c,d", alphabets)
    }

    /*
    *  filter 를 사용해서 Item 으로만 걸렀기 때문에 as List<Item> 으로 캐스팅. 뭔가 아쉽다.
    * */
    @Test
    fun test2() {
        var alphabets = ""
        (items.filter {
            (it is Item && it.isSelected)
        } as List<Item>).let {
            for (index in 0 until it.size) {
                if (index == it.size - 1) {
                    alphabets += it[index].alphabet
                } else {
                    alphabets += it[index].alphabet + ","
                }
            }
        }

        Assert.assertEquals("b,c,d", alphabets)
    }

    /*
    * filter 와 map , joinToString 을 사용하니 함수형 프로그래밍으로 바뀌고
    * 가독성도 많이 좋아졌는데 map 에서 한번더 캐스팅 해서 써야하는게 불편해서 구글링 한번더!!
    * */
    @Test
    fun test3() {
        val alphabets = items.filter {
            (it is Item && it.isSelected)
        }.map {
            (it as Item).alphabet
        }.joinToString(",")

        Assert.assertEquals("b,c,d", alphabets)
    }

    /*
    * filterIsInstance 를 사용하면 Item 만 필터 되어서 Item 형태로 반환 되기때문에 캐스팅이 필요 없다.
    * */
    @Test
    fun test4() {
        val alphabets = items.filterIsInstance<Item>().filter {
            it.isSelected
        }.map {
            it.alphabet
        }.joinToString(",")

        Assert.assertEquals("b,c,d", alphabets)
    }
}
```

test1 과 test4 를 비교 했을때 코드 양도 많이 줄고 훨씬 가독성이 많이 올라간걸 볼 수 있다.