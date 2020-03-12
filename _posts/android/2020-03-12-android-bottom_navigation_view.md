---
layout: post
title: BottomNavigationView
description: "BottomNavigationView"
modified: 2020-03-12
tags: [BottomNavigationView]
categories: [android]
---

### BottomNavigationView in Design SupportLibrary : 
사용 하기 위해서는 다음과 같은 dependency 가 필요 하다.
implementation 'com.android.support:design:28.0.0'
버전은 현재의 최신 버전으로 설정 했다.

Badge 기능을 사용하고 싶다면 다음과 같이 사용할 수 있다.
{% highlight ruby %}
    private fun setBadge() {
        val bottomNavigationMenuView: BottomNavigationMenuView = bottomNavigationView.getChildAt(0) as BottomNavigationMenuView
        val bottomNavigationItemView  = bottomNavigationMenuView.getChildAt(0) as BottomNavigationItemView

        val badgeContainer = LayoutInflater.from(this).inflate(R.layout.view_badge, bottomNavigationMenuView, false)
        val firstBadgeCountText: TextView = badgeContainer.findViewById(R.id.count)
        firstBadgeCountText.text = "99"
        bottomNavigationItemView.addView(badgeContainer)
    }
{% endhighlight %}

BottomNavigationView의 getChildAt 0부터 첫번째 BottomNavigationItemView 다.
inflate 한 view 를 bottomNavigationItemView에 추가해서 사용하면 됨.

### BottomNavigationView in Material : 
사용하기 위해서는 다음과 같은 dependency 가 필요 하다.
implementation 'com.google.android.material:material:1.1.0-alpha06'

이 버전에서는 BadgeDrawable 이 BottomNavigationView 안에 구현 되어 있어서 showBadge, removeBadge api를 통해 간편하게 사용 할 수 있다.
1.1.0-alpha05 이하 버전에서는 BadgeDrawable 이 구현 되어 있지 않아 showBadge, removeBadge 를 사용할 수 없는 걸 확인 했다.

사용방법은 Badge 구현 빼고는 Support BottomNavigationView 와 비슷했다.

{% highlight ruby %}
    public void updateFirstBadge(int count) {
            if (count <= 0) {
                bottomNavigationView.removeBadge(R.id.title_first);
            } else {
                BadgeDrawable badgeDrawable = bottomNavigationView.showBadge(R.id.title_first);
                badgeDrawable.setBackgroundColor(getResources().getColor(R.color.red));
                badgeDrawable.setNumber(count);
            }
        }
{% endhighlight %}

2020년 03 월 12일 기준 최신 버전
implementation 'com.google.android.material:material:1.2.0-alpha05' 에서는 위의 showBade가 사라지고 getOrCreateBadge로 변경 되었다.  
적용하면 아래와 같다.  

{% highlight ruby %}
    public void updateFirstBadge(int count) {
            if (count <= 0) {
                bottomNavigationView.removeBadge(R.id.title_first);
            } else {
                BadgeDrawable badgeDrawable = bottomNavigationView.getOrCreateBadge(R.id.title_first);
                badgeDrawable.setBackgroundColor(getResources().getColor(R.color.red));
                badgeDrawable.setNumber(count);
            }
        }
{% endhighlight %}