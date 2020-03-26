---
layout: post
title:  Observer Pattern (디자인 패턴 2장)
description: "Observer Pattern (디자인 패턴 2장)"
modified: 2020-03-25
tags: [Observer Pattern]
categories: [java,kotlin]
---

#### 정의  
한 객체의 상태가 바뀌면 그 객체에 의존하는 다른 객체들한테 연락이 가고 자동으로 내용이 갱신되는 방식으로 일대다(one-to-many) 의존성을 정의함.

#### 디자인 원칙  
서로 상호작용을 하는 객체 사이에서는 가능하면 느슨하게 결합하는 디자인을 사용해야 한다.

<figure>
	<img src="/images/2020-03-25-android-observer-pattern.png" alt="">
</figure>

위의 사진은 위키백과에서 가져온 이미지이다.  
Subject 에 Observer를 등록 시키고 Subeject 의 데이터가 변경 되었을때 Observer 에게 알려주는 내용이다.  

이번에는 옵저버패턴 예시를 위해 TextView를 살펴 보자.  
전체 소스는 많기 때문에 필요한 부분만 짤라서 요약함.  
Subject 는 TextView 이고 Observer 는 TextWatcher 라고 생각하면 될것 같다.  
아래를 보면 TextView(Subject)에서 Observer 를 관리 하기위해 ArrayList<TextWatcher> 로 가지고 있는걸 볼수 있다.  

```java
public class TextView extends View implements ViewTreeObserver.OnPreDrawListener {
    private ArrayList<TextWatcher> mListeners;

    public void addTextChangedListener(TextWatcher watcher) {
        if (mListeners == null) {
            mListeners = new ArrayList<TextWatcher>();
        }
        mListeners.add(watcher);
    }

    public void removeTextChangedListener(TextWatcher watcher) {
        if (mListeners != null) {
            int i = mListeners.indexOf(watcher);
            if (i >= 0) {
                mListeners.remove(i);
            }
        }
    }

    void sendOnTextChanged(CharSequence text, int start, int before, int after) {
        if (mListeners != null) {
            final ArrayList<TextWatcher> list = mListeners;
            final int count = list.size();
            for (int i = 0; i < count; i++) {
                list.get(i).onTextChanged(text, start, before, after);
            }
        }
        if (mEditor != null) mEditor.sendOnTextChanged(start, before, after);
    }

    ...
}
```

아래는 LogTextWatcher(옵저버) 와 PrintTextWatcher(옵저버) 이다.
```java
class LogTextWacher : TextWatcher {
    override fun afterTextChanged(s: Editable?) {
        Log.d("kyh", "텍스트 변경후")
    }

    override fun beforeTextChanged(s: CharSequence?, start: Int, count: Int, after: Int) {
        Log.d("kyh", "텍스트 변경전")
    }

    override fun onTextChanged(s: CharSequence?, start: Int, before: Int, count: Int) {
        Log.d("kyh", "텍스트 변경할때")
    }
}

class PrintTextWatcher : TextWatcher {
    override fun afterTextChanged(s: Editable?) {
        System.out.println("텍스트 변경후")
    }

    override fun beforeTextChanged(s: CharSequence?, start: Int, count: Int, after: Int) {
        System.out.println("텍스트 변경전")
    }

    override fun onTextChanged(s: CharSequence?, start: Int, before: Int, count: Int) {
        System.out.println("텍스트 변경할때")
    }

}
```

아래는 LogTextWatcher(옵저버) 와 PrintTextWatcher(옵저버) 를 TextView(Subject)에 등록
```kotlin
fun initTextView(context: Context) {
    val textView = TextView(context)
    textView.addTextChangedListener(LogTextWacher())
    textView.addTextChangedListener(PrintTextWatcher())
}
```

TextView 의 text 가 변경 될 때마다 Log 와 Print 가 찍히게 된다.  
하지만 유의해야하는것은 같은 옵저버를 두번 등록 하지않게 해야한다.  
두번 등록 하게 될 경우 Log 와 Print 를 한번씩만 호출되길 원하는데 두번씩 호출 될 수 있다.