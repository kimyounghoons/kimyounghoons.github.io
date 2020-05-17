---
layout: post
title:  Singleton Pattern (디자인 패턴 5장)
description: "Singleton Pattern (디자인 패턴 5장)"
modified: 2020-05-17
tags: [Singleton Pattern]
categories: [java,kotlin]
---

### 싱글턴 패턴 정의
싱글턴 패턴은 해당 클래스의 인스턴스가 하나만 만들어지고, 어디서든지 그 인스턴스에 접근할 수 있도록 하기 위한 패턴입니다.

싱글턴 패턴 사용방법은 크게 3가지로 나뉘어진다.

1. getInstance() 메소드에 synchronized 키워드를 사용
2. 인스턴스를 필요할 때 생성하지 말고, 처음부터 만듦
3. DCL(Double-Checking Locking) 을 써서 getInstance()에서 동기화 되는 부분을 줄임

첫번째 방법부터 보자.
```java
class Singleton {
    private static Singleton uniqueInstance;

    private Singleton() {//생성자를 외부에서 사용하지 못하도록 private 으로 셋팅
    }

    public static synchronized Singleton getInstance() {
        //synchronized 를 사용해 서로 다른 스레드에서 실행 시 하나의 스레드가 끝난 뒤 다른 스레드가 접근 가능
        if (uniqueInstance == null) {
            uniqueInstance = new Singleton();
        }
        return uniqueInstance;
    }

}
```
위의 방법처럼 메소드를 동기화 하게 되면 속도가 100배 정도 저하된다고 한다.
속도가 크게 중요하지 않은 부분에서 사용하면 괜춘~!

두번째 방법을 보자.
```java
class Singleton {
    private static Singleton uniqueInstance = new Singleton();//정적 초기화 부분에서 생성
    
    private Singleton() {
    }

    public static Singleton getInstance() {
        return uniqueInstance;
    }

}
```
위의 방법처럼 사용하면 JVM에서 Singleton 의 유일한 인스턴스를 생성해준다.
JVM에서 유일한 인스턴스를 생성하기 전에는 어떤 스레드도 uniqueInstance 에 접근 할 수 없다.

세번째 방법을 보자.
```java
class Singleton {
    private volatile static Singleton uniqueInstance;

    private Singleton() {
    }

    public static Singleton getInstance() {
        if (uniqueInstance == null) {
            synchronized (Singleton.class) {
                if (uniqueInstance == null) {
                    uniqueInstance = new Singleton();
                }
            }
        }
        return uniqueInstance;
    }

}
```
volatile 키워드를 사용하면 멀티 스레딩을 쓰더라도 uniqueinstance 변수가 Singleton 인스턴스로 초기화 되는 과정이 올바르게 진행되도록 할 수 있다.  
하지만 자바5 보다 전에 나온 버전의 JVM 을 사용해야 한다면 DCL을 사용할 수 없다.  

일반적인 변수의 값은 CPU cache 를 통해 가져 오는데 volatile 키워드를 사용하면 CPU cache가 아닌 MainMemory 에서 가져온다.  
그렇기 떄문에 읽고 쓰는 작업을 할 때 제대로 된 값을 읽고 쓸 수 있다.  
하지만 멀티 스레드에서 값을 가져올 때 synchronized 를 사용하지 않고 가져오면 완벽하게 보장 되지 않기 때문에 위의 코드를 보면 volatile uniqueInstance 를 초기화 할때 synchronized 를 사용한 것을 볼 수 있다.  
이렇게 사용함으로써 안전하게 초기화 하고 instance를 가져와서 사용할 수 있다.  
코틀린에서는 아주 간단하게 싱글턴을 만들어 사용한다.  
```java
object Singleton {

}
```
위의 코드가 끝이다.  
해당 코틀린 코드는 자바로 Decompile 했을때 1,2,3 방법 중 어떤 방법으로 만들어 질까 궁금해서 한번 해봤다.  
Android Studio 에서 Menu > Tools > Kotlin > Show Kotlin Bytecode > Decompile !!  

```java
public final class Singleton {
   public static final Singleton INSTANCE;

   private Singleton() {
   }

   static {
      Singleton var0 = new Singleton();
      INSTANCE = var0;
   }
}
```
두번째 방법으로 만들어지는걸 볼 수 있었다.  
이상 싱글턴 패턴에 대해 알아보았다.  

참고 URL  
http://tutorials.jenkov.com/java-concurrency/volatile.html