---
layout: post
title:  Template Pattern (디자인 패턴 10장)
description: "Template Pattern (디자인 패턴 10장)"
modified: 2020-07-15
tags: [Template Method Pattern]
categories: [java,kotlin]
---

### 템플릿 메소드 패턴 정의
메소드에서 알고리즘의 골격을 정의한다.  
알고리즘의 여러 단계 중 일부는 서브클래스에서 구현할 수 있습니다.  
템플릿 메소드를 이용하면 알고리즘의 구조는 그대로 유지하면서 서브클래스에서 특정 단계를 재정의할 수 있습니다.  
  
#### 헐리우드 원칙(디자인 원칙)
먼저 연락하지 마세요. 저희가 연락 드리겠습니다.
-> 의존성 부패를 방지 할 수 있다.

#### 의존성 부패란?
고수준 구성요소가 저수준 구성요소에 의존하고 그 저수준 구성요소는 다시 고수준 구성요소에 의존하게 된다면 서로에게 의존성이 있다는걸 알 수 있다.  
이런 현상을 의존성 부패라고 한다.  

### 템플릿 패턴 언제 사용할까!?
CustomActionBar  
왼쪽 뒤로가기 메뉴 버튼  
타이틀  
흰색 배경  

SecondCustomActionBar  
왼쪽 뒤로가기 메뉴 버튼  
타이틀  
분홍색 배경  
오른쪽 즐겨찾기 버튼(CustomActionBar 에는 없음)  

두개의 CustomActionBar 가 있다고 가정하고 공통된 부분을 찾아보자.  
왼쪽 뒤로가기 메뉴 버튼은 완전 동일 하고 타이틀과 배경을 셋팅 하는 부분은 타이틀과 배경색은 다르지만 셋팅하는 부분은 공통된 부분이다.  
그리고 오른쪽 즐겨찾기 버튼은 CustomActionBar 에는 존재하지 않는다. 이부분은 hook를 통해서 처리 한다.  

아래의 코드를 보자.  

```java
class TemplateMethodPattern {
    @Test
    fun test() {
        val customActionBar = CustomActionBar()
        customActionBar.prepare()

        println("----------------------------------------------")

        val secondActionBar = SecondCustomActionBar()
        secondActionBar.prepare()
    }

    abstract class ActionBar {
        fun prepare() {
            initTitle()
            initBackButtonView()
            setBackground()
            hook()
        }

        abstract fun initTitle()

        private fun initBackButtonView() {
            println("뒤로가기 버튼 셋팅")
        }

        abstract fun setBackground()

        open fun hook() {

        }
    }

    class CustomActionBar : ActionBar() {
        //왼쪽 뒤로가기 메뉴 버튼
        //타이틀 셋팅
        //흰색 배경
        override fun initTitle() {
            println("Title: CustomActionBar")
        }

        override fun setBackground() {
            println("흰색 배경")
        }
    }

    class SecondCustomActionBar : ActionBar() {
        //왼쪽 뒤로가기 메뉴 버튼
        //타이틀 셋팅
        //분홍색 배경
        //오른쪽 즐겨찾기 버튼(CustomActionBar 에는 없음)

        override fun initTitle() {
            println("Title : SecondCustomActionBar")
        }

        override fun setBackground() {
            println("분홍색 배경")
        }

        override fun hook() {
            println("오른쪽 즐겨찾기 버튼 셋팅")
        }
    }

}
```
아래는 테스트 결과이다.

<figure>
	<img src="/images/2020-07-15-android-template-pattern.png" alt="">
</figure>