---
layout: post
title: notifyDataSetChanged()
description: "notifyDataSetChanged()"
modified: 2019-10-15
tags: [recyclerview,notifyDataSetChanged]
categories: [android]
---

## 문제
notifyDataSetChanged() 호출 시 RecyclerView 리스트가 갱신이 되지 않는 문제

## 문제 해결
<https://developer.android.com/reference/android/support/v7/widget/RecyclerView.Adapter.html#notifyDataSetChanged()>

LayoutManagers will be forced to fully rebind and relayout all visible views.

문서에 의하면 뷰가 Gone 상태 일때는 갱신이 일어나지 않는다.

그래서 Invisible 상태로 두고 갱신시켜 줘서 해결 함.

## 회고
처음에는 data 가 제대로 들어가 있지 않은것 같아서 확인 했는데 정상적으로 들어가 있었고  
두번째는 adapter 의 data list 의 참조 문제를 의심해서 한 곳에서 list 를 만들고 해당 리스트를 다른곳에서도 같이 바라보게 했지만 그대로 문제가 발생  
세번째는 뷰가 바인딩 되지 않는 것을 보고 이상하게 생각해서 뷰를 가리지 않고 그대로 하니 잘 나옴..  
뷰의 visible 상태에 뭔가 문제가 있다고 생각해서 다큐먼트를 찾아 보니.. 위의 글이 있었다.  
좀 더 정확히 알고 사용하는 버릇을 들여야 할 것 같다.