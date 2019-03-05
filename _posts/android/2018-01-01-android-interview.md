---
layout: post
title: 안드로이드 면접 관련 질문 
description: "안드로이드 면접 관련 질문"
modified: 2018-01-01
tags: [android,interview]
categories: [android,interview]
---

## 면접 보면서 느낀점 : 
제대로 알고 있어야 제대로 설명이 가능하다.

머리로는 아는데 말로 잘 안나오는 경우는 제대로 모르고 있는 경우인듯...

## 자바 질문 : 

### abstract 와 interface 의 차이 와 자바는 왜 다중상속이 안되는 것인가?
abstract 와 interface 는 자식 클래스 또는 인터페이스를 사용하는 곳에서 추상메소드를 강제로 사용하게끔 유도하는부분이 공통점이라고 할 수있다.
비슷비슷 하지만 다른점은 자바가 다중상속을 지원하지 않지만 다중인터페이스를 지원한다는 것에 있다. 다중 상속을 지원하지 않는 것은 모호성때문이다. 다중상속을 하게 되면 부모로부터 상속받은 method가 동일하게 되면 누구의 부모로부터 가져와야하는지 애매해진다. 그렇기 때문에 자바에서는 다중상속을 지원하지 않는다. 하지만 왜 인터페이스는 다중인터페이스가 가능할까? 그부분에서는 인터페이스는 어떠한 특정 동작을 보장하기 위해서 존재하기 때문이다.
이러한 부분이 차이점이라고 할 수 있다. 

### 쓰레드풀 설명
JVM은 쓰레드 생성 개수 제한이 없어서 계속해서 쓰레드를 만들게 되면 성능 저하와 메모리 부족문제가 생길 수 있다. 이것을 막기위해 쓰레드풀이라는 쓰레드 관리 방식이 사용된다. 쓰레드를 허용된 갯수 안에서만 사용할 수 있도록 제약하는 방식이다. 어플리케이션에서 사용자로부터 들어온 요청을 작업큐에 넣고 스레드풀은 작업큐에 들어온 Task를 미리 생성해놓은 Tread 들에게 일감을 할당한다.
일을 다 처리한 Thread들은 다시 어플리케이션에 결과값을 리턴하는 방식이다. 이용하는 이유는 즉, 프로그램 성능저하 방지 그리고 다수의 사용자 요청을 처리하기 위해 사용된다.

## 코틀린 질문 : 
### let, apply,run, with 차이점 
let : 이 함수를 호출한 객체를 이어지는 함수 블록의 인자로 전달한다.

{% highlight ruby %}
getPadding().let {
        it 은 패딩 값
        setPadding(it, 0, it, 0)
    }
{% endhighlight %}

apply : 이 함수를 호출한 객체를 이어지는 함수 블록의 리시버로 전달한다.

{% highlight ruby %}
params.apply {
        weight = 1f     params를 리시버로 전달 받았기 때문에 params.weight 이나 params.topMargin 바로사용
        topMargin = 100
    }
{% endhighlight %}

with : 인자로 받은 객체를 이어지는 함수 블록의 리시버로 전달, block 함수의 결과를 반환

{% highlight ruby %}
with(textView) {
        text = "textView!!!"
        gravity = Gravity.CENTER_HORIZONTAL
    }
{% endhighlight %}

run : 인자가 없는 익명 함수처럼 사용하는 형태와 객체에서 호출하는 형태 제공
함수형 인자 block 을 호출하고 결과반환 또는 호출한 객체를 함수형 인자 block의 리시버로 전달하고 그 결과를 반환한다.
{% highlight ruby %}
val AplusB = run {
      val a = 1
      val b = 2
      a + b
  }

textView?.run {  with 와 비슷하지만 textView 의 널체크를 해야 하는경우 사용하면 좋다
        text = "textView!!!"
        gravity = Gravity.CENTER_HORIZONTAL
    }
{% endhighlight %}

### kotlin SAM(Single Abstract Method) 질문
선언부가 Java에 있고 ,Kotlin에서 호출 할 경우 SAM 이 동작
Kotlin 에서 AbstractMethod 에 Lambdas 식 적용하려면 Higher-Order-Functions 이용하면된다.


