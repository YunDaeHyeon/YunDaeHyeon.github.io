---
layout: post
title: "[알고리즘, 그리디] Java - 숫자 카드 게임"
data:   2022-02-14 16:20:00+0900
categories: jekyll update
tags: [algorigtm]
---
# 그리디 알고리즘 - 숫자 카드 게임
숫자 카드 게임은 여러 개의 숫자 카드 중에서 가장 높은 숫자가 쓰인 카드 한 장을 뽑는 게임이다.  
단, 게임의 룰을 지키며 카드를 뽑아야 하고 룰은 다음과 같다.  
1. 숫자가 쓰인 카드들이 N * M 형태로 놓여있다. N은 행의 개수, M은 열의 개수를 의미한다.  
2. 먼저 뽑고자 하는 카드가 포함되어 있는 행을 선택한다.  
3. 그 다음 선택된 행에 포함된 카드들 중 가장 숫자가 낮은 카드를 뽑아야 한다.  
4. 따라서 처음 카드를 골라낼 행을 선택할 때, 이후에 해당 행에서 가장 숫자가 낮은 카드를 뽑는 것을 고려하여 최종적으로 가장 높은 숫자의 카드를 뽑을 수 있도록 전략을 세워야 한다.  
  
[ 예시 ]  
3 1 2  
4 1 4  
2 2 2  
  
여기서 카드를 골라낼 행을 고를 때 첫 번쨰 혹은 두 번쨰 행을 선택하는 경우, 최종적으로 뽑는 카드는 1이다.  
하지만 세 번쨰 행을 선택하는 경우 최종적으로 뽑는 카드는 2이다. 해당 예시에서는 세 번째 행을 선택하여 숫자 2가 쓰여진 카드를 뽑는 것이 정답이다.  
  
---
  
[ 입력 조건 ]  
1. 첫째 줄에 숫자 카드들이 놓인 행의 개수 N과 열의 개수 M이 공백을 기준으로 하여 각각 자연수로 주어진다.  
2. 둘째 줄 부터 N개의 줄에 걸쳐 카드에 적힌 숫자가 주어진다. 각 숫자는 1 이상 10,000 이하의 자연수이다.  
  

[ 출력 조건 ]  
- 첫째 줄에 게임의 룰에 맞게 선택한 카드에 적힌 숫자를 출력한다.  
  

[ 입력 예시 ]  
3 3  
3 1 2  
4 1 4  
2 2 2  
  

[ 출력 예시 ]  
2  

# 결과

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Arrays;
import java.util.StringTokenizer;

public class Question3 {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine()," ");
        int[][] arr = new int[Integer.parseInt(st.nextToken())][Integer.parseInt(st.nextToken())];
        int[] result = new int[arr.length];
        for(int i = 0; i < arr.length; i++){
            st = new StringTokenizer(br.readLine(), " ");
            for(int j = 0 ; j < arr[i].length; j++){
                arr[i][j] = Integer.parseInt(st.nextToken());
            }
            Arrays.sort(arr[i]);
            result[i] = arr[i][0];
        }
        Arrays.sort(result);
        System.out.println(result[result.length-1]);
    }
}
```
  
case 1 :  
```console
3 3     // 입력
3 1 2   // 입력
4 1 4   // 입력
2 2 2   // 입력
2       // 출력
```
case 2 :  
```console
2 4        // 입력
7 3 1 8    // 입력
3 3 3 4    // 입력
3          // 출력
```
  


---
[문제 출처]  
[나동빈님](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9791162243077)