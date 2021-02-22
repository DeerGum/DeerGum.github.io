---
title:  "[Programmers] 신규아이디 추천"
search: true
categories: 
  - algorithm
tags:
  - Programmers
  - Java
  - String
last_modified_at: 2021-02-08T08:06:00-05:00
---

## 개요

2021 kakao 공채 1번 문제

문자열 다루는 능력을 확인해보는 문제였다.

카카오는 특히 문자열을 좋아하는 것 같다...

[문제링크](https://programmers.co.kr/learn/courses/30/lessons/72410)

## 코드

```java
public class NewId {
    public String solution(String new_id) {
        int len;

        new_id = new_id.toLowerCase();  //소문자로 변경
        new_id = new_id.replaceAll("[^0-9a-z-_.]+",""); //알파벳 숫자, -, _, . 제외한 문자제거
        new_id = new_id.replaceAll("[..]+",".");    //2번이상 연속된 마침표는 하나로 치환

        if(new_id.charAt(0) == '.') //처음에 . 이 있으면 제거
            new_id = new_id.substring(1);

        len = new_id.length();
        if (len > 0 && new_id.charAt(len-1) == '.') //마지막에 . 이 있으면 제거
            new_id = new_id.replaceFirst(".$","");

        if (new_id.length() == 0)   //빈문자열이면 a를 대입
            new_id = "a";

        if (new_id.length() >= 16) {    //길이가 16자 이상이면 15자로 자름
            new_id = new_id.substring(0,15);
            if (new_id.charAt(new_id.length()-1) == '.')    //만약 자르고 끝에 . 이 남으면 제거
                new_id = new_id.replaceFirst(".$","");
        }

        while(new_id.length() < 3) {    //길이가 2자 이하이면 마지막 문자를 길이가 3이 될때까지 이어 붙임
            new_id += new_id.charAt(new_id.length()-1);
        }

        return new_id;
    }
}
```

## 해결 과정

카카오는 쉬운 난이도의 문제에선 알고리즘을 알려주고 그것을 구현시키는 경우가 많다.

문제에 제시된 알고리즘은 다음과 같다.

```
1단계 new_id의 모든 대문자를 대응되는 소문자로 치환합니다.
2단계 new_id에서 알파벳 소문자, 숫자, 빼기(-), 밑줄(_), 마침표(.)를 제외한 모든 문자를 제거합니다.
3단계 new_id에서 마침표(.)가 2번 이상 연속된 부분을 하나의 마침표(.)로 치환합니다.
4단계 new_id에서 마침표(.)가 처음이나 끝에 위치한다면 제거합니다.
5단계 new_id가 빈 문자열이라면, new_id에 "a"를 대입합니다.
6단계 new_id의 길이가 16자 이상이면, new_id의 첫 15개의 문자를 제외한 나머지 문자들을 모두 제거합니다.
     만약 제거 후 마침표(.)가 new_id의 끝에 위치한다면 끝에 위치한 마침표(.) 문자를 제거합니다.
7단계 new_id의 길이가 2자 이하라면, new_id의 마지막 문자를 new_id의 길이가 3이 될 때까지 반복해서 끝에 붙입니다.
```

위 알고리즘 순서대로 문자열을 바꾸면 해결되는 문제인데 2단계는 정규표현식을 사용해야 쉽게 해결 가능했다. 정규표현식에 대해서 좀 더 공부해뵈야겠다.

## 결과

![image](https://user-images.githubusercontent.com/47655983/107176408-a8dddd00-6a12-11eb-8a6b-a03d68136b1a.png)
