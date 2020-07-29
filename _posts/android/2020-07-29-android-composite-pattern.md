---
layout: post
title:  Composite Pattern (디자인 패턴 11장)
description: "Composite Pattern (디자인 패턴 11장)"
modified: 2020-07-15
tags: [Composite Pattern]
categories: [java,kotlin]
---

### 컴포지트 패턴 정의
객체들을 트리 구조로 구성하여 부분과 전체를 나타내는 계층구조로 만들 수 있습니다.  
이 패턴을 이용하면 클라이언트에서 개별 객체와 다른 객체들로 구성된 복합 객체를 똑같은 방법으로 다룰 수 있다.  

<figure>
	<img src="/images/2020-07-29-android-composite-pattern-02.png" alt="">
</figure>


### 컴포지트 패턴 언제 사용할까!?
식당에 메뉴가 있는데 아침 메뉴 점심 메뉴 저녁메뉴가 있을 수 있고 또 다른 디저트 메뉴 혹은 디저트 메뉴 안에서도 또 다른 메뉴로 나뉘어 질 수 있다.  
이런 식으로 계층 구조로 생각 해야 하는 경우 사용하면 유용하다.  

아래의 코드를 보자.  

```java

class CompositePattern {
    @Test
    fun test() {
        val allMenus = Menu("전체메뉴")

        val breakfastMenu = Menu("아침메뉴").apply {
            menuComponents.apply {
                add(MenuItem("토스트", 3000))
                add(MenuItem("삼겹살", 10000))
                add(MenuItem("김밥", 2500))
            }
        }

        val lunchMenu = Menu("점심메뉴").apply {
            menuComponents.apply {
                add(MenuItem("ㅄㅎㄱ", 10000))
                add(MenuItem("ㅍㅇㅈ", 3500))
            }
        }

        allMenus.menuComponents.apply {
            add(breakfastMenu)
            add(lunchMenu)
        }

        allMenus.print()

    }

    //Component
    abstract class MenuComponent {
        abstract fun print()
    }

    //Leaf
    class MenuItem(private val name: String, private val price: Int) : MenuComponent() {
        override fun print() {
            println("name : $name , price : $price")
        }
    }

    //Composite
    class Menu(private val name: String) : MenuComponent() {
        val menuComponents = arrayListOf<MenuComponent>()

        override fun print() {
            println("name : $name")
            val iterator = menuComponents.iterator()
            while (iterator.hasNext()) {
                val menuComponent = iterator.next()
                menuComponent.print()
            }
        }
    }
}

```
Leaf 에서는 더 이상 메뉴를 추가,제거 할 수 없다.  
Composite 에서만 가능

아래는 테스트 결과이다.
<figure>
	<img src="/images/2020-07-29-android-composite-pattern.png" alt="">
</figure>


[사진 자료 출처]
(https://ko.wikipedia.org/wiki/%EC%BB%B4%ED%8F%AC%EC%A7%80%ED%8A%B8_%ED%8C%A8%ED%84%B4#:~:text=%EC%BB%B4%ED%8F%AC%EC%A7%80%ED%8A%B8%20%ED%8C%A8%ED%84%B4(Composite%20pattern)%EC%9D%B4%EB%9E%80,%EB%8F%99%EC%9D%BC%ED%95%98%EA%B2%8C%20%EB%8B%A4%EB%A3%A8%EB%8F%84%EB%A1%9D%20%ED%95%9C%EB%8B%A4.)
