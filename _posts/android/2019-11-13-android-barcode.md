---
layout: post
title: DS6 Barcode (Android 7.1 Nougat) 
description: "DS6 Barcode"
modified: 2019-11-13
tags: [barcode,DS6,바코드]
categories: [android]
---

### 기기명 : DS6 (Android 7.1 Nougat) 

기본적으로 바코드 트레이라는 앱이 설치 되어있습니다.  
이 앱 안에서 스캔 설정을 할 수 있습니다.  
스캔 타임, 데이터 수신 방식, 인식 성공시 알림 등등 여러가지가 있습니다.  
여기서 중요한 데이터 수신 방식(3가지)에 대해 알아 보겠습니다.  

1. INTERNET_EVENT
2. KEYBOARD_EVENT
3. CLIPBOARD_EVENT

### INTERNET_EVENT 방식
**BroadcastReceiver** 로 데이터를 받아 올 수 있습니다.

ActionName : **app.dsic.barcodetray.BARCODE_BR_DECODING_DATA**

BarcodeData : **EXTRA_BARCODE_DECODED_DATA**

해당 액션이름으로 이벤트를 받을 수 있고 **EXTRA_BARCODE_DECODED_DATA** 키 값으로 intent 에 담겨 오는 데이터를 가져올 수 있습니다.

이 방식으로 데이터를 받을 경우 EditText 에 포커스가 되어 있어도 브로드캐스트리시버로 데이터가 넘어 왔기 때문에 EditText 에 입력 되지 않습니다.

### KEYBOARD_EVENT 방식  
바코드 스캔 할 때 EditText 포커스 된 경우만 데이터가 셋팅 됩니다.  

<span style="color:red">주의 사항</span>  
이 방식은 실제로 키보드 타이핑하는 방법과 같습니다.  
키보드가 한글로 셋팅 되어 있을 경우 스캔한 내용이 영어일지라도 한글 키보드로 입력한것 처럼 됩니다.  
rladudgns0837 내용을 스캔한경우 김영훈0837 이런식으로 입력되기 때문에 주의해야합니다.

키보드가 안보일 경우도 똑같이 적용됩니다.

### CLIPBOARD_EVENT 방식
바코드 스캔 할 때 클립보드에 복사되고 EditText에 포커스 되어 있을 경우만 데이터 셋팅 됩니다. 키보드 타이핑 방식은 아니기 때문에 키보드가 영어, 한국어 무관합니다.