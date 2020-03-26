---
layout: post
title: Firebase Push Test in Postman
description: "파이어베이스 푸시"
modified: 2018-11-20
tags: [firebase]
categories: [firebase]
---

# 포스트맨에서 파이어베이스 푸시 테스트!!

POST 형식 url : https://fcm.googleapis.com/fcm/send

### Headers

| KEY              | VALUE              |
|:-----------------|:------------------:|
| Authorization    | key= server key    |
| Content-Type     | application/json   |
|-
|
{: rules="groups"}

### Body raw (application/json)

```
{
    "to":"받을 사람 firebase token key",
    "notification":{
      "title":"제목",
      "body":"바디"
      ...
    },
    "data" : {
      "letterId" : "CUXbq8XrXOBOw8bWfMkz"
      ...
    }
}
```

보내면 push 가 성공적으로 200 떨어진다.