### 코틀린 사용의 장점
은근 딱 말해보라고 하면 널 안전성과 중복코드가 많이 없어진다 정도? 생각이 잘 안남ㅋㅋㅋ
1. 간결한문법
2. 널 안전성
3. 가변/불변 구분
4. 람다 표현식 지원
5. 스트림 API 지원
6. 완벽한 자바 호환성

## 안드로이드 질문 : 

### PagingLibrary 구현 질문 

### ViewModel 의 장점
화면 회전시 데이터를 유지할 수 있는 구조로 디자인하였으며 Android Lifecycle 의 onDestroy코드가 동작한다.
Lifecycle 을 내부적으로 알아서 호출해주기 때문에 좋다.

### ConstraintLayout vs RelativeLayout vs LinearLayout 차이(ConstraintLayout 장점)
LinearLayout 은 orientation 이 있어서 가로 또는 세로로 차곡차곡 쌓여지는 방식이다.
RelativeLayout 은 상대적인 기준으로 어떤 위젯의 왼쪽 오른쪽등등의 방향으로 배치 할 수 있게 해주는 레이아웃이다.
ConstraintLayout 은 복잡한 레이아웃을 단순한 계층구조를 이용하여 표현할 수 있는 ViewGroup 이다.
RelativeLayout과 비슷하지만 더 유연하고 다양하고 강력한 기능을 제공한다.

### CustomView 와 Fragment 의 차이점 

### Android KTX 란 무엇인가?
안드로이드 프레임워크와 서포트 라이브러리를 모두 지원하여 안드로이드를 위한 코틀린(Kotlin) 코드를 간결하고 편하게 사용할 수 있게 설계된 확장 라이브러리이다.
### mvp 패턴과 mvvm 패턴의 차이
mvp 와 mvvm 의 차이는 presenter와 viewModel 의 차이가 가장 크다. 
presenter 는 view의 interface 를 가지고 있고 view 관련 처리를 요청한다.
하지만 viewModel 은 view 의 참조를 하지않고 Rx를 사용하거나 LiveData를 사용해서 데이터가 변경될때 같이 자동으로 변경될수 있게 구조가 되어있다.

### LiveData 는 인터럽트방식인가 폴링방식인가 
폴링방식은 정해진 시간 또는 순번에 상태를 확인해서 상태 변화가 있는지 없는지 체크 하는 방식이고 
인터럽트방식은 main문을 실행 하는 동중 외부에서 정해져 있는 인터럽트 핀에 신호가 들어오면 MCU는 즉각적으로 하고 있는 동작을 멈추고 인터럽트 서비스 루틴을 실행하는 것이다.

LiveData 는 데이터가 변경된 시점에 Observer 가 실행 되기 때문에 인터럽트 방식이라고 볼 수 있다.

### 옵저버패턴 설명
한객체의 상태가바뀌면 그 객체에 의존하는 다른 객체들한테 연락이 가고 자동으로 내용이 갱신되는 방식으로
일대다(one-to-many) 의존성을 정의한다.

데이터 전달 방식은 2가지가 있다 
주제 객체에서 옵저버로 데이터를 보내는 방식 (푸시방식)
옵저버에서 주제객체의 데이터를 가져가는 방식(풀 방식)

Subject 와 Observer 가 존재하고 Subject에 Observer 를 등록하고 난 뒤 subject 의 데이터가 변경 되면 등록된 Observer 가 호출 되는 방식
### 어댑터패턴 설명
한 클래스의 인터페이스를 클라이언트에서 사용하고자하는 다른 인터페이스로 변환한다.
어댑터를 이용하면 인터페이스 호환성 문제 때문에 같이 쓸 수 없는 클래스들을 연결해서 쓸 수 있다.
### 프로토콜이란?

### mvvm 구조에서 ViewModel 상속을 사용 안하게 된다면?

## Rx관련 질문 : 

### 새로운 스레드를 생성해도 되는데 왜 Schedulers.io() 를 사용하는가? 내부 동작이 어떻게 돌아가는 것인가를 묻는 질문