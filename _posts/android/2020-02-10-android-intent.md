---
layout: post
title: 인텐트 사용해서 다른 액티비티로 데이터 넘기기
description: "인텐트 사용해서 다른 액티비티로 데이터 넘기기"
modified: 2020-02-10
tags: [intent]
categories: [android]
---

StartActivity 에서 DestActivity 로 String 데이터를 넘긴다고 가정하면 다음과같이 일반적으로 사용한다.

### StartActivity 코드  
``` 
val stringValue = "스트링 값"
Intent intent = new Intent(this, DestActivity.class)
            intent.putExtra(DestActivity.KEY_VALUE, stringValue)
            startActivity(intent)
```

### DestActivity 코드
```
companion object {
    const val KEY_VALUE = "KEY_VALUE"
}

override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    val stringValue = intent.getStringExtra(KEY_VALUE) ?: "",
}
```
Key값은 보통 DestActivity 에서 Static value 로 가지고 있고 StartActivity 에서 DestActivity 의 키 값을 사용한다.

또 다른 방법은 DestActivity 에서 innerClass 로 Arguments 클래스를 가지고 있는 방법이다.
```
class DestActivity{
  class Arguments(val stringValue: String) {
        fun startActivity(context: Context) {
            val intent = Intent(context, DestActivity::class.java)
            intent.putExtra(KEY_VALUE, stringValue)
            context.startActivity(intent)
        }

        companion object {
            private const val KEY_VALUE = "KEY_VALUE"

            fun createFromIntent(intent: Intent): Arguments {
                return Arguments(intent.getStringExtra(KEY_VALUE) ?: "")
            }
        }
    }
}
```

## Arguments 사용법은 다음과 같다.

### StartActivity 에서의 코드
```
DestActivity.Arguments(stringValue)
            .startActivity(this@BuyingBuildingsActivity)
```
### DestActivity 코드
```
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    val binding = DataBindingUtil.setContentView(this, R.layout.activity_dest)
    val arguments = Arguments.createFromIntent(intent)
    binding.arguments = arguments
}
```

### 데이터 바인딩 (activity_dest.xml)
```
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:app="http://schemas.android.com/apk/res-auto">
    <data>
        <variable
            name="arguments"
            type="com.example.test.DestActivity.Arguments" />
    </data>
    <androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical">
        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@{arguments.stringValue}"
            app:layout_constraintBottom_toBottomOf="parent"
            app:layout_constraintLeft_toLeftOf="parent"
            app:layout_constraintRight_toRightOf="parent"
            app:layout_constraintTop_toTopOf="parent" />
    </androidx.constraintlayout.widget.ConstraintLayout>
</layout>
```

두번째 방법의 장점은 크게 3가지이다.
1. 외부에서 DestActivity 에서의 키를 사용하지 않아도 된다.  
Arguments 클래스 내부에서 키 값 관리 하면 됨.
2. 데이터 바인딩을 사용한다면 Arguments 를 레이아웃에서 바로 사용할 수 있다.
3. intent 를 통해서 가져온 값은 Nullable 타입 이지만 Arguments 를 통해 가져온 변수는 NonNull 타입으로 변환해서 가져올 수 있다.