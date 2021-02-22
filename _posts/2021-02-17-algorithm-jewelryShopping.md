---
title:  "[Programmers] 보석쇼핑"
search: true
categories: 
  - algorithm
tags:
  - Programmers
  - Java
  - HashMap
  - HashSet
  - Queue
last_modified_at: 2021-02-17T08:06:00-05:00
---

## 개요

2020 카카오 인턴십 문제

해쉬와 큐를 사용하여 해결

[문제링크](https://programmers.co.kr/learn/courses/30/lessons/67258)

## 코드

```java
import java.util.HashMap;
import java.util.HashSet;
import java.util.LinkedList;
import java.util.Queue;

public class JewelryShopping {
    public int[] solution(String[] gems) {
        int[] answer = new int[2];
        HashSet<String> set = new HashSet<>();  //보석의 종류를 저장해주는 HashSet
        HashMap<String,Integer> map = new HashMap<>();  //보석이 나온 갯수 카운트해주는 HashMap
        Queue<String> q = new LinkedList<>();   //슬라이싱해줄 큐
        int minLength = gems.length+1;          //최소길이
        int startPos = 0;                       //탐색하면서 저장되는 시작위치
        int startIndex = 0;                     //최종 시작위치

        for(int i=0;i<gems.length;i++)  //HashSet 데이터 저장 
            set.add(gems[i]);
        
        for(int i=0;i<gems.length;i++) {
            if(map.containsKey(gems[i]))    //hASHmAP에 데이터가 있으면
                map.put(gems[i], map.get(gems[i])+1);   //카운트 증가
            else
                map.put(gems[i], 1);    //카운트 1로 추가

            q.add(gems[i]); //슬라이싱 큐에 보석 저장

            while(true) {
                String temp = q.peek();
                if(map.get(temp) > 1) { //맵에 두개이상 카운트 되어있으면 큐에서 방출
                    map.put(temp, map.get(temp)-1);
                    q.poll();
                    startPos++; //시작지점 한칸 이동
                }
                else
                    break;
            }

            if(map.size() == set.size() && minLength > q.size()) {  //HashMap과 HashSet에 저장된 사이즈 같으면 보석이 다들어있음
                minLength = q.size();
                startIndex = startPos;
            }
        }
        answer[0] = startIndex + 1;         //시작지점
        answer[1] = startIndex + minLength; //끝지점
        
        return answer;
    }
}
```

## 해결 과정

처음에는 해쉬맵과 해쉬셋을 사용해 2중 반복문을 통해 모든 경우의 수를 살폈는데

입력값이 10만이나 되다보니 시간초과가 났다.

그래서 큐를 이용한 슬라이싱을 사용하게 되었고

방법은 다음과 같다

1. 해쉬셋에 모든 종류의 보석을 저장함

2. 해쉬맵은 키값으로 보석이름을 데이터값으로 숫자를 저장하는데 순회를 돌면서 중복된 보석이 들어오면 숫자를 하나씩 늘림

3. 순회할때마다 보석을 큐에 삽입하고 큐의 맨 앞( q.peek() )에 있는 보석을 해쉬맵에서 확인해서 카운트가 2이상이면 방출하고 카운트를 줄이는 것을 반복함

4. 해쉬맵과 해쉬셋의 사이즈가 같고 최소길이보다 큐의 길이가 작으면 보석이 한종류씩 다 있고 최소길이가 갱신된 것이므로 최소길이 갱신하고 시작지점 저장함

## 결과

![image](https://user-images.githubusercontent.com/47655983/103510736-24df8500-4ea9-11eb-9db8-a7f979a940d5.png)