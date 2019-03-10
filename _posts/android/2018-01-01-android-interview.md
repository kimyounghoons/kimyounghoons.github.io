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

### ViewModel 의 장점
화면 회전시 데이터를 유지할 수 있는 구조로 디자인하였으며 Android Lifecycle 의 onDestroy코드가 동작한다.
Lifecycle 을 내부적으로 알아서 호출해주기 때문에 좋다.

### ConstraintLayout vs RelativeLayout vs LinearLayout 차이(ConstraintLayout 장점)
LinearLayout 은 orientation 이 있어서 가로 또는 세로로 차곡차곡 쌓여지는 방식이다.
RelativeLayout 은 상대적인 기준으로 어떤 위젯의 왼쪽 오른쪽등등의 방향으로 배치 할 수 있게 해주는 레이아웃이다.
ConstraintLayout 은 복잡한 레이아웃을 단순한 계층구조를 이용하여 표현할 수 있는 ViewGroup 이다.
RelativeLayout과 비슷하지만 더 유연하고 다양하고 강력한 기능을 제공한다.

### CustomView 와 Fragment 의 차이점 
Fragment 는 자체 생명주기를 따르고 Activity 의 생명주기에 따라 직접적으로 영향을 받는다. Fragment를 사용하려면 Activity 내에서 또는 상위 프래그먼트내에서 사용이 가능하고 다른 액티비티에서 재사용이 가능하다. 여기서 CustomView 는 자체 생명주기는 가지고 있지 않고 Fragment 보다는 더 작은 단위?를 커스터마이징해서 재사용을 할 수 있다. 

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
통신 프로토콜 또는 통신 규약은 컴퓨터나 원거리 통신 장비 사이에서 메세지를 주고 받는 양식과 규칙의 체계이다. 통신 프로토콜은 신호 체계, 인증 그리고 오류 감지 및 수정기능을 포함할 수 있다. 간단히 말해서 데이터 주고받는 상호간에 미리 약속된 규칙, 규약이다!!

### ViewModel 상속받은 클래스에서 ViewModel 상속을 받지 않는다면 달라지는점은?
viewModel 상속을 받지 않는다면 생명주기에 따른 처리,rotate상황 등등을 추가 해주어야한다는 점이 달라진다.

## Rx관련 질문 : 

### 새로운 스레드를 생성해도 되는데 왜 Schedulers.io() 를 사용하는가? 내부 동작이 어떻게 돌아가는 것인가를 묻는 질문
화면단에서 사용되는 request 가 하나가 아니라 적게는 한자리수 많게는 두세자리수까지 갈수 있는데 이부분에서 Thread를 계속해서 생성,수거 하게 된다면 사용할 때마다 드는 비용을 무시할수 없다. 그렇기 때문에 Thread Pool 이 사용된다. 몇개의 스레드를 생성한뒤 큐에 Task를 넣고 작업하고 있지 않은 Thread에 Task를 할당하는 방식이다. 작업이 끝난 Thread 는 다시 어플리케이션에 결과값을 리턴한다. Thread를 재사용 하기때문에 성능저하를 방지할 수 있다. 하지만 Thread를 너무 많이 만들어 놓게 되면 메모리만 낭비하게 되므로 주의해서 사용해야 한다.

###프로가드
코드 축소와 바이트코드를 최적화하고 미사용 코드 명령을 제거하며 남아 있는 클래스,필드 및 메서드를 짧은 이름으로 난독 처리한다. 난독 처리된 코드는 APK 의 리버스 엔지니어링을 어렵게 만들며 보안에 민감한 기능이 앱에 사용되는 경우 특히 유용하다. 
64K 참조 제한을 해결하기 위한 유용한 도구 이기도 하다.

###코틀린 by lazy
lateinit은 필요할 경우 언제든 초기화가 가능한 Properties 이지만 lazy properties는 생성 후 값을 변경할 수 없는 val 로 되어 있다.
by lazy 정의에 의해서 초기화를 진행하고 val 이므로 값을 교체하는 건 불가능하다.
lazy를 사용하는 경우 기본 synchronized 로 동작한다.

###viewModel rotate 상황에서 파기 왜 안되는지 과정 ?
ViewModelStoreOwner 인터페이스를 가지고 있는 Activity 또는 Fragment 는 viewModelStore를 가지고 있다. rotate 될 때 onDestroy 가 불리게 되는데 
{% highlight ruby %}
      boolean isChangingConfigurations = activity != null && activity.isChangingConfigurations();
        if (this.mViewModelStore != null && !isChangingConfigurations) {
            this.mViewModelStore.clear();
        }
{% endhighlight %}
viewModelStore 가 null 이 아니고 configuration변화가 없을때 clear 를 불러 주기 때문에 rotate상황에서 viewModel이 clear 되지 않는다.

###Dangerous permission


###Multi dex
안드로이드 앱을 구성하는 코드는 컴파일 되어 덱스 파일로 만들어 진다. 하나의 덱스 파일에는 64K 메소드참조만 저장할 수 있다. 큰 규모 앱을 작성하다 보면 앞의 메소드 제한을 훌쩍 넘게 되는데 멀티덱스를 사용하면 덱스 파일을 여러 개로 나누어 이러한 문제를 피할 수 있다. 

###Viewholder 패턴에 대해 설명



###Intent service 란?

###Foreground service 사용?

###Fragment 사용 장점 ? 

###Parcelable serializable 차이와 성능은?

###onSaveInstanceState 와 onRestoreInstanceState