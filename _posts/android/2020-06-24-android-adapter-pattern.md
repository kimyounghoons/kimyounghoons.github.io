---
layout: post
title:  Adapter Pattern (디자인 패턴 8장)
description: "Adapter Pattern (디자인 패턴 8장)"
modified: 2020-06-24
tags: [Adapter Pattern]
categories: [java,kotlin]
---

### 어댑터 패턴 정의

한 클래스의 인터페이스를 클라이언트에서 사용하고자 하는 다른 인터페이스로 변환합니다.  
어댑터를 이용하면 인터페이스 호환성 문제 때문에 같이 쓸 수 없는 클래스들을 연결해서 쓸 수 있습니다.  

### 어댑터 패턴 언제 사용할까!?
예를 들어 기존 NormalPager 를 사용하고 있는데 LibPager 를 같이 사용해야 하는 상황이 생겼다고 가정 함.  
근데 LibPageListener 는 PageListener 와 다르게 같은 기능인데 다른 메소드명을 가지고 있다.  
그리고 LibPageListener 에는 없는 기능(onDoubleNextPage,onDoublePreviousPage)이 PageListener에는 존재 한다.  
LibPagerAdapter 를 통해서 해결해보자!!  

아래의 코드를 보자.  
```java

class AdapterPattern {

    interface PageListener {
        fun onPreviousPage()
        fun onDoublePreviousPage()
        fun onNextPage()
        fun onDoubleNextPage()
    }

    class NormalPager : PageListener {

        override fun onPreviousPage() {
            println("NormalPager onPreviousPage")
        }

        override fun onDoublePreviousPage() {
            println("NormalPager onPreviousPage")
            println("NormalPager onPreviousPage")
        }

        override fun onNextPage() {
            println("NormalPager onNextPage")
        }

        override fun onDoubleNextPage() {
            println("NormalPager onNextPage")
            println("NormalPager onNextPage")
        }
    }

    class LibPager : LibPageListener {
        override fun onPrevious() {
            println("LibPager onPrevious")
        }

        override fun onNext() {
            println("LibPager onNext")
        }

    }

    interface LibPageListener {
        fun onPrevious()
        fun onNext()
    }

    class LibPagerAdapter(private val libPageListener: LibPageListener) : PageListener {

        override fun onPreviousPage() {
            libPageListener.onPrevious()
        }

        override fun onDoublePreviousPage() {
            libPageListener.onPrevious()
            libPageListener.onPrevious()
        }

        override fun onNextPage() {
            libPageListener.onNext()
        }

        override fun onDoubleNextPage() {
            libPageListener.onNext()
            libPageListener.onNext()
        }

    }

	@Test
    fun test() {
        val normalPager = NormalPager()
        normalPager.onNextPage()
        normalPager.onPreviousPage()
        normalPager.onDoubleNextPage()
        normalPager.onDoublePreviousPage()

        val libPager = LibPager()
        val libPagerAdapter = LibPagerAdapter(libPager)
        libPagerAdapter.onNextPage()
        libPagerAdapter.onPreviousPage()
        libPagerAdapter.onDoubleNextPage()
        libPagerAdapter.onDoublePreviousPage()
    }

}

```
위의 코드를 보면 onDoublePreviousPage,onDoubleNextPage 는 LibPageListener 없지만 같은 기능을 하게 만들어 주었다.

아래의 결과를 보면 동일하다.

<figure>
	<img src="/images/2020-06-24-android-adapter-pattern.png" alt="">
</figure>