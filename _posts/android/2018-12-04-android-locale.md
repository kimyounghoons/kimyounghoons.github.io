---
layout: post
title: Locale 
description: "안드로이드 언어 및 지역"
modified: 2018-12-04
tags: [lanuage,country]
categories: [android]
---

문제점 : 다국어 지원 앱
다국어 지원을 위한 라이브러리를 사용해서 앱을 실행할 때 다국어 지원 관련 셋팅을 하게 된다.
간체,번체의 경우 language 와 country 두가지 모두 사용해서 구분을 하게 된다.
하지만 지역별로 이벤트를 설정 할 경우 locale 지역 정보를 가져올때 시스템에 설정된 지역 정보가 아닌 앱에서 설정된 지역을 가져오기때문에 문제   

해결 : Application Class 에서 다국어 관련 Locale 을 셋팅 하기 전 country 정보를 미리 SharedPreference 에 저장해 놓고 필요한 경우 가져 온다. 시스템 언어를 변경 할 경우 onConfigurationChanged 가 불리게 되어서 여기서 다시 지역 정보를 갱신 시켜 주어야 한다.

```
    Application Class

    @Override
    public void onCreate() {
        super.onCreate();
        PreferenceManager.getDefaultSharedPreferences(this)
        .edit()
        .putString(getString(R.string.pref_key_system_country), Locale.getDefault().getCountry())
        .apply();
        LocaleChanger.initialize(getApplicationContext(), SUPPORTED_LOCALES);
    }

      @Override
    public void onConfigurationChanged(Configuration newConfig) {
        super.onConfigurationChanged(newConfig);
        if (newConfig.getLocales().get(0) != null) { //첫번째가 변경된 Locale 정보를 가진다.
            PreferenceManager.getDefaultSharedPreferences(this).edit().putString(getString(R.string.pref_key_system_country), newConfig.getLocales().get(0).getCountry()).apply();
        }
        LocaleChanger.onConfigurationChanged();
    }

```