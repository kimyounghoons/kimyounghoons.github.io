---
layout: post
title: 애니메이션 UI 대응 로티 라이브러리
description: "애니메이션 UI 대응 로티 라이브러리"
modified: 2019-10-07
tags: [animation,lottie]
categories: [android]
---
UI 작업을 하다보면 애니메이션 효과는 피할래야 피할수 없다.  
하지만 처음 디자인팀으로부터 전달 받는 경우 여러 효과가 있을 때 각 효과에 대해 정확한 데이터를 받는 경우는 드물다.  
그렇기 때문에 우선 작업하고 디자인QA 를 거치는 경우가 많다.  
하지만 이 lottie 라이브러리를 사용하게 되면 개발자와 디자인 작업자 모두 행복해 질 수 있다.  
안드로이드 뿐만아니라 IOS , React Native , Web , Window 다 지원하기 때문에 Json파일만 있으면 모두 동일한 애니메이션을 만들 수 있다.  

디자인팀에 해당 애니메이션을 json 파일로 보내달라고 요청하면 받을 수 있는데 해당 파일만 있으면 Ok!  

사용법은 쉽기 때문에 업로드 하지 않는다.  
하지만 사용하면서 RecyclerView 에서 사용하는 경우 2가지 이슈를 발견 했다.  

첫번째 : 나는 한번만 실행 하고 싶은데 스크롤 시 계속 애니메이션 실행 되는 이슈가 있었다.  
라이브러리 내부 코드를 보니 애니메이션이 다 끝나기 전에 해당 뷰가 Detach 되는 경우 Attach 될 때 강제로 animation을 실행 시키고 있었다.  
그래서 다음과 같이 Detach 될때 애니메이션을 강제로 취소시켰다.  
```
@Override
public void onViewDetachedFromWindow(@NonNull ViewHolderGeneric holder) {
    super.onViewDetachedFromWindow(holder);
    if (holder instanceof LottieViewHolder) {
        ((LottieViewHolder) holder).cancelLottieAnimation(); //아래에 LottieViewHolder method 
    }
}

//method in LottieViewHolder
public void cancelLottieAnimation() {
    if (lottieAnimation.isAnimating()) {
        lottieAnimation.cancelAnimation();
        lottieAnimation.setProgress(1f);
    }
}
```

두번째 : 핸들러 사용 하지 않으면 playAnimation 정상 작동 하지 않는 이슈.  
아래와 같이 처리해주었다.  
```
new Handler().post(() -> lottieAnimation.playAnimation());
```

이제는 편하게 애니메이션 적용해보자~!ㅎㅎ