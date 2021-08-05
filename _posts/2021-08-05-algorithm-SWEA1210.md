---
title:  "[SWEA] 1210번 Ladder1"
search: true
categories: 
  - algorithm
tags:
  - SWEA
  - Java
  - Simulation
  - Implementation
last_modified_at: 2021-08-05T08:06:00-05:00
---

## 개요

2차원배열에 주어진 사다리타기를 해서 당첨에 걸리는 시작지점을 반환하는 문제

[문제링크](https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AV14ABYKADACFAYh&categoryId=AV14ABYKADACFAYh&categoryType=CODE&problemTitle=ladder&orderBy=FIRST_REG_DATETIME&selectCodeLang=ALL&select-1=&pageSize=10&pageIndex=1&&&&&&&&&)


## 코드1

```java
import java.io.*;
import java.util.StringTokenizer;

public class SWEA1210 {
    public static int[][] ladder;
    public static boolean[][] visited;
    public static int[][] dir = {{0,-1},{0,1},{1,0}};   //왼쪽 오른쪽 아래
    public static int win;
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        StringTokenizer st;

        for (int t = 1; t <= 10; t++) {
            int tc = Integer.parseInt(br.readLine());
            ladder = new int[100][100];
            win = -1;

            for (int i = 0; i < ladder.length; i++) {
                st = new StringTokenizer(br.readLine());
                for (int j = 0; j < ladder[i].length; j++) {
                    ladder[i][j] = Integer.parseInt(st.nextToken());
                }
            }

            for (int i = 0; i < ladder[0].length; i++) {
                if (ladder[0][i] == 1) {
                    visited = new boolean[100][100];
                    search(0, i, i);
                }
            }

            bw.write("#"+tc+" "+win+"\n");
        }
        bw.flush();
    }

    public static void search(int r, int c, int startPos) {
        if (ladder[r][c] == 2) {
            win = startPos;
            return;
        }
        visited[r][c] = true;

        for (int i = 0; i < dir.length; i++) {
            int R = r + dir[i][0];
            int C = c + dir[i][1];

            if (R < 0 || R >= 100 || C < 0 || C >= 100 || ladder[R][C] == 0 || visited[R][C]) 
                continue;

            search(R, C, startPos);
            break;  //한방향으로 이동했으면 반복문 종료
        }
    }
}
```

## 코드2

```java
import java.io.*;
import java.util.StringTokenizer;

public class SWEA1210_2 {
    public static int[][] ladder;
    public static boolean[][] visited;
    public static int[][] dir = {{0,-1},{0,1},{-1,0}};   //왼쪽 오른쪽 위
    public static int win;
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        StringTokenizer st;

        for (int t = 1; t <= 10; t++) {
            int tc = Integer.parseInt(br.readLine());
            ladder = new int[100][100];
            win = -1;
            int startRow = -1;
            int startCol = -1;
            for (int i = 0; i < ladder.length; i++) {
                st = new StringTokenizer(br.readLine());
                for (int j = 0; j < ladder[i].length; j++) {
                    ladder[i][j] = Integer.parseInt(st.nextToken());
                    if (ladder[i][j] == 2) {
                        startRow = i;
                        startCol = j;
                    }
                    if (i == 0 && ladder[i][j] == 1) {  // 시작지점 매핑
                        ladder[i][j] = 3;
                    }
                }
            }
            visited = new boolean[100][100];
            search(startRow, startCol);
            

            bw.write("#"+tc+" "+win+"\n");
        }
        bw.flush();
    }

    public static void search(int r, int c) {
        if (ladder[r][c] == 3) {    //시작지점을 만나면 끝
            win = c;
            return;
        }
        visited[r][c] = true;

        for (int i = 0; i < dir.length; i++) {
            int R = r + dir[i][0];
            int C = c + dir[i][1];

            if (R < 0 || R >= 100 || C < 0 || C >= 100 || ladder[R][C] == 0 || visited[R][C]) 
                continue;

            search(R, C);
            break;  //한방향으로 이동했으면 반복문 종료
        }
    }
}
```
## 해결 과정

처음에는 배열의 0번째 행에서 시작지점마다 사다리타기를 진행하고 당첨이 나오면 종료하는 방식으로 간단하게 생각했지만 코드를 짜고 나니 당첨지점부터 역으로 올라가면 더욱 효율적으로 문제를 풀 수 있겠다는 생각이 들었다.

시작지점부터 사다리 타기를 하면 최악의 경우 사다리 갯수만큼 반복해야 하지만 당첨지점부터 탐색하면 한번만 탐색하면 끝나기 때문이다.

코드1은 시작지점부터 사다리 타기를 한 코드이고 코드2는 당첨지점부터 사다리 타기를 한 코드이다.

코드1에서 사다리 타기를 할땐 탐색방향을 왼쪽, 오른쪽, 아래 순으로 살펴보면서 갈 수 있는 방향을 발견하면 한번만 이동하는 알고리즘이 적용되었고

코드2에서는 탐색방향을 왼쪽, 오른쪽, 위 순으로 살펴보면서 갈 수 있는 방향을 발견하면 이동하게 했다. 그리고 시작지점을 따로 매핑해 탐색을 끝낼야 하는 지점을 기억해두었다.

