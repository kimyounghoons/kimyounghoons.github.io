---
layout: post
title:  Strategy Pattern (디자인 패턴 1장)
description: "Strategy Pattern (디자인 패턴 1장)"
modified: 2020-03-16
tags: [Strategy Pattern]
categories: [java,kotlin]
---

### Strategy Pattern 에 대해 알아보자.

알고리즘군을 정의하고 각각을 캡슐화하여 교환해서 사용할 수 있도록 만든다.  
스트래티지를 활용하면 알고리즘을 사용하는 클라이언트와는 독릭적으로 알고리즘을 변경할 수 있다.  

1장에서는 다음과 같은 3가지 디자인 원칙이 나온다.  

1. 애플리케이션에서 달라지는 부분을 찾아내고, 달라지지 않는 부분으로부터 분리시킨다.
2. 구현이 아닌 인터페이스에 맞춰서 프로그래밍한다.
3. 상속보다는 구성을 활용한다.

디자인 패턴책에서는 오리의 예제를 들었는데 안드로이드 부분에서 찾아 보았다.  
얼마전 AppCenter 를 사용하면서 사용된 스트래티지패턴의 예제를 보려고 한다.

```
class InAppUpdateListener : DistributeListener {
    override fun onReleaseAvailable(
        activity: Activity?,
        releaseDetails: ReleaseDetails
    ): Boolean {
        activity?.apply {
            val dialogBuilder = AlertDialog.Builder(activity)
            dialogBuilder.setTitle(R.string.in_app_update)
            dialogBuilder.setMessage(R.string.msg_download_latest_version)
            dialogBuilder.setPositiveButton(R.string.common_confirm) { _, _ ->
                Distribute.notifyUpdateAction(UpdateAction.UPDATE)
            }
            dialogBuilder.setCancelable(false)
            dialogBuilder.create()
                .show()
            return true
        }
        return false
    }
}

Distribute.setListener(InAppUpdateListener())
```

위의 코드는 업데이트할 버전이 있을때 onReleaseAvailable 가 불려서 다이얼로그를 띄우고 확인을 누르게 되면 업데이트를 진행한다.  
Distribute 에는 DistributeListener 인터페이스를 구성했다.   
  
다이얼로그를 띄우고 확인을 누르게 되면 업데이트를 진행한다. 라는 알고리즘을 캡슐화해서 사용하였고 InAppUpdateListener를 다른 DistributeListener로 교환해서 사용할 수 있다.  
그렇기 때문에 이 코드도 strategy pattern 으로 볼 수 있다.  