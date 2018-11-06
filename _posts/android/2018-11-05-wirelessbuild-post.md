---
layout: post
title: wireless build
description: "wireless build"
modified: 2018-11-05
tags: [android]
categories: [android]
---

## 무선으로 빌드 하는 방법

처음에는 usb 연결이 필요하다.

터미널에서 
adb tcpip 5555 치면

restarting in TCP mode port : 5555

5555포트가 열린거임

adb connect 주소:5555 라고 치면 

connected to 주소:5555 라고 나온다. 

그럼 연결 성공

usb를 빼고 

adb devices 로 연결된 디바이스를 확인할 수 있다.