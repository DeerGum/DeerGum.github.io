---
title:  "[Programmers] 메뉴리뉴얼"
search: true
categories: 
  - algorithm
tags:
  - Programmers
  - Java
  - String
  - BruteForce
  - HashMap
last_modified_at: 2021-02-11T08:06:00-05:00
---

# [Programmers] 메뉴 리뉴얼

## 개요
---

2021 kakao 공채 2번 문제

브루트포스 알고리즘을 사용해서 해결

[문제링크](https://programmers.co.kr/learn/courses/30/lessons/72411)

## 코드

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.HashMap;

public class MenuRenewal {
    HashMap<String, Integer>[] hashMaps;    //메뉴 경우의 수 카운트
    int[] course;
    int[] maxCount; //코스요리 갯수별로 최대값

    public String[] solution(String[] orders, int[] course) {
        hashMaps = new HashMap[course.length];
        for (int i = 0; i < hashMaps.length; i++)
            hashMaps[i] = new HashMap<>();
        this.course = course;
        maxCount = new int[course.length];

        char[] order;

        for (int i = 0; i < orders.length; i++) {   //주문을 알파벳순으로 정렬
            order = orders[i].toCharArray();
            Arrays.sort(order);
            orders[i] = String.valueOf(order);
        }

        for (int i = 0; i < orders.length; i++) {   //주문 경우의 수 해쉬맵에 저장
            order = orders[i].toCharArray();
            for (int j = 0; j < course.length; j++) {
                courseCase(order, j, 0, 0,"");
            }
        }

        ArrayList<String> result = new ArrayList<>();
        for (int i=0;i<hashMaps.length;i++) {   //해쉬맵에 저장된 최대 주문값을 갖는 주문을 리스트에 저장
            int finalI = i;
            hashMaps[i].forEach((key, value) -> {
                    if (value == maxCount[finalI]) {
                        result.add(key);
                    }
            });
        }
        String[] answer = result.toArray(new String[0]);
        Arrays.sort(answer);
        return answer;
    }

    public void courseCase(char[] order, int courseIdx, int idx, int size, String str) {
        if (size == course[courseIdx]) { //재귀 탈출조건
            if (hashMaps[courseIdx].containsKey(str)) {
                int count = hashMaps[courseIdx].get(str);
                hashMaps[courseIdx].put(str,count+1);
                if (maxCount[courseIdx] < count+1)
                    maxCount[courseIdx] = count+1;
            }
            else
                hashMaps[courseIdx].put(str,1);
            return;
        }

        if (order.length < course[courseIdx])   //주문 갯수가 코스요리 갯수보다 작으면 리턴
            return;

        StringBuilder sb = new StringBuilder(str);
        for(int i = idx ;i < order.length; i++) {   //경우의 수 계산
            sb.append(order[i]);    //문자열에 문자추가
            courseCase(order,courseIdx,i+1,size+1,sb.toString());
            sb.deleteCharAt(sb.length()-1); //재귀가 끝나고 문자 제거
        }
    }
}
```

## 해결 과정

입력값이 크지 않았기 때문에 브루트포스 알고리즘으로 해결하였다.(모든 경우의 수 확인)

주문 배열의 주문을 알파벳순으로 정렬해준 뒤에 해쉬맵에 주문의 경우의 수를 카운트해서 저장하고 코스 갯수별로 제일 많이 나온 경우의 수를 리스트에 저장해서 반환해주었다.

## 결과

![image](https://user-images.githubusercontent.com/47655983/107641097-866bee00-6cb6-11eb-8b7f-83fe2fc4070b.png)
