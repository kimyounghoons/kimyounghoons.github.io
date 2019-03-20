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

### 클래스란?
객체를 만들어 내기 위한 설계도 혹은 틀

### 객체란?
소프트웨어 세계에 구현할 대상

### 인스턴스란?
설계도를 바탕으로 소프트웨어 세계에 구현된 구체적인 실체

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

### Context 란 ? 
자신이 어떤 어플리케이션을 나타내고 있는지 알려주는 ID 역할
ActivityManagerService 에 접근 할수 있도록 하는 통로 역할

### ANR(Application Not Responding) 이란?
메인 스레드가 일정 시간 어떤 TASK 에 잡혀 있으면 발생한다.
시간 소모가 많은 작업은 스레드를 통해 처리해야 함

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

### 프로가드
코드 축소와 바이트코드를 최적화하고 미사용 코드 명령을 제거하며 남아 있는 클래스,필드 및 메서드를 짧은 이름으로 난독 처리한다. 난독 처리된 코드는 APK 의 리버스 엔지니어링을 어렵게 만들며 보안에 민감한 기능이 앱에 사용되는 경우 특히 유용하다. 
64K 참조 제한을 해결하기 위한 유용한 도구 이기도 하다.

### 코틀린 by lazy
lateinit은 필요할 경우 언제든 초기화가 가능한 Properties 이지만 lazy properties는 생성 후 값을 변경할 수 없는 val 로 되어 있다.
by lazy 정의에 의해서 초기화를 진행하고 val 이므로 값을 교체하는 건 불가능하다.
lazy를 사용하는 경우 기본 synchronized 로 동작한다.

### viewModel rotate 상황에서 파기 왜 안되는지 과정 ?
ViewModelStoreOwner 인터페이스를 가지고 있는 Activity 또는 Fragment 는 viewModelStore를 가지고 있다. rotate 될 때 onDestroy 가 불리게 되는데 
{% highlight ruby %}
      boolean isChangingConfigurations = activity != null && activity.isChangingConfigurations();
        if (this.mViewModelStore != null && !isChangingConfigurations) {
            this.mViewModelStore.clear();
        }
{% endhighlight %}
viewModelStore 가 null 이 아니고 configuration변화가 없을때 clear 를 불러 주기 때문에 rotate상황에서 viewModel이 clear 되지 않는다.

### Multi dex
안드로이드 앱을 구성하는 코드는 컴파일 되어 덱스 파일로 만들어 진다. 하나의 덱스 파일에는 64K 메소드참조만 저장할 수 있다. 큰 규모 앱을 작성하다 보면 앞의 메소드 제한을 훌쩍 넘게 되는데 멀티덱스를 사용하면 덱스 파일을 여러 개로 나누어 이러한 문제를 피할 수 있다. 

### 액티비티 생명 주기
onCreate 
onStart
onResume
Running
onPause
onStop
onDestroy


### 프래그먼트 생명 주기
onAttach
onCreate
onCreateView
onActivityCreated
onStart
onResume
Running
onPause
onStop
onDestroyView
onDestroy
onDetach

### 서비스 생명 주기 2가지
onCreate
onStartCommand()
return start_sticky , start_not_sticky, start_redeliver_intent
Service Running
onDestroy

onCreate
onBind
onUnbind
onDestroy

### FLAG_ACTIVITY_CLEAR_TOP 과 FLAG_ACTIVITY_SINGLE_TOP 차이
CLEARTOP 은 A -> B -> A 일 때 최상위 A 만 남고 나머지 다 destroy 
SINGLETOP 은 A -> B -> B 일때 A -> B 로 B가 하나로 됨.  

### 스레드 통신방법?
스레드가 시작 되면 이 스레드는  루퍼! 핸들러! 메세지 큐! 를 하나씩 가지고 있다.
메세지큐는 외부 스레드로부터 핸들러를 통하여 받은 message 혹은 Task 를 저장하는 역할을 한다.
루퍼는 메세지큐에서 메세지를 순차적으로 꺼내서 핸들러에게 전달하는 역할!!
핸들러는 두가지 기능이 있다.
1. 루퍼에게서 받은 메세지 혹은 Task 를 일정한 시간에 수행하는 기능을 한다.
2. 외부 스레드로부터 받은 메세지를 핸들러를 통하여 Message Queue에 집어 넣는 역할을 한다.

