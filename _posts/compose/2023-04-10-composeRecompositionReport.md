---
layout: post
title: Compose Compiler Metrics Report
description: "Compose Compiler Metrics Report"
modified: 2023-04-07
tags: [Compose]
categories: [compose]
---

## Compose Compiler Metrics Report?

Compose Compiler Metrics Report는 Compose Compiler의 성능과 효율성을 측정하고 개선하기 위해 생성되는 보고서입니다.</br>

Compose를 사용하여 개발할 때, 의도치 않은 Recomposition이 발생할 수 있기 때문에 Layout Inspector를 사용하여 RecompositionCount와 SkipCount를 확인할 수 있습니다.</br>
그러나 이 방법은 Composable의 위치는 확인할 수 있지만, 정확히 어떤 부분에서 문제가 발생했는지를 알기 어렵습니다.</br>
이때 Compose Compiler Metrics Report를 사용하면 더 자세하게 문제를 파악할 수 있습니다.

## 사용 방법

app.gradle 파일에 아래와 같은 코드를 추가하여 Compose Compiler Metrics Report를 생성할 수 있습니다.
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

그 중 app_debug-classes.txt 파일을 보면 다음과 같이 stable 과 unstable 그리고 runtime 으로 표시 된걸 볼 수 있습니다.

<figure>
    <p align="center">
	    <img src="/images/2023-4-10-compose-compiler-metrics-report02.png" alt="" width="600" height="450"/>
	</p>
</figure>

Composable의 파라미터로 들어가게 되는 UiState에는 unstable value 가 들어가는 경우 불필요한 Recomposition 이 발생됩니다.

해당 부분의 원인을 찾아 stable 한 상태로 변경 시켜주면 불필요한 recomposition을 막을 수 있습니다.

제가 어떤 식으로 원인을 찾았고 또 어떤 방법으로 문제를 해결하였는지에 대해서는 다음 포스트에 올리도록 하겠습니다.