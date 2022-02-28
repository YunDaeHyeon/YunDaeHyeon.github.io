---
layout: post
title: "[알고리즘, 정렬] Java - 두 배열의 원소 교체"
data:   2022-02-28 23:43:00+0900
categories: jekyll update
tags: [algorigtm]
---
# 정렬 - 두 배열의 원소 교체
동빈이는 두 개의 배열 A와 B를 가지고 있다. 두 배열은 N개의 원소로 구성되어 있으며, 배열의 원소는 모두 자연수이다. 동빈이는 최대 K번의 바꿔치기 연산을 수행할 수 있는데, 바꿔치기 연산이란 배열 A에 있는 원소 하나와 배열 B에 있는 원소 하나를 골라서 두 원소를 서로 바꾸는 것을 말한다.  
동빈이의 최종 목표는 배열 A의 모든 원소의 합이 최대가 되도록 하는 것이다.  
N, K와 배열 A, B의 정보가 주어졌을 때, 최대 K번의 바꿔치기 연산을 수행하여 만들 수 있는 배열 A의 모든 원소의 합의 최댓값을 출력하는 프로그램을 작성하시오.  
  
[ 예시(N = 5, K = 3일때) ]  
- 배열 A = {1, 2, 5, 4, 3}  
- 배열 B = {5, 5, 6, 6, 5}  
- 첫 번째 연산 : 배열 A의 `1`과 배열 `B`의 6을 바꾼다.  
- 두 번째 연산 : 배열 A의 `2`와 배열 `B`의 6을 바꾼다.  
- 세 번째 연산 : 배열 A의 `3`과 배열 `B`의 5를 바꾼다.  
따라서 배열 A는 `[6, 6, 5, 4, 5]`가 되며 이때 배열 A의 모든 원소의 합은 26이 되며 이보다 더 합을 크게 만들 수 없으므로 이 문제의 정답은 26이다.  
  
[ 입력 조건 ]  
- 첫 번째 줄에 N, K가 공백으로 구분되어 입력된다.  
- 두 번째 줄에 배열 A의 원소들이 공백으로 구분되어 입력된다.  
- 세 번째 줄에 배열 B의 원소들이 공백으로 구분되어 입력된다.    
  
[ 출력 조건 ]  
- 최대 K번의 바꿔치기 연산을 수행하여 만들 수 있는 배열 A의 모든 원소들의 합의 최댓값을 출력한다.  
  
[ 입력 예시 ]  
5 3  
1 2 5 4 3  
  
[ 출력 예시 ]  
26  
  
# 내가 작성한 코드

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Arrays;
import java.util.Collections;
import java.util.StringTokenizer;

public class Question3 {
    static int MAX = 0;
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine()," ");
        Integer[] A = new Integer[Integer.parseInt(st.nextToken())];
        Integer[] B = new Integer[A.length];
        int K = Integer.parseInt(st.nextToken());
        String[] temp = br.readLine().split(" ");
        for(int i = 0 ; i < A.length ; i++)
            A[i] = Integer.parseInt(temp[i]);
        temp = br.readLine().split(" ");
        for(int i = 0 ; i < B.length ; i++)
            B[i] = Integer.parseInt(temp[i]);
        Arrays.sort(A); // 오름차순 수행
        Arrays.sort(B, Collections.reverseOrder()); // 내림차순 수행
        // 배열 A에서 K개의 가장 작은 숫자를 찾기
        // 배열 B에서 K개의 가장 큰 숫자를 찾기
        // 그 둘을 바꾸기
        for(int i = 0 ; i < K ; i++){
            int dummy = B[i];
            B[i] = A[i];
            A[i] = dummy;
        }
        // 최대값 출력
        for(int i = 0 ; i < A.length ; i++){
            MAX += A[i];
        }
        System.out.print(MAX);
    }
}
```

# 결과
case 1 :  
```console
// 입력
5 3
1 2 5 4 3
5 5 6 6 5
// 출력
26
```
case 2 :  
```console
// 입력
5 3
7 8 6 5 4
6 7 3 9 3
// 출력
37
```
case 3 :  
```console
// 입력
6 4
3 4 5 6 7 9
8 3 2 1 9 4
// 출력
40
```
  
  
  
  
---    
[문제 출처]  
[나동빈님](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9791162243077)