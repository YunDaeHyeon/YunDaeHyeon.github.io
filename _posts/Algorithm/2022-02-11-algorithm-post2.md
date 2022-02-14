---
layout: post
title: "[알고리즘, 그리디] Java - 거스름돈 구하기"
data:   2022-02-11 03:40:00+0900
categories: jekyll update
tags: [algorigtm]
---
# 그리디 알고리즘 - 거스름돈 구하기
카운터에는 거스름돈으로 사용할 동전이 500원, 100원, 50원, 10원이 무한정 있다.
손님에게 거슬러 줘야 할 돈이 N원일 때 거슬러줘야 할 최소 개수를 구하라. (N은 항상 10의 배수이다.)

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Question1 {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int N = Integer.parseInt(br.readLine()); // 손님이 주는 금액 1260원
        int count = 0; // 동전의 개수
        int money[] = {500, 100, 50, 10};
        for(int i = 0 ; i < money.length; i++){
            count += N / money[i]; // (동전의 개수 + 손님이 주는 금액) / 500원(100원, 50원, 10원)
            N %= money[i]; // 손님이 준 금액 % 동전
        }
        System.out.println(count);
    }
}
```

# 결과

```console
7680 // 입력
20 // 출력
```
  


---
[문제 출처]  
[나동빈님](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9791162243077)