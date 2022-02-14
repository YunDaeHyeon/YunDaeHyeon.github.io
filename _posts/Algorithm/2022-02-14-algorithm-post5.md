---
layout: post
title: "[알고리즘, 그리디] Java - 1이 될 때까지"
data:   2022-02-14 17:15:00+0900
categories: jekyll update
tags: [algorigtm]
---
# 그리디 알고리즘 - 1이 될 때까지
어떠한 수 N이 1이 될 때까지 다음의 두 과정 중 하나를 반복적으로 선택하여 수행하려고 한다.  
단, 두 번째 연산은 N이 K로 나누어 떨어질 때만 선택할 수 있다.  
1. N에서 1을 뺀다.  
2. N을 K로 나눈다.  
예를 들어 N이 17, K가 4라고 가정할 때 1번의 과정을 한 번 수행하면 N은 16이 된다.  
이후 2번의 과정을 2번 수행하면 N은 1이 된다. 결과적으로 전체 과정을 실행한 횟수는 3이다.  
이는 N을 1로 만드는 최소 횟수이다.  
N과 K가 주어질 때 N이 1이 될 때 까지 1번 혹은 2번의 과정을 수행해야하는 최소 연산 횟수를 구하는 프로그램을 작성하시오.  
  
[ 입력 조건 ]  
첫째 줄에 N(2 <= N <= 100,000)과 K(2 <= K <= 100,000)가 공백으로 구분되어 각각 자연수로 주어진다.  
이때, N은 항상 K보다 크거나 같다.  
  
[ 출력 조건 ]  
첫째 줄에 N이 1이 될 때 까지 1번 혹은 2번의 과정을 수행해야 하는 횟수의 최소값을 출력한다.  
  
[ 입력 예시 ]  
25 5  
  
[ 출력 예시 ]  
2  
  
# 결과

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Question4 {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine(), " ");
        int N = Integer.parseInt(st.nextToken());
        int K = Integer.parseInt(st.nextToken());
        int count = 0;
        while(true){
            if(N % K == 0){ // 만약 N이 K로 나뉘어 떨어지면
                N/=K;
                count++;
            }else if(N == 1){
                break;
            }else{
                N--;
                count++;
            }
        }
        System.out.println(count);
    }
}
```
  
case 1 :  
```console
25 5    // 입력
2       // 출력
```
case 2 :  
```console
17 4    // 입력
3       // 출력
```
case 3 :  
```console
18 3    // 입력
3       // 출력
```
  
  
---
[문제 출처]  
[나동빈님](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9791162243077)