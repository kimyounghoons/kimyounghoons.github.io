---
layout: post
title: 알고리즘 문제
description: "알고리즘 문제"
modified: 2019-03-11
tags: [자바,알고리즘]
categories: [알고리즘,자바]
---
쇠막대기 알고리즘

여러 개의 쇠막대기를 레이저로 절단하려고 합니다. 효율적인 작업을 위해서 쇠막대기를 아래에서 위로 겹쳐 놓고, 레이저를 위에서 수직으로 발사하여 쇠막대기들을 자릅니다. 쇠막대기와 레이저의 배치는 다음 조건을 만족합니다.

- 쇠막대기는 자신보다 긴 쇠막대기 위에만 놓일 수 있습니다.
- 쇠막대기를 다른 쇠막대기 위에 놓는 경우 완전히 포함되도록 놓되, 끝점은 겹치지 않도록 놓습니다.
- 각 쇠막대기를 자르는 레이저는 적어도 하나 존재합니다.
- 레이저는 어떤 쇠막대기의 양 끝점과도 겹치지 않습니다.

(a) 레이저는 여는 괄호와 닫는 괄호의 인접한 쌍 '()'으로 표현합니다. 또한 모든 '()'는 반드시 레이저를 표현합니다.
(b) 쇠막대기의 왼쪽 끝은 여는 괄호 '('로, 오른쪽 끝은 닫힌 괄호 ')'로 표현됩니다.

위 예의 괄호 표현은 그림 위에 주어져 있습니다.
쇠막대기는 레이저에 의해 몇 개의 조각으로 잘리는데, 위 예에서 가장 위에 있는 두 개의 쇠막대기는 각각 3개와 2개의 조각으로 잘리고, 이와 같은 방식으로 주어진 쇠막대기들은 총 17개의 조각으로 잘립니다.

쇠막대기와 레이저의 배치를 표현한 문자열 arrangement가 매개변수로 주어질 때, 잘린 쇠막대기 조각의 총 개수를 return 하도록 solution 함수를 작성해주세요.

제한사항
arrangement의 길이는 최대 100,000입니다.
arrangement의 여는 괄호와 닫는 괄호는 항상 쌍을 이룹니다.

{% highlight ruby %}
import java.util.*;
class Solution {
    public int solution(String arrangement) {
      int answer = 0;
        int count = 0;
        String[] array = arrangement.split("");
        Stack<String> stack = new Stack<>();
        for (String character : array) {
            if (character.equals("(")) {
                stack.push(character);
                count++;
            } else {
                if (stack.peek().equals("(")) {//레이저
                    stack.push(character);
                    --count;
                    answer += count;
                } else {//막대 끝
                    answer += 1;
                    count--;
                }
            }
        }
        return answer;
    }
}
{% endhighlight %}

여기서 포인트는  stack 을 사용하면서 레이저와 막대끝을 구분하는 것 같다 ㅎㅎ
알고리즘하니깐 시간 잘 가네 ㅎㅎ

출처 : https://programmers.co.kr/learn/courses/30/lessons/42585