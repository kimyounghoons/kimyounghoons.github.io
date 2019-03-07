---
layout: post
title: 알고리즘 문제
description: "알고리즘 문제"
modified: 2019-03-07
tags: [자바,알고리즘]
categories: [알고리즘,자바]
---

알고리즘 공부 시작!!!

문제 설명
수많은 마라톤 선수들이 마라톤에 참여하였습니다. 단 한 명의 선수를 제외하고는 모든 선수가 마라톤을 완주하였습니다.

마라톤에 참여한 선수들의 이름이 담긴 배열 participant와 완주한 선수들의 이름이 담긴 배열 completion이 주어질 때, 완주하지 못한 선수의 이름을 return 하도록 solution 함수를 작성해주세요.

제한사항
마라톤 경기에 참여한 선수의 수는 1명 이상 100,000명 이하입니다.
completion의 길이는 participant의 길이보다 1 작습니다.
참가자의 이름은 1개 이상 20개 이하의 알파벳 소문자로 이루어져 있습니다.
참가자 중에는 동명이인이 있을 수 있습니다.

입출력 예 설명
예제 #1
leo는 참여자 명단에는 있지만, 완주자 명단에는 없기 때문에 완주하지 못했습니다.

예제 #2
vinko는 참여자 명단에는 있지만, 완주자 명단에는 없기 때문에 완주하지 못했습니다.

예제 #3
mislav는 참여자 명단에는 두 명이 있지만, 완주자 명단에는 한 명밖에 없기 때문에 한명은 완주하지 못했습니다.

{% highlight ruby %}
   class Solution {
    public String solution(String[] participant, String[] completion) {
        Arrays.sort(participant);
        Arrays.sort(completion);

        HashMap<Integer, String> m = new HashMap<>();
        for (int i = 0; i < participant.length; i++) {
            m.put(i, participant[i]);
        }


        for (int i = 0; i < completion.length; i++) {
            if(!participant[i].equals(completion[i])){
                return participant[i];
            }
        }

        Set<Integer> keys = m.keySet();
        String answer = "";
        for (Integer key : keys) {
            answer = m.get(key);
        }
        return answer;
    }
}
{% endhighlight %}

처음 문제 풀었을 때 정확성은 100 인데 효율성이 꽝이라 고민을 많이 하게 되었다.. 
O(N) 으로 풀어야 하는 문제였다.;;
두 배열을 sort 하고 참여자와 완주자가 다른 순간 값을 리턴시켜주면 되는 문제.
동명이인도 있기 때문에 HashMap 을 사용하였다.

출처 : https://programmers.co.kr/learn/courses/30/lessons/42576?language=java