---
layout: post
title: RxKotlin combineLatest
description: "RxKotlin combineLatest"
modified: 2019-12-12
tags: [RxKotlin]
categories: [android]
---

집에 와서 안드로이드 단톡방 쭉쭉 보다가 어떤 분이 질문 올린 글을 봤다.  
Rx 공부 해야지 해야지 생각 하다가 안하고 있었는데 마침 문제가 하나 생김 ㅎㅎ  

문제는 RxKotlin 을 사용해서 입력창 세가지가 있는데 각각 입력창에서 입력 할 때마다 validation 체크를 하고 다 통과 되면 버튼을 활성화 시키고 싶다.  

키워드는 combineLatest 이다. 아래의 TestCode 를 보자.  

```
class CombineLatestTest {
    @Test
    fun test() {
        var isValid = false
        val emailSubject = BehaviorSubject.create<String>()
        val passwordSubject = BehaviorSubject.create<String>()
        val nickNameSubject = BehaviorSubject.create<String>()
        BehaviorSubject.combineLatest<String, String, String, Boolean>(
            emailSubject,
            passwordSubject,
            nickNameSubject,
            Function3<String, String, String, Boolean> { email, password, nickname ->
                return@Function3 email.isNotEmpty() && password.isNotEmpty() && nickname.isNotEmpty()
            }
        ).subscribe { result ->
            isValid = result
             println("활성화 여부 $isValid")
        }

        emailSubject.onNext("이메일 입력")
        assertEquals(isValid, false)

        passwordSubject.onNext("패스워드 입력")
        assertEquals(isValid, false)

        nickNameSubject.onNext("닉네임 입력")
        assertEquals(isValid, true)

        passwordSubject.onNext("")  //패스워드 제거
        assertEquals(isValid, false)

        passwordSubject.onNext("패스워드 입력")  //패스워드 다시 입력
        assertEquals(isValid, true)

    }

}


    결과는 
    활성화 여부 false
    활성화 여부 false
    활성화 여부 true
    활성화 여부 false
    활성화 여부 true


```

combineLatest 은 파라미터로 ObservableSource 를 받고 마지막에 리턴할 Function 을 받는다.  
ObservableSource 갯수 제한은 9개 까지로 확인했다.  
테스트 코드라서 isNotEmpty 로 처리 해뒀는데 저기 부분이 isValid 로 변경 되면 되겠다~  