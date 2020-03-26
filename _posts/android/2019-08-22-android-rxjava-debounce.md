---
layout: post
title: 카운트 연속 클릭 시 마지막 카운트 api 호출 상황 
description: "카운트 연속 클릭 시 마지막 카운트 api 호출 상황"
modified: 2019-08-22
tags: [rxJava2,rxAndroid,rxKotlin,debounce]
categories: [android]
---
RxKotlin을 사용하지 않고 처리하려면 까다로운 처리인데...  
RxKotlin을 사용하면 간단하게 처리 할 수 있다.  

```
class MainActivity : AppCompatActivity() {

    var count = 0
    val compositeDisposable: CompositeDisposable = CompositeDisposable()
    var publishSubject: PublishSubject<String> = PublishSubject.create()

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        button.setOnClickListener {
            textView.text = (++count).toString()
            publishSubject.onNext(count.toString())
            Log.d("kyh", "count : $count")
        }

        compositeDisposable.add(publishSubject
            .debounce(500, TimeUnit.MILLISECONDS)
            .observeOn(AndroidSchedulers.mainThread())
            .doOnError {
                Toast.makeText(this, it.message, Toast.LENGTH_LONG).show()
            }
            .subscribe { string ->
                Toast.makeText(this, "publishSubject 사용~! api에 보낼 카운트 : $string", Toast.LENGTH_LONG).show()
            })

        compositeDisposable.add(RxTextView.textChanges(textView)
            .debounce(300, TimeUnit.MILLISECONDS)
            .map { charSequence -> charSequence.toString() }
            .filter {
                !it.equals("Hello World!")
            }
            .observeOn(AndroidSchedulers.mainThread())
            .doOnError {
                Toast.makeText(this, it.message, Toast.LENGTH_LONG).show()
            }
            .subscribe { string ->
                Toast.makeText(this, "RxBinding 사용~! api에 보낼 카운트 : $string", Toast.LENGTH_LONG).show()
            }
        )
    }

    override fun onDestroy() {
        super.onDestroy()
        compositeDisposable.dispose()
    }
}
```

debounce 를 사용하면 편하게 처리할 수 있다.
위의 소스는 PublishSubject 와 RxBidning Library의 RxTextView 를 사용한 예제 소스

둘중 마음에 드는 걸로 사용하면 됨

rx 문서에서는 debounce 를   
only emit an item from an Observable if a particular timespan has passed without it emitting another item    
라고 소개 한다.

아이템이 들어오다가 지정한 시간동안 아이템이 들어오지 않는 경우 마지막 아이템을 subscribe 를 통해 값을 보내준다.
rx 는 마블을 보고 이해 하는게 제일 좋은 것 같다. 직관적으로 이해가 잘됨.

<figure>
	<img src="/images/2019-08-22-android-rxjava-debounce.png" alt="">
	<figcaption>출처 http://reactivex.io/documentation/operators/debounce.html</figcaption>
</figure>