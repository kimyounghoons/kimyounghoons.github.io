---
layout: post
title: 안드로이드 10(29) 블루투스 연결 대응
description: "안드로이드 10(29) 블루투스 연결 대응"
modified: 2020-01-16
tags: [안드로이드 10(29) 블루투스 연결 대응]
categories: [android]
---

# 문제점 
## bluetoothAdapter.startDiscovery() return false !!!

true로 반환 되어야 하는데 false 로 반환 되어 블루투스 검색을 할 수 없음  

안드로이드 10 버전에서 위치 관련 보안이 강화 되었는데 블루투스 검색 할때 위치 관련 퍼미션이 필요하기 때문에 일어난 퍼미션 관련 이슈였습니다.  

버전 별로 블루투스 검색 및 연결 관련 퍼미션을 알아보겠습니다.  

기본적으로 블루투스 기능 ON , 위치 기능 ON 상태 여야 합니다.   

블루투스 기능 OFF 상태일 때는 사용 가능 하게 팝업 띄어 주고 위치 기능 OFF 상태일때는 체크 해서 설정 화면으로 보내면됩니다.  

## targetSdkVersion 29 미만인 경우

AndroidManifest.xml 파일

```
<uses-permission android:name="android.permission.BLUETOOTH" />
<uses-permission android:name="android.permission.BLUETOOTH_ADMIN" />
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
```

29 미만인 경우 위치 관련 퍼미션이 적용 되어 있을 경우 아래 퍼미션이 자동으로 추가 됩니다.

```
<uses-permission android:name="android.permission.ACCESS_BACKGROUND_LOCATION"/>
```

아래 퍼미션은 런타임 퍼미션 적용  
Manifest.permission.ACCESS_COARSE_LOCATION

## targetSdkVersion 29 인 경우

AndroidManifest.xml 파일 

```
<uses-permission android:name="android.permission.BLUETOOTH" />
<uses-permission android:name="android.permission.BLUETOOTH_ADMIN" />
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_BACKGROUND_LOCATION"/>
```

아래 퍼미션은 런타임 퍼미션 적용  
```
Manifest.permission.ACCESS_COARSE_LOCATION,
Manifest.permission.ACCESS_FINE_LOCATION,
Manifest.permission.ACCESS_BACKGROUND_LOCATION
```
퍼미션 승인 후 bluetoothAdapter. startDiscovery() 이 true로 반환되는걸 확인 할 수 있었습니다.

아래 이미지는 블루투스 연결 버튼 클릭 시 플로우 차트  
<figure>
	<img src="/images/2020-01-17-bluetooth-flow-chart.png" alt="">
</figure>