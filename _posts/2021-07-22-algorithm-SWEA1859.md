---
title:  "[SWEA] 1859번 백만 장자 프로젝트"
search: true
categories: 
  - algorithm
tags:
  - SWEA
  - Java
last_modified_at: 2021-07-22T08:06:00-05:00
---

## 개요

물건의 가격을 알고 있고 최대 이익을 얻기 위해서는 어떻게 사고 팔아야 하는지 정하는 문제

[문제링크](https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AV5LrsUaDxcDFAXc)


## 코드

```java
import java.io.*;
import java.util.StringTokenizer;

public class SWEA1859 {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        int t = Integer.parseInt(br.readLine());
        StringTokenizer st;

        for (int i = 0; i < t; i++) {
            int n = Integer.parseInt(br.readLine());
            st = new StringTokenizer(br.readLine());
            int[] arr = new int[n];

            for (int j = 0; j < n; j++) //배열에 가격 저장 
                arr[j] = Integer.parseInt(st.nextToken());
            
            long profit = 0;  //이익
            int maxPrice = 0; //최대가격
            for (int j = arr.length-1; j >= 0; j--) { //역순 순회
                if(arr[j] >= maxPrice) //현재인덱스가 최대가격보다 크거나 같으면
                    maxPrice = arr[j];  //최대 가격 갱신
                else  //현재인덱스가 최대가격보다 작으면
                    profit += maxPrice - arr[j];  //이익 추가
            }
            bw.write("#"+(i+1)+" "+profit);
            bw.newLine();
        }
        bw.flush();

    }
}
```

## 해결 과정

최대 이익을 얻기 위해서는 비교적 저렴한 날에는 구입하고 비싼 날에 몰아서 팔아야한다. 예를 들어 3일동안 `6 7 10`에 판다고 하면 6원, 7원일 때 사서 10일에 팔면 최대 이익을 얻을 수 있다.

하지만 3일동안 `10 7 6`에 판다고 하면 아예 구입을 하지 않는 것이 최대 이익이 된다.

이 방식을 알고리즘에 적용하려고 맨 처음 생각해본 것은 앞에서부터 차례대로 가격을 확인하면서 최대이익으로 팔 수 있는 가격을 발견하면 표시해두고 나중에 다시 순회하면서 이익을 계산하는 방식인데 많이 비효율적이라고 생각했다.

그래서 역순으로 계산하는 방법을 생각했는데 역순으로 할 경우 한번의 순회로 계산을 마칠 수 있다는 사실을 알았다.

역순 순회하면서 최대값을 계속 갱신해나가는데 현재 인덱스가 최대값보다 크거나 같으면 최대값만 갱신하고 작으면 최대값에서 현재 인덱스의 가격을 뺀 만큼 이익으로 더해주는 방식으로 풀이하였다.
