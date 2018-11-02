---
layout: post
title: app build 문제 
description: "app build 문제"
modified: 2018-11-01
tags: [android]
categories: [android]
---

<div class ="errBlock">    
Error while executing: am start -n "com.mngs.kimyounghoon.mngs/com.mngs.kimyounghoon.mngs.MainActivity" -a android.intent.action.MAIN -c android.intent.category.LAUNCHER
Starting: Intent { act=android.intent.action.MAIN cat=[android.intent.category.LAUNCHER] cmp=com.mngs.kimyounghoon.mngs/.MainActivity }
Error type 3
Error: Activity class {com.mngs.kimyounghoon.mngs/com.mngs.kimyounghoon.mngs.MainActivity} does not exist.

Error while Launching activity
</div>

핸드폰에 어플은 지웠는데 계속 저 문구가 뜨거나 이전 버전 삭제하고 새로 설치 하시겠습니까 팝업도 떠서 ok 눌러도 새로 설치가 안됨...

adb uninstall -r <패키지명>
adb uninstall <패키지명>


지우려고 시도해도 삭제가 안되고 계속 에러가 뜸..

어플은 다운 그레이드 시켜서 설치 할수 없다는 문구도 뜸..

adb uninstall -r -d <패키지명>
시도 해도 안됨...

그러다가 보안 폴더에 앱을 넣었던 기억이.. 갑자기 생각남 아 ... 설마 했는데 안에 들어있네..

보안 폴더에 있는 어플은 따로 관리 되서 adb로는 지울 수 없게 되어있는 듯함..

보안 폴더 지우고 새로 build 해보니 잘 됨..

다음부터는 uninstall 안되면 보안폴더부터 보게 될거 같음..

삽질이었지만 보안폴더의 보안은 좋았던걸로 하고 끝..