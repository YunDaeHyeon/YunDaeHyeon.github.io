---
layout: post
title: "[알고리즘, 정렬] Java - 위에서 아래로"
data:   2022-02-28 19:10:00+0900
categories: jekyll update
tags: [algorigtm]
---
# 정렬 - 위에서 아래로
하나의 수열에는 다양한 수가 존재한다. 이러한 수는 크기에 상관없이 나열되어 있다. 이 수를 큰 수부터 작은 수의 순서로 정렬해야 한다.  
수열을 내림차순으로 정렬하는 프로그램을 만드시오.  
  
[ 입력 조건 ]  
- 첫째 줄에 수열에 속해 있는 수의 개수 N이 주어진다. (1 <= N <= 500)  
- 둘째 줄부터 N+1 번째 줄까지 N개의 수가 입력된다. 수의 범위는 1 이상 100,000 이하의 자연수이다.  
  
[ 출력 조건 ]  
- 입력으로 주어진 수열이 내림차순으로 정렬된 결과를 공백으로 구분하여 출력한다. 동일한 수의 순서는 자유롭게 출력해도 된다.  
  
[ 입력 예시 ]  
3  
15  
27  
12  
  
[ 출력 예시 ]  
27 15 12  
  
# 내가 작성한 코드

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Arrays;
import java.util.Collections;

public class Question1 {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        Integer[] N = new Integer[Integer.parseInt(br.readLine())];
        for(int i = 0 ; i < N.length ; i++){
            N[i] = Integer.parseInt(br.readLine());
        }
        br.close();
        Arrays.sort(N, Collections.reverseOrder());
        for(int i = 0 ; i  < N.length ; i++)
            System.out.print(N[i]+" ");
    }
}
```

# 결과
case 1 :  
```console
// 입력
3
15
27
12
// 출력
27 15 12
```
case 2 :  
```console
// 입력
5
38
45
12
33
79
// 출력
79 45 38 33 12
```
  
  
  

---    
[문제 출처]  
[나동빈님](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9791162243077)