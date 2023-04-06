---
layout: post
title: 재귀
description: 재귀
modified: 2021-06-20
tags: [Android, Kotlin]
categories: [Android, Kotlin]
---

함수형 프로그래밍에서는 명령문을 반복할 때 루프 대신에 재귀를 사용 한다.

### 3.1 함수형 프로그래밍에서 재귀가 가지는 의미

재귀는 어떤 함수의 구현 내부에서 자기 자신을 호출하는 함수를 정의하는 방법을 말한다.

피보나치 수열을 명령형 프로그래밍으로 구현한 예제를 보자.

```java
fun main(args: Array<String>){
    println(fibo(10,IntArray(100)) // 55 출력
}

private fun fibo(n : Int, fibo: IntArray): Int {
    fibo[0] = 0
    fibo[1] = 1

    for(i in 2..n){
        fibo[i] = fibo[i-1] + fibo[i-2]
    }
    return fibo[n]
}
```

피보나치 수열의 결과를 한번에 얻으려고 하지 않고 단계별 결과 값을 구해서 합하였다.

이전 값들을 기억하기 위해 한 메모리 IntArray(100) 을 확보해 놓았다.

루프가 반복 되면서 이전 값이 필요하면 메모리에 저장된 값을 사용한다.

본예제에서는 피보나치수열을 100개 까지만 계산할 수 있다.

피보나치 수열을 재귀로 구현한 예제를 보자.

```java
fun main(args: Array<String>){
    println("결과 : "+fiboResursion(10))//55 출력
}

private fun fiboResursion(n : Int) : Int = when(n){
        0 -> 0
        1 -> 1
        else -> fiboResursion(n-1) + fiboResursion(n-2)
}
```

내부에서 자기 자신을 호출하여 재귀로 피보나치 수열 문제를 해결 하였다.

재귀로 구현한 예제는 고정 메모리 할당이나 값의 변경이 없다.

메모리를 직접 할당해서 사용하지 않고 스택을 활용 한다.

재귀 호출을 사용하면 컴파일러는 내부적으로 현재 호출하는 함수에 대한 정보들을 스택에 기록해 두고 다음 함수를 호출한다.

메모리에 할당 하지 않고 컴파일러에 의해 관리 된다고 보면 된다.

### 함수형 프로그래밍에서 재귀
함수형 프로그래밍에서는 어떻게 값을 계산할 수 있을지 선언하는 대신 무엇을 선언할지를 고민 해야 한다.

for , while 문과 같은 반복 명령어는 구조적으로 어떻게 동작해야 하는지를 명령하는 구문이다.

따라서 함수형 프로그래밍은 루프를 사용해서 해결하던 문제들을 재귀로 풀어야 한다.

재귀는 반복문에 비하여 복잡한 알고리즘을 간결하게 표현할 수 있지만, 다음과 같은 문제점을 가진다.

1. 동적 계획법 방식에 비해서 성능이 느리다.
2. 스택 오버플로 오류가 발생할 수 있다.