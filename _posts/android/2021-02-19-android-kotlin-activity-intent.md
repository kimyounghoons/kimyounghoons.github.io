---
layout: post
title: 액티비티 이동 및 데이터 전달
description: 액티비티 이동 및 데이터 전달
modified: 2021-02-19
tags: [Android, Kotlin]
categories: [Android, Kotlin]
---

#### A 액티비티 → B 액티비티 이동

Intent 에 key value 1:1 형식으로 데이터를 담아 액티비티를 시작한다.

```java
//A 액티비티에서의 호출
Intent intent = new Intent(activity, B.class);
        intent.putExtra(키 값, 넘겨줄 내용);
        intent.putExtra(키 값, 넘겨줄 내용);
        activity.startActivity(intent);
        ActivityAnimation.activityAnimateEnter(activity); //액티비티 전환시 애니메이션

//B 액티비티에서의 데이터 수신
getIntent().getIntExtra(키 값, 0L)
getIntent().getBooleanExtra(키 값, false)
```

#### 개선할 점
1. 기존 방식은 키 값을 B액티비티 외부에서도 알고 있어야 한다.
2. 새로운 키 값의 정의가 필요하다.
3. 키 값을 다른 키값으로 사용하지는 않았는지 확인이 필요하다.
4. 같은 키값을 사용 했어도 동일한 키값에 대해 Boolean 으로 넘겨 줬는데 Long 으로 받는 경우 데이터를 제대로 전달 받지 못할 수 있다.

이런 실수를 방지 하기 위해 새로운 방식을 도입하려고 한다.

#### 새로운 방식
A 액티비티 → B 액티비티 이동

공통으로 필요한 부분을 위해 BaseArguments Abstract Class 를 준비했다.

```java
abstract class BaseArguments : Parcelable {

    fun startActivityEnter(activity: Activity, destinationClass: Class<*>) {
        Intent(activity, destinationClass).apply {
            putExtra(KEY_ARGUMENTS, this@BaseArguments)
        }.let {
            activity.startActivity(it)
            enterAnimation(activity)
        }
    }

    fun startActivityEnterForResult(activity: Activity, destinationClass: Class<*>, requestCode: Int) {
        Intent(activity, destinationClass).apply {
            putExtra(KEY_ARGUMENTS, this@BaseArguments)
        }.let {
            activity.startActivityForResult(it, requestCode)
            enterAnimation(activity)
        }
    }

    //reified는 자바 코드에서는 사용 못함
    inline fun <reified destinationActivity : Activity> startActivityEnter(activity: Activity) {
        Intent(activity, destinationActivity::class.java).apply {
            putExtra(KEY_ARGUMENTS, this@BaseArguments)
        }.let {
            activity.startActivity(it)
            enterAnimation(activity)
        }
    }

    //reified는 자바 코드에서는 사용 못함
    inline fun <reified destinationActivity : Activity> startActivityEnter(activity: Activity, requestCode: Int) {
        Intent(activity, destinationActivity::class.java).apply {
            putExtra(KEY_ARGUMENTS, this@BaseArguments)
        }.let {
            activity.startActivityForResult(it, requestCode)
            enterAnimation(activity)
        }
    }

    //enterAnimation 커스텀 사용 가능
    //진입시 다른 애니메이션 원하는 경우 오버라이드 해서 사용
    open fun enterAnimation(activity: Activity) {
        //진입 애니메이션 구현
    }

    companion object {
        const val KEY_ARGUMENTS = "KEY_ARGUMENTS"

        @JvmStatic
        fun createFromIntent(intent: Intent): BaseArguments {
            return intent.getParcelableExtra(KEY_ARGUMENTS)!!
        }
    }
}
```

#### 코틀린 코드 사용 방법
```java
class B {//B 액티비티 안에 Arguments 클래스를 생성
  private lateinit var arguments: Arguments

  @Parcelize
  class Arguments(
        val value: Boolean
  ) : BaseArguments()

  override fun onCreate(savedInstanceState: Bundle?) {
        arguments = BaseArguments.createFromIntent(intent) as Arguments
        arguments.value //넘겨준 데이터 접근
  }
}

A 액티비티에서의 호출
B.Arguments(true).startActivityEnter(this, B.class);
```

#### 자바 코드 사용 방법
```java
class B {
    public static class Arguments extends BaseArguments {
        private final long value;

        public Arguments(long value) {
            this.value = value;
        }

        protected Arguments(Parcel in) {
           this.value = in.readLong();
        }

    public static final Creator<Arguments> CREATOR = new Creator<Arguments>() {
        @Override
        public Arguments createFromParcel(Parcel source) {
            return new Arguments(source);
        }

        @Override
        public Arguments[] newArray(int size) {
            return new Arguments[size];
        }
    };

    @Override
    public int describeContents() {
        return 0;
    }

    @Override
    public void writeToParcel(Parcel dest, int i) {
        dest.writeLong(this.value);
    }

  }

    @Override
    public void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        arguments = (Arguments) BaseArguments.createFromIntent(getIntent());
    }
}

A 액티비티에서의 호출
new B.Arguments(true).startActivityEnter(this, B.class);
```

#### 개선된 점
1. 외부에서 키 값을 알고 있지 않아도 된다.
2. 새로운 키값에 대한 정의가 필요없다.
3. 키 값을 다른 키값으로 사용하지는 않았는지 확인이 필요 없다.
4. 데이터를 넘겨 받는 순간 Type을 알기 때문에 데이터 보장이 된다.


#### 아쉬운점
1. 액티비티 생성 시 1:1 로 Arguments 클래스를 생성해야한다.
2. 코틀린에서는 Parcelize 어노테이션을 사용하면 편하지만 자바에서 Parcelable 을 사용시 선언이 번거롭다.