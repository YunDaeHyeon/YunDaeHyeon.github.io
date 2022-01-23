---
layout: post
title:  "[백준] 15596번 정수 N개의 합: JAVA"
date:   2022-01-23 22:25:00+0900
categories: jekyll update
tags: [백준, java, algorithm]
---
# 백준 15596 정수 N개의 합
 - 언어 : Java

# 문제
![image](/assets/img/blog/백준/백준 15596.png) <br>

# 코드
```java
public class Test {
    long sum(int[] a) {
        long ans = 0;
        for(int i=0;i<a.length;i++)
            ans+=a[i];
        return ans;
    }
}
```
# 설명
지금까지 main을 이용하여 문제를 풀다가 낯선 문제풀이 방식을 접하니 처음에는 많이 헷갈렸지만 아주 조금만 생각하면 매우 쉽게 풀 수 있는 문제.

# 헷갈림
```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Test {
    long sum(int[] a) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int n = Integer.parseInt(br.readLine());
        a = new int[n];
        int result = 0;
        for(int i = 0 ; i < a.length; i++)
            result += a[i];
        return result;
    }
}
#=> 이렇게 푸는 건가? (위 코드는 처음에 제출한 틀린 답안)
```
문제를 이해를 못하여 n을 입력받아야 하나? 정수 n 개가 저장되어 있다면 그것을 연산해야 하는데 할당도 안한 매개변수 a를 사용하면 오류가 발생하지 않나? 이런 식으로 생각을 굉장히 많이 했던 것 같다. 나무를 보는 것이 아닌 숲을 보라는 말이 이럴 때 사용되는 말인 것 같다.
