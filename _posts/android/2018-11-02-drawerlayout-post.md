---
layout: post
title: drawerlayout navigationView 클릭 먹통
description: "drawerlayout navigationView 클릭 먹통"
modified: 2018-11-02
tags: [android]
categories: [android]
---

딤처리 된 팝업을 추가 할 일이 생겨 아래의 구조로 작성함

DrawerLayout 사용하는 layout 에서의 구조 

{% highlight ruby %}

<DrawerLayout>
    <ConstraintLayout> 
        메인 뷰
    </ConstraintLayout>

    <NavigationView>  
    </NavigationView>

    <ConstraintLayout>  
        딤처리 팝업
    </ConstraintLayout>
</DrawerLayout>

{% endhighlight %}

이런식으로 구성 했다.

근데 딤처리 팝업 뜨고 나서만 NavigationView 에 있는 뷰들이 클릭을 먹지 않음...

딤처리 팝업을 확인 누르면 VISIBLE -> GONE 상태로 변경되서 상관이 없어야 할텐데 클릭이 되지 않는 문제..  

처음에는 문제 초점을 딤처리 팝업에서의 setClickable 에 뒀다.

하지만 효과가 없고 구글링해서 찾아보니 DrawerLayout안에 NavigationView의 위치는 맨 마지막으로 와야한다고 나와있었다.

{% highlight ruby %}

<DrawerLayout>
    <ConstraintLayout>  
        메인 뷰
    </ConstraintLayout>

    <ConstraintLayout>  
        딤처리 팝업
    </ConstraintLayout>

    <NavigationView>  
    </NavigationView>

</DrawerLayout>

{% endhighlight %}

그렇게 처리하니 이슈가 사라짐!! 
<div class="redText">
기억하자 NavigationView의 위치는 맨 마지막에 와야함!!
</div>