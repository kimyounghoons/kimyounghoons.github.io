---
layout: post
title:  Koin (1편)
description: Koin (1편)
modified: 2020-09-20
tags: [Koin]
categories: [kotlin]
---

### 코인이란?
코틀린 개발자를 위한 실용적인 경량 종속성 주입 프레임 워크이다.  
코인은 도메인 특화 언어인 DSL(Domain Specific Language) 이다.  

#### Koin DSL
applicationContext : Koin 모듈 생성  
factory : 매번 inject 할때마다 인스턴스를 생성  
single : 싱글톤으로 생성  
bind : 종속시킬 class 나 interface를 주입  
get : 컴포넌트내에 알맞은 의존성 주입  

의존성 주입을 하기 위한 라이브러리는 대거2 도 있지만 코인은 좀 더 사용하기 쉽고 가볍게 만들어 졌기 때문에 러닝커브가 낮다고 할 수 있다.  

우선 코인을 사용하기 위해 앱 모듈 단에서 dependency를 추가 해준다.
```java
dependencies {
    //현재 최신버전인 1.0.2 버전으로 사용하였다.
    implementation "org.koin:koin-android:1.0.2" 
}
```

다음 예제 코드를 보자.  
```java
class TestApplication : Application() { //Application 클래스에서 모듈과 startKoin을 호출 해준다.

    private val goodsModule = module {
        factory { GoodsData() } //상품데이터 생성
        single { GoodsServiceImpl(get()) as GoodsService } 
        //GoodsService는 싱글톤으로 생성 되고  get() 은 컴포넌트내에 알맞은 의존성 주입을 해준다.
        //get() 은 GoodsData 라고 생각 하면 된다.
        factory {
            //FactoryData 가 생성 될 때마다 변경되는 값을 넣기 위해 현재시간(Long 타입)을 사용
            return@factory FactoryData(Calendar.getInstance().timeInMillis)
        }
    }

    override fun onCreate() {
        super.onCreate()
        startKoin(this, listOf(goodsModule)) //코인 시작!! listOf 에는 다른 모듈도 정의 한다면 뒤에 더 붙여서 사용
    }

}

//FactoryData 데이터 클래스
data class FactoryData(val timeInMillis: Long = 0L)

//GoodsData 데이터 클래스
data class GoodsData(var name : String = "청바지", var price : Long = 30000)

interface GoodsService {
    fun getGoodsInfo(): String
}

class GoodsServiceImpl(private val goodsData: GoodsData) : GoodsService {

    override fun getGoodsInfo(): String {
        //기본적으로 생성 되는 goodsData 정보는 상품명 청바지 , 가격 30000
        return "상품명 : " + goodsData.name + "  가격 : " + goodsData.price
    }
}

class MainActivity : AppCompatActivity() {
    private val goodsService: GoodsService by inject()  //by inject() 를 사용해서 GoodsService 주입
    private val goodsData: GoodsData by inject() //by inject() 를 사용해서 GoodsData 주입
    private val factoryData: FactoryData by inject() //by inject() 를 사용해서 FactoryData 주입

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }

}
```

#### Koin 사용 방법  
1. Application 부분에서 모듈 정의를 해주고 Application onCreate 시점에서 Startkoin을 호출 해주면서 사용 할 module 을 정의해준다.  
2. module을 정의 할 때 데이터를 어떤 방식으로 생성할지 따로 정의를 해준다  
   ex) factory , single , bind, get  
3. 실제 사용 되는 부분에서 주입 하기 위해 by inject() 를 통해 주입한다.  

### 다음 편에서는 Android 에서 사용되는 ViewModel에 의존성 주입하는 방법을 알아보자.