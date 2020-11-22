---
layout: post
title: Saving UI States
description: UI 상태 저장(Saving UI States)
modified: 2020-11-22
tags: [Android, Kotlin]
categories: [Android, Kotlin]
---

오늘은 UI 상태 저장에 대한 조금 더 깊은 이해와 어떤 옵션들이 있는지 그리고 각각의 옵션들의 장단점에 대해서 알아보려고 한다.  

## UI 상태를 유지 하기 위한 옵션
1. ViewModel
2. Saved instance state
3. Persistent storage

<figure>
	<img src="/images/2020-11-22-save-ui-state.png" alt="">
</figure>

### ViewModel
Android Architecture Components 에 속하는 Component 이다.  
메모리에 저장 되고 화면 가로 세로 변경같은 경우 데이터가 저장 되지만 프로세스에 의해 죽고 살아 나는 경우에 데이터는 다시 복구 되지 않는다.  
그리고 완벽하게 액티비티가 종료 되어 버리거나 사용자가 백버튼을 눌러 종료 하는 경우 데이터를 복구 할 수 없다.  
데이터 제한은 메모리에 저장 되기 때문에 사용가능한 메모리 내에서 가능하고 속도가 빠르다.  

### Save instance state
저번 커스텀 뷰에서 상태 저장을 위한 onSaveInstanceState 를 사용해서 저장하는 방법이다.  
화면 가로 세로 변경 같은 경우, 프로세스 종료 된 이후에도 데이터가 저장 가능 하다.  
하지만 ViewModel 과 마찬가지로 완벽하게 액티비티가 종료 되어 버리거나 사용자가 백버튼을 눌러 종료 하는 경우 데이터를 복구 할 수 없다.  
데이터 제한은 직렬화되어 디스크에 저장 되기 때문에 primitive type 또는 심플하고 작은 object 만 가능 하다.  
직렬화는 데이터를 byte 형태로 변환 하는 것을 말한다.  
디스크에 접근하고 serialization , deserialization 을 거치기 때문에 ViewModel 보다 비교적 느리다.  

### Persistent storage
Shared preferences,SQLite Database,Realm DB 기타 등등..!
디스크에 저장 하거나 네트워크 통신을 통해 서버에 저장하는 것을 의미한다.  
이 경우는 모든 경우에 대비해서 데이터를 저장 할 수 있다.  
네트워크 통신을 통해 저장 하는 경우 다른 옵션들 보다 비교적 느리지만 데이터를 영구 저장하기에는 제일 좋다고 볼 수 있다.  
위와 같이 여러 타입의 UI 상태 저장 방법을 알아보고 비교하는 시간을 가져 보았다.  

참고 문서
https://developer.android.com/topic/libraries/architecture/saving-states

