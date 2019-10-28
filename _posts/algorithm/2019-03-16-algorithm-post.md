---
layout: post
title: 알고리즘 문제
description: "알고리즘 문제"
modified: 2019-03-16
tags: [자바,알고리즘]
categories: [알고리즘,자바]
---
직사각형 나머지 한 좌표 구하기 !!

직사각형을 만드는 데 필요한 4개의 점 중 3개의 좌표가 주어질 때, 나머지 한 점의 좌표를 구하려고 합니다. 점 3개의 좌표가 들어있는 배열 v가 매개변수로 주어질 때, 직사각형을 만드는 데 필요한 나머지 한 점의 좌표를 return 하도록 solution 함수를 완성해주세요. 단, 직사각형의 각 변은 x축, y축에 평행하며, 반드시 직사각형을 만들 수 있는 경우만 입력으로 주어집니다.

v는 세 점의 좌표가 들어있는 2차원 배열입니다.
v의 각 원소는 점의 좌표를 나타내며, 좌표는 [x축 좌표, y축 좌표] 순으로 주어집니다.
좌표값은 1 이상 10억 이하의 자연수입니다.
직사각형을 만드는 데 필요한 나머지 한 점의 좌표를 [x축 좌표, y축 좌표] 순으로 담아 return 해주세요.

입출력 예 설명
입출력 예 #1
세 점이 [1, 4], [3, 4], [3, 10] 위치에 있을 때, [1, 10]에 점이 위치하면 직사각형이 됩니다.

입출력 예 #2
세 점이 [1, 1], [2, 2], [1, 2] 위치에 있을 때, [2, 1]에 점이 위치하면 직사각형이 됩니다.



{% highlight ruby %}
import java.util.HashMap;
class Solution {
    public int[] solution(int[][] v) {
         HashMap<Integer, Integer> xHashMap = new HashMap<>();
        HashMap<Integer, Integer> yHashMap = new HashMap<>();

        for (int i = 0; i < v.length; i++) {
            xHashMap.put(v[i][0], xHashMap.getOrDefault(v[i][0], 0)+1);
            yHashMap.put(v[i][1], yHashMap.getOrDefault(v[i][1], 0)+1);
        }
        int x = 0;
        int y = 0;
        for (Integer key : xHashMap.keySet()) {
            if (xHashMap.get(key) == 1) {
                x = key;
            }
        }

        for (Integer key : yHashMap.keySet()) {
            if (yHashMap.get(key) == 1) {
                y = key;
            }
        }
        return new int[]{x, y};
    }
}
{% endhighlight %}




==========================================================

앞뒤를 뒤집어도 똑같은 문자를 palindrome(팰린드롬)이라고 합니다. 예를 들어 12321은 팰린드롬이며, 21453은 팰린드롬이 아닙니다.

자연수 n이 매개변수로 주어질 때, n이 팰린드롬이면 true를, 아니면 false를 반환하도록 함수 solution 을 완성하세요.

제한사항
n은 231 - 1 보다 작거나 같은 자연수입니다.

입출력 예
n	result
12321	true
21453	false
입출력 예 설명
입출력 예 #1
12321을 뒤집으면 12321이 되어 팰린드롬입니다.

입출력 예 #2
21453을 뒤집으면 35412가 되어 팰린드롬이 아닙니다.

class Solution {
    public boolean solution(int n) {
         int length = String.valueOf(n).length();
        String[] values = String.valueOf(n).split("");

        if (length % 2 == 0) {
            int center = (length / 2);
            for (int i = 0; i < center; i++) {
                if (!values[i].equals(values[length -i - 1])) {
                    return false;
                }
            }
            return true;
        } else {
            int center = (length / 2) + 1;
            for (int i = 0; i < center; i++) {
                if (!values[i].equals(values[length -i - 1])) {
                    return false;
                }
            }
            return true;
        }
    }
}

출처 : https://programmers.co.kr/tryouts/3923/challenges/11576