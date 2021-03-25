---
title:  "[Programmers] 경주로 건설"
search: true
categories: 
  - algorithm
tags:
  - Programmers
  - Java
  - Queue
  - BFS
last_modified_at: 2021-03-22T08:06:00-05:00
---

## 개요

2020 카카오 인턴쉽 문제

너비우선탐색을 이용해 코스트를 계산

[문제링크](https://programmers.co.kr/learn/courses/30/lessons/67259)


## 코드

```java
import java.util.LinkedList;
import java.util.Queue;

public class BuildRaceTrack {
    public int n;
    public int[][] track;
    public int[][] dir = { {-1,0},{1,0},{0,-1},{0,1} };
    public int solution(int[][] board) {
        n = board.length;
        track = board;
        int minCost = bfs(0, 0);

        return minCost;
    }

    public int bfs(int row, int col) {
        Queue<int[]> q = new LinkedList<>();
        q.add(new int[] {row,col,-1,0});    //(행,열,이전방향,코스트)
        int cost;
        int prev;
        while(!q.isEmpty()) {
            int[] temp = q.poll();
            cost = temp[3]; //코스트
            prev = temp[2]; //이전방향

            for(int i=0;i<dir.length;i++) { // i = 0:상 1:하 2:좌 3:우
                int r = dir[i][0] + temp[0];
                int c = dir[i][1] + temp[1];
    
                if(r < 0 || c < 0 || r >= n || c >= n) //배열의 범위를 벗어나는 경우
                    continue;

                if(track[r][c] != 1) {  //벽이 아닌 경우
                    int nextCost = (prev != -1 && prev != i) ? cost+600 : cost+100; //다음칸 코스트 계산
                    if(track[r][c] == 0 || track[r][c] >= nextCost) {   //처음방문하거나 기존코스트보다 계산한 코스트가 낮은 경우
                        track[r][c] = nextCost;
                        q.add(new int[] {r,c,i,nextCost});
                    }            
                }   
            }
        }
        return track[n-1][n-1];  //도착지점 코스트 저장
    }
}
```

## 해결 과정

처음에는 DFS를 사용해 코스트를 계산했는데 배열의 크기가 큰 테스트케이스를 통과하지 못했다. (시간초과)

그래서 BFS로 바꾸었는데 여기서 중요한 점은 코스트계산이다.

코스트를 계산할 때 이전방향이 중요한데 나는 탐색할 때 이전방향도 같이 확인해서 코스트를 계산하였다.

```java
int nextCost = (prev != -1 && prev != i) ? cost+600 : cost+100; //다음칸 코스트 계산
```

이전방향과 탐색할 방향이 다르면 600을 더해주고 같으면 100만 더해준다.

BFS로 탐색한 결과는 다음 이미지와 같다.

![image](https://user-images.githubusercontent.com/47655983/103513095-40e52580-4ead-11eb-8f48-e2644b26a5c8.png)

## 결과

![image](https://user-images.githubusercontent.com/47655983/103512658-79383400-4eac-11eb-932b-25fbc85c7836.png)
