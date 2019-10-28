---
layout: post
title: FragmentStatePagerAdapter 
description: "FragmentStatePagerAdapter BEHAVIOR_SET_USER_VISIBLE_HINT, BEHAVIOR_RESUME_ONLY_CURRENT_FRAGMENT"
modified: 2019-10-29
tags: [FragmentStatePagerAdapter,setUserVisibleHint,onResume]
categories: [android]
---

FragmentStatePagerAdapter 안에서 사용 되는 Fragment 의 경우 현재 보여지는 Fragment 를 인식 하기 위해 

## 기존 방법(Deprecated)  
setUserVisibleHint(isVisibleToUser : Boolean) method를 오버라이드 해서 사용하였다. 

## 새로운 방법  
FragmentStatePagerAdapter 생성자부분에서 두번째 파라미터가 생겼는데 BEHAVIOR_SET_USER_VISIBLE_HINT,BEHAVIOR_RESUME_ONLY_CURRENT_FRAGMENT 두가지 값으로 나뉘어진다. BEHAVIOR_SET_USER_VISIBLE_HINT 는 deprecated 되었고 BEHAVIOR_RESUME_ONLY_CURRENT_FRAGMENT 를 권장 사용하게 한다.

<figure>
	<img src="/images/20191029_Android_FragmentStatePagerAdapter.png" alt="">
</figure>
<https://developer.android.com/reference/androidx/fragment/app/FragmentStatePagerAdapter.html#BEHAVIOR_RESUME_ONLY_CURRENT_FRAGMENT>  

아래와 같이 사용하면 된다~!

Adapter 예시 
```
class MainAdapter(fm: FragmentManager) :
    FragmentStatePagerAdapter(fm, BEHAVIOR_RESUME_ONLY_CURRENT_FRAGMENT) {

    }
```

Fragment 예시
```
override fun onPause() {
        super.onPause()
        //Fragment 가려질 때 처리
    }

    override fun onResume() {
        super.onResume()
        //Fragment 보여질 때 처리
    }
```

그리고 마지막으로
만약 onResume 이나 onPause가 두번 불리게 된다면 다른 Listener 를 통해 해당 프래그먼트가 불리고 있지 않은지 체크 해봐야한다.