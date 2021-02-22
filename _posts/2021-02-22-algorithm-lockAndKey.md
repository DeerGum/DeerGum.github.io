---
title:  "[Programmers] 자물쇠와 열쇠"
search: true
categories: 
  - algorithm
tags:
  - Programmers
  - Java
  - Array
last_modified_at: 2021-02-22T08:06:00-05:00
---

## 개요

2020 카카오 공채 문제

2차원 배열인 자물쇠와 키를 잘 조합해서 자물쇠의 값을 모두 1로 만드는 문제

[문제링크](https://programmers.co.kr/learn/courses/30/lessons/60059)


## 코드

```java
class LockAndKey {
    public boolean solution(int[][] key, int[][] lock) {
        int n = lock.length;
        int m = key.length;
        int len = n+2*(m-1);
        int[][] board = new int[len][len];
        int[][] temp = new int[len][len];
        int range = n+m-1;

        for(int i=0;i<n;i++)   //보드배열 중앙에 자물쇠 값 저장
            for(int j=0;j<n;j++) 
                board[m-1+i][m-1+j] = lock[i][j];

        temp = arrayCopy(board);    //계산할 때 사용할 배열 복사
        
        if(isUnlock(temp, m-1, m-1, m)) //처음부터 자물쇠가 풀려있는 경우
            return true;

        int rotation = 0;
        do {    //회전을 위한 반복문
            
            for(int i=0;i<range;i++) {
                for(int j=0;j<range;j++) {
                    
                    for(int row=0;row<m;row++)     //키값 대입
                        for(int col=0;col<m;col++) 
                            temp[i+row][j+col] ^= key[row][col];    //xor 계산
                
                    if(isUnlock(temp, m-1, m-1, n)) //계산한 값이 답일 경우 true
                        return true;

                    temp = arrayCopy(board);
                }
            }
            key = rotate(key);  //키값 회전
            rotation++;
        }while(rotation < 4);

        return false;
    }

    public int[][] rotate(int[][] arr) {    //배열 90도 회전
        int n = arr.length;
        int[][] result = new int[n][n];

        for(int i=0;i<n;i++) {
            for(int j=0;j<n;j++) {
                result[i][j] = arr[n-1-j][i];
            }
        }
        return result;
    }

    public int[][] arrayCopy(int[][] arr) { //2차원 배열 깊은 복사(deep copy)
        int[][] result = new int[arr.length][arr[0].length];
        for(int i=0;i<arr.length;i++) 
            System.arraycopy(arr[i], 0, result[i], 0, arr[0].length);
        return result;
    }

    public boolean isUnlock(int[][] board ,int row, int col, int lockLen) { //잠금해제 확인
        for(int i=0;i<lockLen;i++) 
            for(int j=0;j<lockLen;j++) 
                if(board[row+i][col+j] == 0)
                    return false;

        return true;
    }

}
```


## 해결 과정

자물쇠를 중심으로 키의 모서리부터 반대쪽 모서리 끝까지 모든 경우의 수를 조합해보고 키를 회전시키는 방식으로 구현하였다.

![image](https://user-images.githubusercontent.com/47655983/103123279-2a79e580-46c7-11eb-870e-e8df02231217.png)

위 사진을 보면 `4x4`자물쇠에 `3x3`열쇠의 경우의 수 탐색을 위한 2차원 배열을 만들었다.

탐색은 왼쪽 위부터 한칸씩 오른쪽으로 이동하면서 잠금의 풀림유무를 살피고 끝까지 이동하면 그 다음 행으로 이동한다. 끝까지 탐색하고나면 `rotate()`함수로 배열을 90도, 180도, 270도로 회전시켜 다시 탐색한다.

사이즈는 중앙에 자물쇠를 놓고 키는 최소 한개만 겹친 양옆의 상태를 더한 값이다.

즉 자물쇠 사이즈를 n, 키 사이즈를 m 이라고 하면 비교를 위한 2차원 배열은 `n+2*(m-1)`이 된다.

그리고 키를 움직이는 범위를 정해야하는데 이것은 식으로 표현하면 `n+m-1`이 된다

이제 키를 자물쇠에 맞추는 방법을 말해보면 돌기는 돌기끼리 만나면 안되고 돌기(1)와 홈(0)끼리만 맞아야 한다. 서로 다른 값이어야 참이 됨으로 XOR 게이트로 계산하면 된다.


## 결과

![image](https://user-images.githubusercontent.com/47655983/103123298-3b2a5b80-46c7-11eb-889a-50e7f324a2a1.png)