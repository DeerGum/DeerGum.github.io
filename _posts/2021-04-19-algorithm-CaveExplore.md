---
title:  "[Programmers] 동굴 탐험"
search: true
categories: 
  - algorithm
tags:
  - Programmers
  - Java
  - Stack
  - Graph
  - DFS
last_modified_at: 2021-04-19T08:06:00-05:00
---

## 개요

2020 카카오 인턴쉽 문제

미리 방문해야 하는 방과 다음에 방문해야 할 방을 기억해서 탐색해야 한다.

[문제링크](https://programmers.co.kr/learn/courses/30/lessons/67260)


## 코드

```java
import java.util.ArrayList;
import java.util.Stack;

public class CaveExplore {
    public boolean solution(int n, int[][] path, int[][] order) {
        int[] prev = new int[n];    //미리 방문해야 하는 방
        int[] next = new int[n];    //다음에 방문할 방
        boolean[] visited = new boolean[n];
        ArrayList<Integer>[] roomPath = new ArrayList[n];
        Stack<Integer> s = new Stack<>();

        for (int i = 0; i < roomPath.length; i++) //초기화
            roomPath[i] = new ArrayList<>();
        
        for (int i = 0; i < path.length; i++) { //양방향 경로 저장
            int prevRoom = path[i][0];
            int nextRoom = path[i][1];
            
            roomPath[prevRoom].add(nextRoom);
            roomPath[nextRoom].add(prevRoom);
        }

        for (int i = 0; i < order.length; i++)  //이전에 방문해야하는 방들 저장
            prev[order[i][1]] = order[i][0];

        if(prev[0] != 0)    //0번방 이전에 방문해야 하는 방이 있으면 false
            return false;
        
        visited[0] = true;  //0번방에서 시작
        for (int i = 0; i < roomPath[0].size(); i++) //0번방과 직접 연결된 방들을 스택에 삽입
            s.add(roomPath[0].get(i));
        
        while(!s.isEmpty()) {
            int now = s.pop();
            int prevVisitedRoom = prev[now];    //현재 방 이전에 방문했어야 하는 방
            if(visited[now])    //현재 방을 방문한 적 있음
                continue;
            else if(!visited[prevVisitedRoom]) {  //현재 방 이전에 방문했어야 하는 방을 가본 적 없음
                next[prevVisitedRoom] = now;      //방문해야(next) 하는 방 목록에 현재 방 저장
                continue;
            }  

            visited[now] = true;    //현재 방 방문 표시

            for (int i = 0; i < roomPath[now].size(); i++) 
                if(!visited[roomPath[now].get(i)])
                    s.add(roomPath[now].get(i));    //현재 방과 직접 연결된 방 중에 방문 안한 방 삽입
            
            if(next[now] != 0)
                s.add(next[now]);   //현재 방 방문 이후에 갈 수 있는 방 삽입
        }

        for (int i = 0; i < n; i++)     //하나라도 방문한 적 없으면 false
            if(!visited[i])
                return false;
        
        return true;
    }
}
```

## 해결 과정

[https://jellyinghead.tistory.com/41](https://jellyinghead.tistory.com/41)

위의 블로그 참고해서 해결함

### 스택을 이용한 풀이

방마다 2개의 값을 저장함 

- 먼저 방문해야 하는 방 (prev)
- 이 방을 방문하고 갈 수 있는 방 (next)

`prev[a] = b`  : a를 방문하기 이전에 b를 방문해야 함

`next[b] = a`  : b를 방문하고 a를 방문해야 함

알고리즘

- 0번 방을 방문하고 0번 방과 직접 연결된 방들을 스택에 삽입
- 스택에 아무것도 없을 때까지 반복
    - 스택을 pop (현재 방)
    - 현재 방이 방문한 방이면 continue
    - 현재 방을 방문(visited)하지 않았지만 방문해야 하는 방(prev)에 가본 적이 없으면 next에 넣고 continue
    - 현재 방 방문(visited)
    - 스택에 현재 방에서 직접 연결된 방들 모두 삽입
    - 스택에 현재 방 방문 이후에 갈 수 있는 방(next[now]) 삽입
- 만약 한번도 방문하지 않은 방이 있으면 false 반환
- 모두 방문했으면 true 반환

## 결과

![image](https://user-images.githubusercontent.com/47655983/104799427-614ca280-5812-11eb-92e9-2c10a988a52c.png)