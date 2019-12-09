---
layout: post
title: 디버그 용 서명 키 만들기
description: "디버그 용 서명 키 만들기"
modified: 2019-12-09
tags: [SigningKey,DebugStore]
categories: [android]
---

기본적으로 Android Studio 에서 프로젝트를 만들게 되면 기본적으로 Build Variants가 Debug 로 셋팅 되어 있다.  

Android SDK 도구에서 생성된 디버그 인증서를 사용하여 앱에 자동으로 서명하기 때문이다.  

안드로이드 앱을 출시 하기 위해서는 Release KeyStore 를 통해 서명 된 앱이 필요하다.  

Release KeyStore 를 생성해서 서명한 앱을 출시 하고 난 뒤, Release 용 apk 가 설치 된 앱에서 Debug 모드로 빌드하게 되면 빌드 실패가 뜨고 삭제 후 설치 하시겠습니까? 라는 팝업이 뜬다.  

이 이유는 Release apk 와 Debug apk 인증 정보가 다르기 때문이다.  

이렇게 되면 삭제 후 설치는 할 수 있지만 Build Variants 가 변경될 때마다 다시 설치해야하는 번거러움도 있고 삭제하지 않고 디버깅 하고 싶은 상황에서 할 수 없게 된다.  

이 상황을 해결 하기 위해서는 Release Keystore 로부터 Debug Keystore 를 만들면 된다.

```
keytool -importkeystore -v -srckeystore 릴리즈용 키스토어 파일이름 및 경로 -destkeystore 디버그용 키스토어 파일이름 및 경로 -srcstorepass 릴리즈용 키스토어 비밀번호 -deststorepass android -srcalias alias이름 -destalias androiddebugkey -srckeypass alias비밀번호 -destkeypass android
```
    
새로 만드는 Debug Keystore 를 Android Studio 기본 제공해주는 Debug Keystore 와 매칭 시켜주는 작업이다.  
-destalias androiddebugkey , -deskkeypass android  는 Android SDK 도구에서 생성된 디버그 인증 정보와 동일하게 해서 나중에 별도 셋팅이 필요없다. 

build.gradle 셋팅은 아래와 같이 하면 된다.

```
android {
 signingConfigs {
        release {
            storeFile file('릴리즈 키스토어 경로')
            storePassword '패스워드'
            keyAlias '릴리즈 alias'
            keyPassword '패스워드'
        }

        debug {
            storeFile file('디버그 키스토어 경로')
            //따로 정보 필요 없음
        }
    }
}
```