### 스레드 충돌을 막으려면?
두개 이상의 스레드가 같이 참조 하는 메소드 혹은 블럭구문을 synchronized , synchronized block 사용해서 해결할 수 있다.

### Viewholder 패턴에 대해 설명
예전 ListView 를 사용할 때 getView() 를 오버라이드 해서 뷰를 인플레이팅 시키는데 이부분에서 계속해서 생성하고 findviewById 를 불러주게 되면 부담이 많이 가는 작업이 된다. 데이터가 많아 질 수록 버벅거리는 현상이 일어날 확률이 높다. 그래서 viewHolder 패턴이 나오게 되었는데 해당 뷰가 널일때만 view를 인플레이트 시키고 viewHolder 클래스에 findviewById 를 사용해서 viewHolder 는 해당 뷰에 태깅처리해서 다음번 부터는 태그한 viewHolder 를 가져와서 사용한다. 그 이후에 viewHolder 를 강제로 구현하게 나온것이 RecyclerView 이다.

### Dangerous permission
시스템 권한은 두가지로 나눠 지는데 정상권한과 위험권한이 있다.
정상권한은 사용자 개인정보를 직접 위험에 빠뜨리지않는다. 앱이 매니페스트에 정산 권한을 나열하는 경우 시스템은 자동으로 권한을 부여.
위험권한은 사용자 기밀 데이터에 대한 액세스를 앱에 부여할 수 있다. 위험권한을 나열하는 경우, 사용자는 앱에 대한 명시적 승인을 제공해야 한다.

### Intent service 란?
IntentService 는 Intent를 사용해서 시작되고 하나의 workerThread 가 생성 되고 Queue에 작업이 들어 가게 된다. onHandleIntent() 메소드가 이 스레드 내에서 호출되고 작업이 다 끝나면 알아서 destroy 되는 방식이라 stopSelf를 불러줄 필요가 없다.

### Foreground service 사용?


### Fragment 사용 장점 ? 
Activity를 분할하여 화면의 한 부분을 정의할수 있고 자신의 생명주기를 가진다. 액티비티내에서 실행 중 추가 제거가 가능하고 다른 액티비티에서도 사용 할 수 있어 재사용성이 뛰어나다. 태블릿 지원하게 될 때 용이하게 사용 가능하다. 

### Parcelable serializable 차이와 성능은?
복잡한 클래스의 객체를 이동시키려고 할 때 Serializable 또는 Parcelable 을 사용해서 직렬화하여 인텐트에 추가합니다.
Serializable 은 Java 의 인터페이스이다. 해당 객체에 인터페이스 Serializable 을 사용해주면 되기 때문에 사용하기 쉽다. 하지만 내부에서 Reflection 을 사용하여 직렬화 처리하기 때문에 성능 저하 및 배터리 소모가 발생되게 된다.
Parcelable 은 Android SDK 의 인터페이스이다. 
직렬화 방법을 사용자가 명시적으로 작성하기 때문에 작동으로 처리하기 위한 reflection이 필요없다.
속도적인 측면에서는 Parcelable 이 10배 이상 빠르다.

하지만 Serializable 을 사용할 때 writeObject 와 readObject 를 구현해 주면 Parcelable 보다 쓰기는 속도가 3배 읽기의 경우 1.6배 더 빠르다.
어떻게 사용하느냐에 따라 속도적인 측면에서 비교할 수 있다.

### onSaveInstanceState 와 onRestoreInstanceState
Activity 또는 Fragment 가 종료 될 때 onSaveInstanceState은 onPause 다음 상태에서 불리게 된다. 이때 파라미터로 받은 Bundle에 데이터를 저장하고 onCreate 시점에서 savedInstanceState bundle 을 통해 값을 가져와서 사용할 수 있다. onRestoreInstanceState 은 정상적인 경우는 불리지 않고 메모리 부족한 경우 프로세스 자체에서 Activity 또는 Fragment 를 강제종료할 때 onRestoreInstanceState가 불리게 된다. bundle 통해서 데이터 백업 가능.