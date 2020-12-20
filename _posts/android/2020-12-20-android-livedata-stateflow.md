---
layout: post
title: LiveData VS Flow State
description: LiveData VS Flow State
modified: 2020-12-20
tags: [Android, Kotlin]
categories: [Android, Kotlin]
---

오늘은 LiveData 와 Flow State 차이점을 알아보자.

### LiveData
1. Value 가 Nullable
   사용 시 널 체크 또는 liveData.value!! 사용해야함 (번거로움)
2. setValue 와 postValue
setValue 와 postValue 의 차이점을 알고 있어야 한다.

setValue 는 mainThread 에서 사용 해야한다.

postValue 는 background Thread , mainThread 모두 사용 가능

postValue 와 setValue 여러번 호출될 경우 생각한 대로 데이터 싱크가 맞지 않을 수 있음.

setValue 가 여러번 호출 되는 경우 모두 호출 되지만 postValue 는 연속으로 여러번 호출 되는 경우 최신 값이 호출됨.

### StateFlow
1. Value 가 NonNull
   사용 시 널 체크를 할 필요 없이 사용
2. Collect 사용 시 디폴트 값이 그대로 수집됨
3. Value 값이 변경된 값만 수집 됨.

```java
class MainActivity : AppCompatActivity() {
    lateinit var viewModel: MainActivityViewModel

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        viewModel = ViewModelProvider(this).get(MainActivityViewModel::class.java)
        viewModel.apply {
            testLiveData.observe(this@MainActivity, Observer { //liveData 데이터 수집
                Log.e("kyh!!!", "testLiveData : $it")
            })
            lifecycleScope.launch {
                testStateFlow.collect { //testStateFlow 데이터 수집
                    Log.e("kyh!!!", "testStateFlow : $it")
                }
            }

        }
        tvpostValueLiveData.setOnClickListener {
            viewModel.postValueLiveData()
        }
        tvsetValueLiveData.setOnClickListener {
            viewModel.setValueLiveData()
        }
        tvFlowState.setOnClickListener {
            viewModel.setValueFlowState()
        }

    }
}

class MainActivityViewModel : ViewModel(){
    val testLiveData = MutableLiveData<Boolean>()
    val testStateFlow = MutableStateFlow<Boolean>(false) //false 값이 처음 불림

    fun setValueLiveData() {// setValue 로 호출 했기 때문에 모든 값이 수집 된다.
        testLiveData.value = false
        testLiveData.value = false
        testLiveData.value = true
        testLiveData.value = false
    }

    fun postValueLiveData() {// postValue 로 호출 했기 때문에 제일 마지막에 호출 한 false 값이 호출 된다.
        testLiveData.postValue(false)
        testLiveData.postValue(false)
        testLiveData.postValue(true)
        testLiveData.postValue(false)
    }

    fun setValueFlowState(){// stateFlow 는 변경된 최신값을 호출한다. 기존 값이 false 였기 때문에 처음 세번은 수집 되지 않고 맨 뒤 true, false 수집 된다.
        testStateFlow.value = false
        testStateFlow.value = false
        testStateFlow.value = false
        testStateFlow.value = true
        testStateFlow.value = false
    }
}
```

### 주의 !!
A->B 화면 이동을 위한 LiveData 를 뷰모델에서 만들어서 사용하고 있었는데 그냥 StateFlow 로 변경 하면 화면 열리자 마자 A->B 화면 이동이 되버림.