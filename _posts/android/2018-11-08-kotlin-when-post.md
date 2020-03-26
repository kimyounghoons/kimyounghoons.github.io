---
layout: post
title: how to use `when` in kotlin
description: "how to use `when` in kotlin"
modified: 2018-11-08
tags: [kotlin]
categories: [kotlin]
---

우선 자바 소스

```
 @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        switch (item.getItemId()) {
            case R.id.menu_write_letter:
                obtainViewModel().sendLetter()
                return true;
            default: 
                return false;
        }
    }
```

아래는 코틀린 

```
  override fun onOptionsItemSelected(item: MenuItem): Boolean = when (item.itemId) {
        R.id.menu_write_letter -> {
            obtainViewModel().sendLetter()
            true
        }
        else -> {
            false
        }
    }
```

코틀린의 메소드는 = 으로 바로 값을 리턴 시킬수 있다.

when은 switch랑 비슷한것 같은데 뭐가 차이가 날까 ~ 궁금해서 구글링 해서 찾아보니~

2가지가 있었다. 

1.Auto-casting

2.when without arguments

1번 예를 한번 보면 

```
when(view){
    is TextView -> toast(view.text)
    else -> 
}
```

is TextView 한다음에는 view가 TextView로 자동 캐스팅되어서 사용할 수 있다는 점이 편리하다.

2번 예를 한번 보면 

```
val str = when{
    x in 1..5 -> "cheap"
    s.contains("money") -> "Welcome"
    else -> ""
}
```

when(argument) 를 사용하지 않고 when {} 으로 바로 사용.
