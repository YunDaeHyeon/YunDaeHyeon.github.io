---
layout: post
title:  "[백준] 2557번 숫자의 개수 : JAVA"
date:   2022-01-21 21:42:00+0900
categories: jekyll update
tags: [백준, java, algorithm]
---
# 백준 2557번 숫자의 개수
 - 언어 : Java

# 문제
![image](/assets/img/blog/백준/백준 2577.png) <br>

# 코드
```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String result = Integer.toString(Integer.parseInt(br.readLine()) * Integer.parseInt(br.readLine()) * Integer.parseInt(br.readLine()));
        br.close();
        int number[] = new int[10];
        for (int i = 0; i < result.length(); i++) {
            number[(result.charAt(i) - '0')]++;
        }
        for (int i : number)
            System.out.println(i);
    }
}
```
# 설명
Scanner보단 효율이 뛰어난 BufferedReader 사용  
1. result에 입력한 값을 모두 곱하여 저장  
2. 계산된 값의 길이(String.length())만큼 반복문 진행  
3. 정수형 배열 number에 charAt(i) - '0' (혹은 48)을 통해 값을 뽑아와 1씩 증가  
4. for(Object : List) 사용해서 number의 객체를 i로 모두 뽑아와 출력  
**문자열에서 '숫자'를 뽑기 위해서는 반드시 - '0' 혹은 - 48을 연산해야함.**
**아니면 아스키코드 출력**

# 다른 코드
```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String result = Integer.toString(Integer.parseInt(br.readLine())*Integer.parseInt(br.readLine())*Integer.parseInt(br.readLine()));
        br.close();
        for(int i = 0; i < 10; i++){
            int count = 0;
            for(int j = 0 ; j < result.length(); j++){
                if(result.charAt(j) - '0' == i)
                    count++;
            }
            System.out.println(count);
        }
    }
}
```