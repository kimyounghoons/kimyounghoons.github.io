---
layout: post
title: Compose Compiler Metrics Report
description: "Compose Compiler Metrics Report"
modified: 2023-04-07
tags: [Compose]
categories: [compose,kotlin]
---

Compose를 통해 개발하는 경우 의도치 않은 Recomposition 이 발생될 수 있기 때문에 Layout Inspector 를 통해 RecompositionCount 와 SkipCount 를 보면서 간단하게 체크 해볼 수 있습니다. 하지만 이렇게 체크 하는 경우에는 Composable 위치는 알 수 있지만 정확히 어떤 부분이 문제인지는 인지하기가 어렵습니다.
그래서 조금 더 자세히 알기 위해서는 Compose Compiler Metrics Report의 도움을 받을 수 있습니다.

app.gradle에 아래와 같은 코드를 넣어 주기만 하면 됩니다.
```
android{
    kotlinOptions {    
        freeCompilerArgs += [
            "-P",
            "plugin:androidx.compose.compiler.plugins.kotlin:reportsDestination=${rootProject.file(".").absolutePath}/report/compose-reports"
        ]
    }
}
```

빌드 후에 해당 reportsDestination 위치를 찾아 가면 report 폴더안에 아래와 같은 파일들을 볼 수 있습니다.

<figure>
    <p align="center">
	    <img src="/images/2023-4-10-compose-compiler-metrics-report01.png" alt="" width="600" height="450"/>
	</p>
</figure>

그 중 app_debug-classes.txt 파일을 들어가 보면 다음과 같이 stable 과 unstable 그리고 runtime 으로 표시 된걸 볼 수 있습니다.

<figure>
    <p align="center">
	    <img src="/images/2023-4-10-compose-compiler-metrics-report02.png" alt="" width="600" height="450"/>
	</p>
</figure>

여기서 주의해서 봐야 하는 점은 unstable 부분 입니다.
특히 Composable의 파라미터로 들어가게 되는 UiState 에는 unstable value 가 들어가는 경우 불필요한 Recomposition 이 발생됩니다.

해당 부분의 원인을 찾아 stable 한 상태로 변경 시켜주면 불필요한 recomposition을 막을 수 있습니다.

제가 어떤 식으로 원인을 찾았고 또 어떤 방법으로 문제를 해결하였는지에 대해서는 다음 포스트에 올리도록 하겠습니다.