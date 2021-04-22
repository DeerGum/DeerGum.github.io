---
title:  "[Programmers] 기둥과 보 설치"
search: true
categories: 
  - algorithm
tags:
  - Programmers
  - Java
  - Implementation
last_modified_at: 2021-04-22T08:06:00-05:00
---

## 개요

2020 카카오 공채 문제

주어진 알고리즘의 구현을 보는 문제이다.

[문제링크](https://programmers.co.kr/learn/courses/30/lessons/60061)


## 코드

```java
import java.util.ArrayList;

public class BuildColumnAndBeam {
    public int[][] solution(int n, int[][] build_frame) {
        Structure[][] board = new Structure[n+1][n+1];
        ArrayList<int[]> result = new ArrayList<>();

        for (int i = 0; i < board.length; i++)  //초기화
            for (int j = 0; j < board[i].length; j++) 
                board[i][j] = new Structure();
        
        for (int i = 0; i < build_frame.length; i++) {
            int x = build_frame[i][0];
            int y = build_frame[i][1];
            int s = build_frame[i][2];
            int action = build_frame[i][3];
            
            if(action == 1) {   //설치
                if(s == 0) {    //기둥
                    if(isValidColumn(board, x, y)) 
                        board[x][y].setColumn(true);
                }
                else {  //보
                    if(isValidBeam(board, x, y, n)) 
                        board[x][y].setBeam(true);
                }
            }
            else {  //삭제
                if(s == 0) {    //기둥
                    board[x][y].setColumn(false);   //일단 삭제
                    if(!isCheckBoard(board, n))   //현재 설치된 건축물 유효한지 모두 확인
                        board[x][y].setColumn(true);    //조건을 충족 못시켜서 삭제 취소
                }
                else {  //보
                    board[x][y].setBeam(false); //일단 삭제
                    if(!isCheckBoard(board, n))   //현재 설치된 건축물 유효한지 모두 확인
                        board[x][y].setBeam(true);    //조건을 충족 못시켜서 삭제 취소
                }
            }
        }

        for (int i = 0; i < board.length; i++) {    //결과 배열 생성
            for (int j = 0; j < board[i].length; j++) {
                if(board[i][j].isColumn()) 
                    result.add(new int[] {i,j,0});
                if(board[i][j].isBeam()) 
                    result.add(new int[] {i,j,1});
            }
        }

        return result.toArray(new int[0][0]);
    }
    
    public boolean isValidColumn(Structure[][] board, int x, int y) {   //기둥 유효성 확인
        if(y == 0)      //바닥 위일 경우
            return true;
        if(board[x][y].isBeam())    //보의 한쪽 끝 부분 위
            return true;
        if(x != 0 && board[x-1][y].isBeam())   //보의 한쪽 끝 부분 위
            return true;
        if(board[x][y-1].isColumn())    //다른 기둥 위
            return true;

        return false;
    }

    public boolean isValidBeam(Structure[][] board, int x, int y, int n) {  //보 유효성 확인
        if(board[x][y-1].isColumn() || board[x+1][y-1].isColumn()) 
            //한쪽 끝 부분이 기둥 위
            return true;
        if(x > 0 && (board[x-1][y].isBeam() && board[x+1][y].isBeam())) 
            //양쪽 끝 부분이 다른 보와 동시에 연결
            return true;
        return false;
    }

    public boolean isCheckBoard(Structure[][] board, int n) {   //전체 구조물 유효성 확인
        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board.length; j++) {
                if(board[i][j].isBeam() && !isValidBeam(board, i, j, n))
                    return false;
                if(board[i][j].isColumn() && !isValidColumn(board, i, j))
                    return false;
            }
        }
        return true;
    }

}

class Structure {
    private boolean column;    //기둥
    private boolean beam;      //보

    public Structure() {
        this.column = false;
        this.beam = false;
    }

    public boolean isColumn() {
        return column;
    }

    public void setColumn(boolean column) {
        this.column = column;
    }

    public boolean isBeam() {
        return beam;
    }

    public void setBeam(boolean beam) {
        this.beam = beam;
    }
    
}
```

## 해결 과정

### 기둥 조건
- 바닥 위
- 보의 한쪽 끝 부분 위
- 다른 기둥 위

### 보 조건
- 한쪽 끝 부분이 기둥 위
- 양쪽 끝 부분이 다른 보와 동시에 연결

구조물을 설치할 때는 위의 조건에 맞춰서 설치하면 되고 

삭제할 때는 구조물을 삭제한 후에 전체 구조물이 위의 조건에 모두 부합하는지 확인해서 부합하지 않으면 삭제한 것을 되돌리는 방식으로 구현하였다.

## 결과

![image](https://user-images.githubusercontent.com/47655983/104871670-0b513980-598f-11eb-9125-64daa884c93e.png)