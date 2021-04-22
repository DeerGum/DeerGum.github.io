---
title:  "[Programmers] 키패드 누르기"
search: true
categories: 
  - algorithm
tags:
  - Programmers
  - Java
  - Implementation
last_modified_at: 2021-04-01T08:06:00-05:00
---

## 개요

2020 카카오 인턴쉽 문제

누르려는 번호가 양손과 얼마나 떨어져있는지 확인하여 풀이하는 문제

[문제링크](https://programmers.co.kr/learn/courses/30/lessons/67256)


## 코드

```java
public class KeyPad {
    
    public String solution(int[] numbers, String hand) {
        StringBuilder result = new StringBuilder();
        int[][] keyPad = { {3,1},{0,0},{0,1},{0,2},{1,0},{1,1},{1,2},{2,0},{2,1},{2,2} }; //키패드 위치 저장
        int[] left = {3,0}; //왼손 시작지점
        int[] right = {3,2};//오른손 시작지점
        int r, c;
        boolean isLeft = false;
        for(int i=0;i<numbers.length;i++) {
            r = keyPad[numbers[i]][0];  //행
            c = keyPad[numbers[i]][1];  //열

            if(numbers[i] == 1 || numbers[i] == 4 || numbers[i] == 7) //왼손만 누르는 키
                isLeft = true;
            else if(numbers[i] == 3 || numbers[i] == 6 || numbers[i] == 9) //오른손만 누르는 키
                isLeft = false;               
            else {  //더 가까운 손이 누르는 키
                int leftRange = Math.abs(r-left[0]) + Math.abs(c-left[1]);  //왼손과 키 사이의 거리 
                int rightRange = Math.abs(r-right[0]) + Math.abs(c-right[1]);//오른손과 키 사이의 거리

                if(leftRange < rightRange) {    //왼손이 더 가까울 때
                    isLeft = true;
                }
                else if(leftRange == rightRange) {  //양손 거리가 같을 때
                    if(hand.equals("left")) //왼손잡이 경우
                        isLeft = true;
                    else                    //오른손잡이 경우
                        isLeft = false;
                }
                else    //오른손이 더 가까울 때
                    isLeft = false;
            }

            if(isLeft) {    //왼손으로 키 누름
                left[0] = r;
                left[1] = c;
                result.append("L");
            }
            else {          //오른손으로 키 누름
                right[0] = r;
                right[1] = c;
                result.append("R");
            }
        }

        return result.toString();
    }

}
```

## 해결 과정

![image](https://user-images.githubusercontent.com/47655983/103507340-45f0a780-4ea2-11eb-9d0c-27462972a7c8.png)

키패드를 2차원 배열로 저장한다 그리고 입력으로 받는 숫자를 키패드배열에 매핑시켜서 손의 위치를 저장하는 방식으로 진행하였다.

문제는 중간에 위치한 키들인데(2,5,8,0) 해당 키에 더 가까운 손이 있으면 그 손을 사용하고 양 손 다 거리가 같을 경우는 어느손잡이인지에 따라 처리해준다.

## 결과

![image](https://user-images.githubusercontent.com/47655983/103507649-01194080-4ea3-11eb-8cce-7e2b72e3a672.png)
