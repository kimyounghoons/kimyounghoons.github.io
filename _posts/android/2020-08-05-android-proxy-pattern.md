---
layout: post
title:  Proxy Pattern (디자인 패턴 13장)
description: "Proxy Pattern (디자인 패턴 13장)"
modified: 2020-08-05
tags: [Proxy Pattern]
categories: [java,kotlin]
---

### 프록시 패턴 정의
어떤 객체에 대한 접근을 제어하기 위한 용도로 대리인이나 대변인에 해당하는 객체를 제공하는 패턴이다.

<figure>
	<img src="/images/2020-09-02-android-proxy-pattern.png" alt="">
</figure>

### 프록시 패턴 언제 사용할까!?
원격 프록시 , 가상 프록시 , 보호 프록시 여러가지가 있는데 가상 프록시를 다뤄보자.  
네트워크를 통해 이미지를 가져온다고 가정할 때 네트워크 상태와 인터넷 연결 속도에 따라서 시간 소요 되게 된다.  
이때 이미지를 불러 오는 동안 Proxy를 사용해서 임의의 이미지를 보여준 다음 실제 이미지를 보여줄 수 있다.  
아래 코드를 보자.   

```java
class ProxyPattern {
    @Test
    fun test() {
        ImageProxy().showIcon()
    }
}

interface Icon {
    fun showIcon()
}

class ImageIcon : Icon {

    override fun showIcon() {
        download()
        println("ImageIcon 그림")
    }

    private fun download() {
        println("ImageIcon 다운로드")
    }

}

class ImageProxy : Icon {
    var imageIcon: ImageIcon? = null

    override fun showIcon() {
        println("ImageProxy 그림")
        imageIcon = ImageIcon()
        imageIcon?.showIcon()
    }
}
```

ImageProxy 를 통해 showIcon 메소드를 호출 하게 되면 프록시 임의의 그림을 보여주고 다운로드가 되었을때 실제 이미지 ImageIcon 그림을 보여줄 수 있다.


아래는 테스트 결과이다.
<figure>
	<img src="/images/2020-09-02-android-proxy-pattern-2.png" alt="">
</figure>