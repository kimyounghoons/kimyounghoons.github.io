---
layout: post
title: Center Loading Progress
description: "Center Loading Progress"
modified: 2020-01-10
tags: [Center Loading Progress]
categories: [android]
---
API 호출 할 때 다른 터치를 막기 위해 센터 프로그레스를 보여주는 경우 여러 가지 방법이 있지만 그중 progressDialog 를 사용해보자~!  

BaseActivity 에서 progressDialog 를 가지고 있고 필요할 때 showProgress() , hideProgress() 를 통해 보여주거나 감출수 있다. 

```
abstract class BaseActivity : AppCompatActivity(){
    private var progressDialog: AppCompatDialog? = null

    fun showProgress() {
        if (this == null || isFinishing) {
            return
        }

        progressDialog?.apply {
            dismiss()
        }

        progressDialog = AppCompatDialog(this)
        progressDialog?.apply {
            setCancelable(false)
            window?.setBackgroundDrawable(ColorDrawable(Color.TRANSPARENT))
            setContentView(R.layout.progress_center_loading)
            show()
        }
    }

    fun hideProgress() {
        progressDialog?.dismiss()
    }
}
```

아래는 layout 코드

```
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:clickable="true">

    <ProgressBar
        style="?android:attr/indeterminateDrawable"
        android:layout_width="34dp"
        android:layout_height="34dp"
        android:indeterminate="true"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />
</androidx.constraintlayout.widget.ConstraintLayout>
```

이 경우 장점은 BaseActivity 를 상속 받는 Activity 의 경우 showProgress , hideProgress 를 통해 간편하게 사용할 수 있다.
단점은 필요 하지 않은 Activity 에서도 progressDialog 변수를 가지고 있는다.