---
layout: post
title: "[알고리즘, 구현, 완전탐색] Java - 상하좌우"
data:   2022-02-14 18:19:00+0900
categories: jekyll update
tags: [algorigtm]
---
# 구현 문제 - 상하좌우
여행가 A는 NxN 크기의 정사각형 공간 위에 서 있다. 이 공간은 1x1 크기의 정 사각형으로 나누어져 있다.  
가장 왼쪽 위 좌표는 (1,1)이며, 가장 오른쪽 아래 좌표는 (N, N)에 해당한다. 여행가 A는 상, 하, 좌, 우 방향으로 이동할 수 있으며, 
시작 좌표는 항상 (1,1)이다. 우리 앞에는 여행가 A 가 이동할 계획이 적인 계획서가 놓여 있다.  
계획서에는 하나의 줄에 띄어쓰기를 기준으로 하여 L, R, U, D 중 하나의 문자가 반복적으로 적혀있다.  
  
[ 계획서 내용 ]  
R -> R -> R -> U -> D -> D  
  
각 문자의 의미는 다음과 같다.  
- L : 왼쪽으로 한 칸 이동  
- R : 오른쪽으로 한 칸 이동  
- U : 위로 한 칸 이동  
- D : 아래로 한 칸 이동  
이때, 여행가 A가 NxN 크기의 정사각형 공간을 벗어나는 움직임은 무시한다. 예를 들어 (1,1)의 위치에서 L 혹은 U를 만나면 무시된다.  
계획서가 주어졌을 때 여행가 A가 최종적으로 도착할 지점의 좌표를 출력하는 프로그램을 작성하시오.  
  
[ 입력 조건 ]  
첫째 줄에 공간의 크기를 나타내는 N이 주어진다.  
둘째 줄에 여행가 A가 이동할 계획서 내용이 주어진다.  
  
[ 출력 조건 ]  
첫째 줄에 여행가 A가 최종적으로 도착할 지점의 좌표 (X, Y)를 공백으로 구분하여 출력한다.  
  
[ 입력 예시 ]  
5  
R R R U D D  
  
[ 출력 예시 ]  
3 4  
  
# 내가 작성한 코드

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Question1 {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int N = Integer.parseInt(br.readLine());
        int x = 1, y = 1;
        String[] ba; // 방향
        ba = br.readLine().split(" ");
        for(int i = 0 ; i < ba.length; i++){
            switch(ba[i]){
                case "R":
                    if(y>N) break;
                    y++;
                    break;
                case "L":
                    y--;
                    if(y<=0) {
                        y++;
                        break;
                    }
                    break;
                case "U":
                    x--;
                    if(x<=0) {
                        x++;
                        break;
                    }
                    break;
                case "D":
                    if(x>N) break;
                    x++;
                    break;
            }
        }
        System.out.println(x+" "+y);
    }
}
```
  
# 결과
case 1 :  
```console
5               // 입력
R R R U D D     // 입력
3 4             // 출력
```
case 2 :  
```console
4               // 입력
R L R L R L R   // 입력
1 2             // 출력
```
case 3 :  
```console
5                   // 입력
R R R D D D U D D   // 입력
5 4                 // 출력
```
  


[문제 출처]  
[나동빈님](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9791162243077)