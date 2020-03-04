---
layout: post
title: AppCenter Distribute
description: "AppCenter Distribute"
modified: 2020-03-03
tags: [android]
categories: [android]
---

Firebase Distribute 를 사용하다가 배포된 apk 앱에서 최신 버전 유지가 필요해서 인앱업데이트 기능이 필요했다.  
AppCenter 라는 Microsoft 에서 만든 sdk 가 있어서 이걸 사용해 보도록 했다.  
Gradle Dependency 에는 다음과 같이 추가해 준다.  
```
dependencies {
   def appCenterSdkVersion = '3.0.0'
   implementation "com.microsoft.appcenter:appcenter-distribute:${appCenterSdkVersion}"
}
```

그리고 인앱 업데이트 할 때 CustomDialog를 보여주기 위해 DistributeListener 인터페이스를 구현한 클래스를 하나만들어 준다.  
클래스명은 임의로 InAppUpdateListener 라고 만들었다.  
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

```

Distribute.notifyUpdateAction(UpdateAction.UPDATE) 를 사용하게 되면 리턴을 true 로 셋팅 해준다.  
return 을 false로 할 경우 액티비티 변경 될 때마다 콜백이 호출 될 수 있다.  

```
Distribute.setUpdateTrack(UpdateTrack.PRIVATE)
Distribute.setListener(InAppUpdateListener())
AppCenter.start(application, BuildConfig.KEY_APP_CENTER_SECRET, Distribute::class.java)
```
테스트 그룹을 Private 으로 한 경우 첫번째줄 UpdateTrack 을 셋팅 해줘야 한다.  
KEY_APP_CENTER_SECRET 는 앱의 시크릿 키 값이다. AppCenter Console -> Settings 우측 상단 메뉴 버튼 클릭  
<figure>
	<img src="/images/2020-03-03-appcenter.png" alt="">
</figure>

앱 업데이트가 필요한 곳에서 다음과 같이 호출

```
Distribute.setUpdateTrack(UpdateTrack.PRIVATE)
Distribute.setListener(InAppUpdateListener())
AppCenter.start(application, BuildConfig.KEY_APPCENTER_SECRET, Distribute::class.java)
```

유의할 점은 해당 versionCode 가 Distribute 에 업로드 되어 있다는 전제하에 업데이트 할 버전이 있다면 InAppUpdateListener onReleaseAvailable 가 호출 된다.  
현재 versionCode 가 1 이고 Distribute 에 있는 버전이 2 버전이라면 콜백이 호출 되지 않는다.  
Distribute 에는 1,2 현재버전까지 포함되서 올라가 있어야 호출됨.  
그리고 Distribute 셋팅은 AppCenter.start 가 호출 되기 전에 호출 되어야 한다. 순서가 바뀔 경우 제대로 동작하지 않을수 있음.  
AppCenterSecret Key 는 노출 되면 안되기 떄문에 환경변수에 저장해서 사용했다.

```
//앱 build.gradle

buildTypes {
        release {
            ...
        }

        debug {
            ...
        }

        buildTypes.each {
            it.buildConfigField 'String', 'KEY_APP_CENTER_SECRET', "\"$System.env.KEY_APP_CENTER_SECRET\""
        }
}
```

이번에 Github Actions 를 사용해서 Appcenter Distribute 에 apk 배포하는걸 자동화해서 작업을 했는데 그부분은 다음 글에서 올림.