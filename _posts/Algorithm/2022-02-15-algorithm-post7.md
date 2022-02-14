---
layout: post
title: "[알고리즘, 구현, 완전탐색] Java - 시각"
data:   2022-02-15 02:24:00+0900
categories: jekyll update
tags: [algorigtm]
---
# 구현/완전탐색 문제 - 시각
정수 N이 입력되면 00시 00분 00초부터 N시 59분 59초까지의 모든 시각 중에서 3이 하나라도 포함되는 모든 경우의 수를 구하는 프로그램을 작성하시오.  
예를 들어 1을 입력했을 때 다음은 3이 하나라도 포함되어 있으므로 세어야 하는 시각이다.  
 - 00시 00분 03초  
 - 00시 13분 30초  
반면에 다음은 3이 하나도 포함되어 있지 않으므로 세면 안 되는 시각이다.  
 - 00시 02분 55초  
 - 01시 27분 45초  
  
[ 입력 조건 ]  
 - 첫째 줄에 정수 N이 입력된다.  
  
[ 출력 조건 ]  
 - 00시 00분 00초부터 N시 59분 59초까지의 모든 시각 중에서 3이 하나라도 포함되는 모든 경우의 수를 출력한다.  
  
[ 입력 예시 ]  
5  
  
[ 출력 예시 ]  
11485  
  
# 내가 작성한 코드

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;

public class Question2 {
    public static void main(String[] args) throws Exception{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int N = Integer.parseInt(br.readLine());
        int count = 0;
        for(int i = 0 ; i <= N; i++){
            for(int m = 0 ; m < 60; m++){
                for(int s = 0 ; s < 60; s++){
                    if((String.valueOf(i)+m+s).contains("3")) count++;
                }
            }
        }
        System.out.println(count);
    }
}
```
  
# 결과

case 1 :  
```console
5       // 입력
11475   // 출력
```
case 2:  
```console
1       // 입력
3150    // 출력
```
case 3:
```console
23      // 입력
43875   // 출력
```
  
# 설명
해당 문제는 덧셈 연산자 +의 ***피연산자 중 하나라도 `String`형이라면 연산 결과는 `String`이 된다는 점을 이용하여 풀었다.***  
또한 문자열 연산은 정수 연산에 비해 많이 느리다. 이 점을 다시 공부할 필요가 있다.
  
[문제 출처]  
[나동빈님](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9791162243077)