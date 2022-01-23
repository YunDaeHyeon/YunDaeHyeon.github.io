---
layout: post
title:  "[백준] 1546번 평균 : JAVA"
date:   2022-01-23 12:02:00+0900
categories: jekyll update
tags: [백준, java, algorithm]
---
# 백준 1546 평균
 - 언어 : Java

# 문제
![image](/assets/img/blog/백준/백준 1546.png) <br>

# 코드
```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int arr[] = new int[Integer.parseInt(br.readLine())];
        double avg[] = new double[arr.length];
        int M = 0;
        double all = 0;
        StringTokenizer st = new StringTokenizer(br.readLine()," ");
        for(int i = 0 ; i < arr.length; i++){
            arr[i] = Integer.parseInt(st.nextToken());
            if(arr[i] > M) M = arr[i];
        }
        br.close();
        for(int i = 0 ; i < arr.length; i++){
            avg[i] = (double)arr[i]/M*100;
            all+=avg[i];
        }
        System.out.println(all/arr.length);
    }
}
# 효율이 좀 떨어지는 코드 .. 추후 리팩토링 해야겠다.
```
# 설명
문제의 조건인 최댓값 M을 구한 뒤 M을 바탕으로 *입력받은 값/Mx100*을 해주었다.  
이때, *입력받은 값/Mx100*을 저장하는 값의 자료형은 **실수형**이기 때문에 꼭 *입력한 점수/최댓값*은 형 변환을 시켜야 한다.  
(예를 들어, 입력한 점수 10 최댓값 30인 경우 0.3333이 나와야겠지만 정수형으로 연산되기에 뒤 소수점이 모두 잘려나가 0이 100과 곱해져서 값이 다르게 출력된다.)  
또한, 문제의 입력 조건에서 **엔터**를 통해 값을 하나하나 입력받는 것이 아닌, **한 문장**으로 입력받기에 이를 주의하고 StringTokenizer를 통해 나누어서 연산하면 쉽게 풀 수 있는 문제이다.