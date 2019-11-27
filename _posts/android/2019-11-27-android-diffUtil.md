---
layout: post
title: RecyclerView DiffUtil 
description: "RecyclerView DiffUtil "
modified: 2019-11-27
tags: [DiffUtil,RecyclerView]
categories: [android]
---

oldList 와 newList 리스트의 차이를 계산하고 oldList를 newList로 변환하는 업데이트 작업 목록을 출력 할 수있는 유틸리티 클래스이다.  
업데이트 작업 목록은 newList 가 Insert , Remove , Update 다 포함 된다.

### 리스트 아이템이 변경 되는 갯수 마다의 시간 데이터

* 100 items and 10 modifications: avg: 0.39 ms, median: 0.35 ms
* 100 items and 100 modifications: 3.82 ms, median: 3.75 ms
* 100 items and 100 modifications without moves: 2.09 ms, median: 2.06 ms
* 1000 items and 50 modifications: avg: 4.67 ms, median: 4.59 ms
* 1000 items and 50 modifications without moves: avg: 3.59 ms, median: 3.50 ms
* 1000 items and 200 modifications: 27.07 ms, median: 26.92 ms
* 1000 items and 200 modifications without moves: 13.54 ms, median: 13.36 ms

  
Eugene W. Myers의 차이 알고리즘을 사용하여 하나의 목록을 다른 목록으로 변환하기위한 최소 업데이트 수를 계산한다.

사용법을 알아보자~!

우선 아래와 같이 DiffUtil.Callback 을 상속받는 DiffCallback 클래스를 만들고 비교할 대상을 지정 해준다.
```
class DiffCallback(
		private val oldData: AdapterData,
		private val newData: AdapterData
	) : DiffUtil.Callback() {

	override fun getOldListSize() = oldData.size

	override fun getNewListSize() = newData.size

	override fun areItemsTheSame(oldItemPosition: Int, newItemPosition: Int)
     = oldData[oldItemPosition].id == newData[newItemPosition].id

	override fun areContentsTheSame(oldItemPosition: Int, newItemPosition: Int)
     = oldData[oldItemPosition] == newData[newItemPosition]

}
```

다음으로는 RecyclerView Adapter 클래스의 setData 를 보자  

```
fun setData(newData: AdapterData) {
	val diffResult = DiffUtil.calculateDiff(DiffCallback(data, newData))
	data = newData
	diffResult.dispatchUpdatesTo(this)
}
```
  
DiffUtil.calculateDiff 는 DiffCallback 을 인자로 받고 결과 값으로 DiffUtil.DiffResult 를 반환 한다.  
그 다음 RecyclerView Adapter 의 아이템을 대체 시켜 주고 dispatchUpdatesTo 를 호출한다.  여기서 this는 RecyclerView Adapter 이다.  
RecyclerView Adapter 를 파라미터로 받는 이유는 내부적으로 dispatchUpdatesTo 를 호출 하는 순간 뷰가 갱신 된다.  
그렇기 때문에 다음 4가지 중 한가지를 호출 하기 위해서라고 판단 된다.
* mAdapter.notifyItemRangeInserted(position, count);  
* mAdapter.notifyItemRangeRemoved(position, count);  
* mAdapter.notifyItemMoved(fromPosition, toPosition);  
* mAdapter.notifyItemRangeChanged(position, count, payload);  

기존 방식은 OldList 에서 변경 된 포지션을 직접 위의 4가지중 한가지를 불러서 뷰를 갱신 시켜줬어야 하는데 DiffUtil 을 사용하면 개발자가 신경 쓰지 않아도 편리하게 수정 할수 있어서 좋은 것 같다.

### 주의 사항
1. 리스트 사이즈가 큰 경우는 DiffResult를 가져올때 Background Thread 에서 실행 하고 뷰 갱신 할때만 메인스레드에서 실행 시켜야한다.
2. DiffUtil 사용 할 때 리스트의 Max Size 는 2^26 이다.
