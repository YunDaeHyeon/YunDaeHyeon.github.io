---
layout: post
title:  "[백준] 8958번 OX퀴즈 : JAVA"
date:   2022-01-23 18:13:00+0900
categories: jekyll update
tags: [백준, java, algorithm]
---
# 백준 8958 OX퀴즈
 - 언어 : Java

# 문제
![image](/assets/img/blog/백준/백준 8958.png) <br>

# 코드
```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String arr[] = new String[Integer.parseInt(br.readLine())];
        for(int i = 0 ; i < arr.length; i++)
            arr[i] = br.readLine();
        br.close();
        for(int i = 0; i < arr.length; i++){
            int count = 0;
            int result = 0;
            for(int j = 0 ; j < arr[i].length();j++){
                if(arr[i].charAt(j) == 'O')
                    count++;
                else
                    count = 0;
                result += count;
            }
            System.out.println(result);
        }
    }
}
```
# 설명
배열을 이용하여 문자열을 입력받은 뒤 각 문장 별 O와 X를 구분해야하기에 이중 반복문을 이용하여 구분을 진행한다.

# 헷갈렸던 점
안쪽 반복문의 조건식 **arr[i].length()**에서 **arr[j]**로 입력을 시키니 *ArrayIndexOutOfBoundsException*가 자꾸 튀어나와 죽을맛이었다.  
왜 오류가 발생하지? 라는 생각으로 천천히 코드를 재검토 했더니 arr[j]를 조건식에 넣으면 조건이 가변적으로 변할뿐더러 arr배열의 크기가 5일때 arr[0]의 크기가 6이라면 당연히 arr[i]로 진행을 하는 것이 올바른 방식이었다. 정신 바짝 차려야겠다.