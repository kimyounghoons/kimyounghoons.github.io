---
layout: post
title: Build Type String
description: "빌드 타입별 스트링 관리"
modified: 2018-11-19
tags: [buildType]
categories: [android]
---

# 빌드 타입 별 스트링 관리

페이스북 소셜 연동 시 다음과 같은 facebook 앱 아이디 정보가 필요하다.
```
<meta-data
    android:name="com.facebook.sdk.ApplicationId"
    android:value="@string/facebook_app_id" />
```

페이스북 소셜 연동 시 live , dev , stage 세가지로 나눠서 작업을 하고 있는데 facebook_app_id 가 각각 build Type 별로 다르기 때문에 type이 바뀔 때 마다 동적으로 변경 되어야 하는 문제가 생겼다.

```
   buildTypes {
        release {
            resValue "string", "facebook_app_id","페이스북 앱 아이디"
            resValue "string", "fb_login_protocol_scheme","fb페이스북 앱 아이디"
        }
        staging {
            resValue "string", "facebook_app_id","페이스북 앱 아이디"
            resValue "string", "fb_login_protocol_scheme","fb페이스북 앱 아이디"
        }
        dev {
            resValue "string", "facebook_app_id","페이스북 앱 아이디"
            resValue "string", "fb_login_protocol_scheme","fb페이스북 앱 아이디"
        }
    }
```

buildType 별로 facebook_app_id , fb_login_protocol_scheme 각각 string 정보에 설정한 값이 들어가게 된다.