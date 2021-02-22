---
title:  "[Programmers] 땅따먹기"
search: true
categories: 
  - algorithm
tags:
  - Programmers
  - Java
  - DP
last_modified_at: 2021-02-04T08:06:00-05:00
---

## 개요

![image](https://user-images.githubusercontent.com/47655983/106847602-9140e480-66f2-11eb-983d-e0d87f8555be.png)

다이나믹 프로그래밍을 이용하면 쉽게 풀이가 가능한 문제

[문제링크](https://programmers.co.kr/learn/courses/30/lessons/12913)

## 코드

```java
public class GroundPicking {
    public int solution(int[][] land) {
        int n = land.length;
        int[][] dp = new int[2][4];
        int row = 0;    //dp array row
        int max;
        int resultMax = 0;
        for (int i = 0; i < n; i++) {
            if (row == 0) {
                for (int j = 0; j < 4; j++) {
                    dp[0][j] = land[i][j];    //dp배열에 땅배열 값 저장
                    max = 0;
                    for (int k = 0; k < 4; k++) {   //다른 행 순회 dp[1]
                        if(k == j)  //같은 열에 있으면 건너 뜀
                            continue;
                        if (dp[1][k] > max) //행 최대값 갱신
                            max = dp[1][k];
                    }
                    dp[0][j] += max;  //다른 행의 최대값을 더해줌
                    if (dp[0][j] > resultMax)
                        resultMax = dp[0][j];
                }
                row = 1;    //행 번호 변경
            }
            else {  //row = 1
                for (int j = 0; j < 4; j++) {
                    dp[1][j] = land[i][j];
                    max = 0;
                    for (int k = 0; k < 4; k++) {   //다른 행 순회 dp[0]
                        if(k == j)  //같은 열에 있으면 건너 뜀
                            continue;
                        if (dp[0][k] > max) //행 최대값 갱신
                            max = dp[0][k];
                    }
                    dp[1][j] += max;  //다른 행의 최대값을 더해줌
                    if (dp[1][j] > resultMax) //결과 최대값 갱신
                        resultMax = dp[1][j];
                }
                row = 0;    //행 번호 변경
            }
        }
        return resultMax;
    }
}
```

## 해결 과정

2x4 크기의 dp 배열을 생성해준뒤에 땅 배열을 한행씩 계산해서 dp배열에 저장한다

dp배열 한칸의 값을 계산하는 알고리즘은 다음과 같다

- dp배열의 (0행 or 1행) j열에 땅배열 i행 j열 값을 넣는다.
- 열의 사이즈만큼 반복한다 (k = 0; k < 4; k++)
    - k가 j와 같지 않을 경우에만 행의 최대값을 찾아서 저장한다.
- dp배열 (0행 or 1행) j열에 행의 최대값을 더해준다. 그리고 최고점을 갱신한다.

![image](https://user-images.githubusercontent.com/47655983/106848935-3b217080-66f5-11eb-847f-6ecc362cbdf8.png)

![image](https://user-images.githubusercontent.com/47655983/106848991-4f656d80-66f5-11eb-8784-9fea6ac6d3a6.png)

![image](https://user-images.githubusercontent.com/47655983/106849802-b6375680-66f6-11eb-8f2d-ba6445e96b55.png)

![image](https://user-images.githubusercontent.com/47655983/106849877-d9620600-66f6-11eb-860b-a2ec07b96c77.png)

## 결과

![image](https://user-images.githubusercontent.com/47655983/106849908-e5e65e80-66f6-11eb-810a-08651e7d6f6d.png)