---
layout: post
title: "[알고리즘, DFS/BFS] Java - 음료수 얼려먹기"
data:   2022-02-26 19:04:00+0900
categories: jekyll update
tags: [algorigtm]
---
# DFS/BFS - 음료수 얼려먹기
N x M 크기의 얼음 틀이 있다. 구멍이 뚫려 있는 부분은 0, 칸막이자 존재하는 부분은 1로 표시된다. 구멍이 뚫려있는 부분끼리 상, 하, 좌, 우로 붙어 있는 경우 서로
 연결되어 있는 것으로 간주한다. 이때, 얼음 틀의 모양이 주어졌을 때 생성되는 총 아이스크림의 개수를 구하는 프로그램을 작성하시오.  
다음의 4x5 얼음틀 예시에서는 아이스크림이 총 3개 생성된다.  
00110  
00011  
11111  
00000  
  
[ 입력 조건 ]  
- 첫 번재 줄에 얼음 틀의 세로 길이 N과 가로 길이 M이 주어진다.  
- 두 번째 줄 부터 N + 1번째 줄까지 얼음 틀의 형태가 주어진다.  
- 이때 구멍이 뚫려있는 부분은 0, 그렇지 않은 부분(벽)은 1이다.  
  
[ 출력 조건 ]  
- 한 번에 만들 수 있는 아이스크림의 개수를 출력한다.  
  
[ 입력 예시 ]  
4 5  
00110  
00011  
11111  
00000  
  
[ 출력 예시 ]  
3  
  
# 내가 작성한 코드

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Question1 {
    public static int N;
    public static int M;
    public static int[][] iceCream;
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine(), " ");
        int count = 0; // 아이스크림의 개수
        N = Integer.parseInt(st.nextToken());
        M = Integer.parseInt(st.nextToken());
        iceCream = new int[N][M];
        for(int i = 0; i < N; i++){
            String[] temp = br.readLine().split("");
            for(int j = 0; j < M; j++){
                iceCream[i][j] = Integer.parseInt(temp[j]);
            }
        }

        for(int x = 0 ; x < N; x++){
            for(int y = 0 ; y < M; y++){
                if(dfs(x, y)){
                    count++;
                }
            }
        }
        System.out.println(count);
    }

    public static boolean dfs(int x, int y){
        if(x < 0 || x >= N || y < 0 || y >= M){
            return false ; // 얼음틀의 크기를 벗어남.
        }
        if(iceCream[x][y] == 0){
            iceCream[x][y] = 1;
            dfs(x+1, y); // 우
            dfs(x-1,y); // 좌
            dfs(x, y-1); // 상
            dfs(x, y+1); // 하
            return true;
        }
        return false;
    }
}

```
  
# 결과
case 1 :  
```console
4 4     // 입력
0010
1011
1100
0110
4       // 출력
```
case 2:  
```console
4 5     // 입력
00110
00011
11111
00000
3       // 출력
```
case 3:
```console
15 14   // 입력
00000111100000
11111101111110
11011101101110
11011101100000
11011111111111
11011111111100
11000000011111
01111111111111
00000000011111
01111111111000
00011111111000
00000001111000
11111111110011
11100011111111
11100011111111
8       // 출력
```
  
# 코드 설명
```java
        for(int x = 0 ; x < N; x++){
            for(int y = 0 ; y < M; y++){
                if(dfs(x, y)){
                    count++;
                }
            }
        }
# main 메소드 일부
```
얼음 틀(그래프)의 첫 시작점은 (0,0)이다. 따라서 반복문 변수 x = 0, y = 0을 기준으로 DFS탐색을 시작한다. **DFS로부터 반환되는 값이 `true`일 경우에만 아이스크림의 개수가 1씩 증가하도록 하였다.**  

```java
    public static boolean dfs(int x, int y){
        if(x < 0 || x >= N || y < 0 || y >= M){
            return false ; // 얼음틀의 크기를 벗어남.
        }
        if(iceCream[x][y] == 0){
            iceCream[x][y] = 1;
            dfs(x+1, y); // 우
            dfs(x-1,y); // 좌
            dfs(x, y-1); // 상
            dfs(x, y+1); // 하
            return true;
        }
        return false;
    }
# dfs 메소드
```
- 파라미터 : int x, int y

```java
        if(x < 0 || x >= N || y < 0 || y >= M){
            return false ; // 얼음틀의 크기를 벗어남.
        }
```
해당 조건문은 dfs 메소드 최상단에 위치하여 **아이스크림의 틀을 벗어나면** 즉시 false를 반환시켜 해당 메소드를 종료시킨다.  

```java
        if(iceCream[x][y] == 0){
            iceCream[x][y] = 1;
            dfs(x+1, y); // 우
            dfs(x-1,y); // 좌
            dfs(x, y-1); // 상
            dfs(x, y+1); // 하
            return true;
        }
```
해당 조건문은 처음 방문한 곳이 0이라면 나머지 재귀 함수의 결과와 상관없이 `true`를 반환한다. 만약 방문한 곳이 0이면 **방문한 곳을 1로 방문처리시킨다.** 그 뒤, 해당 위치 기준으로 상, 하, 좌, 우에 해당하는 재귀를 실행한다. **상하좌우 재귀 결과에 상관없이 이미 방문한 곳이 0이기에 결과에 상관없에 true를 반환한다.**  

[문제 출처]  
[나동빈님](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9791162243077)