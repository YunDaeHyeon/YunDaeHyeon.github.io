---
layout: post
title:  "[백준] 4344번 평균은 넘겠지: JAVA"
date:   2022-01-23 18:13:00+0900
categories: jekyll update
tags: [백준, java, algorithm]
---
# 백준 4344 평균은 넘겠지
 - 언어 : Java

# 문제
![image](/assets/img/blog/백준/백준 4344.png) <br>

# 코드
```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int N  = Integer.parseInt(br.readLine());
        for(int i = 0; i < N; i++){
            StringTokenizer st = new StringTokenizer(br.readLine(), " ");
            int num[] = new int[st.countTokens()];
            int first = Integer.parseInt(st.nextToken());
            int all = 0, result = 0;
            double avg;
            for(int j = 0 ; j < first; j++){
                num[j] = Integer.parseInt(st.nextToken());
                all += num[j];
            }
            avg = (double)all/first;
            for(int h = 0 ; h < first; h++) {
                if(num[h] > avg) result++;
            }
            avg = (double)result/first*100;
            System.out.println(String.format("%.3f%%",avg));
        }
    }
}
#=> 약간 효율적이지 못한 코드. 추후 재검토 예정
```
# 설명
한 문장으로 입력받기에 **StringTokenizer**을 이용해서 문자를 나눈 뒤 **countTokens()**를 이용하여 나눈 토큰의 개수만큼 배열의 크기를 배정한다.  
**nextToken()**을 이용하여 차례대로 나눈 문자를 정수로 캐스팅 한 뒤 연산을 진행하여 마지막에 평균을 넘는 학생들의 비율을 **String.format(문자열 포맷팅)**을 사용하여 출력한다.  
***(주의 : 문자열 포맷팅에서 %를 출력하기 위해서는 %를 두 번 사용한다. 그렇지 않으면 UnknownFormatConversionException이 발생하니 주의)***