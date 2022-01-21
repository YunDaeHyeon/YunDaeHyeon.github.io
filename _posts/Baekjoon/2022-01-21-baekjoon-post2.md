---
layout: post
title:  "[백준] 3052번 나머지 : JAVA"
date:   2022-01-21 22:40:00+0900
categories: jekyll update
tags: [백준, java, algorithm]
---
# 백준 3052 숫자의 개수
 - 언어 : Java

# 문제
![image](/assets/img/blog/백준/백준 3052.png) <br>

# 코드
```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.HashSet;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        HashSet<Integer> hashSet = new HashSet<>();
        for(int i = 0 ; i < 10 ; i++)
            hashSet.add(Integer.parseInt(br.readLine()) % 42);
        br.close();
        System.out.println(hashSet.size());
    }
}
```
# 설명
Scanner보단 효율이 뛰어난 BufferedReader 사용  
문제의 **서로 다른 나머지**는 중복이 허용하지 않는다는 것을 의미하여 Set 인터페이스의 HashSet클래스 사용  

# 다른 코드
[Stranger's lab](https://st-lab.tistory.com/46)님의 해당 문제 풀이를 보고 다시 작성해본 코드.  
문제에서 42를 나눈 나머지를 별 대수롭지 않게 넘겼는데 이를 이용해서 해당 문제를 푼 것과 HashSet클래스 없이 코드를 짜봤는데 각 수를 42로 나눈 나머지의 서로 다른 값을 계산할 때 너무 애를 먹었다.  
그런데 이분은 **42의 크기를 가진 boolean**(!!) 배열을 선언하여 이를 이용해서 문제를 푸신 것을 보니까 너무 소름이 돋았다. 더욱 더 공부를 열심히 해야겠다.
```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        boolean arr[]= new boolean[42];
        int count = 0;
        for(int i = 0 ; i < 10 ; i++)
            arr[Integer.parseInt(br.readLine()) % 42] = true;
        for(boolean result : arr){
            if(result)
                count++;
        }
        System.out.println(count);
    }
}
#=> 어떻게 하면 new boolean[42] 이라는 풀이를 할 수 있을까? 대단하다.
```