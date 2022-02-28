---
layout: post
title: "[자료구조] 퀵 정렬(Quick Sort)"
data:   2022-02-28 17:28:00+0900
categories: jekyll update
tags: [data structure]
---
# 퀵 정렬(Quick Sort)이란?
이름과 같이 **빠른(Quick) 정렬**이다. 퀵 정렬은 기본적으로 **`분할 정복(Divide and Conquer)`**을 기반으로 정렬되는 방식이다. 퀵 정렬은 하나의 리스트를 **피벗(pivot)**을 기준으로 **두 개의 부분 리스트**로 나누어 정렬시킨 뒤, **각 부분 리스트에 대해 다시 위 처럼 재귀적으로 수행하여 정렬한다.**  
- 퀵 정렬은 데이터를 `비교`하면서 찾기 때문에 **비교 정렬**이며 정렬의 대상이 되는 데이터 외에는 추가적인 공간이 필요하지 않기 때문에 `제자리 정렬(in-place sort)`이다. 또한, 하나의 피벗을 두고 **두 개의 부분리스트를 만들 때 서로 떨어진 원소끼리 교환이 일어나기에 불안정정렬(Unstable Sort) 알고리즘이다.**  

# 장점과 단점
[장점]  
1. 특정 상태가 아닌 이상 **평균 시간 복잡도는 NlogN을 가지며 다른 NlogN 알고리즘에 비해 대체적으로 속도가 매우 빠르다.**(예를 들어 NlogN의 시간이 소요되는 merge sort에 비해 2~3배정도 빠르다.)  
2. 추가적인 별도의 메모리를 필요하지 않는다.  
3. 재귀 호출에 의한 공간 복잡도는 **logN**으로 메모리를 적게 소비한다.  
  
[단점]  
1. 특정 조건하에 성능이 급격하게 매우 떨어진다.  
2. **재귀를 사용하기 때문에 재귀를 사용하지 못하는 환경일 경우 구현이 매우 복잡해진다.**  
  
# 구현
퀵 정렬의 구현 과정은 아래와 같다.
1. 피벗을 하나 선택한다.  
2. 피벗을 기준으로 양쪽에서 피벗보다 큰 값, 혹은 작은 값을 찾는다. 왼쪽에서부터는 피벗보다 큰 값을, 오른쪽에서부터는 피벗보다 작은 값을 찾는다.  
3. 양 방향에서 찾은 두 원소를 교환(swap)한다.  
4. 왼쪽에서 탐색하는 위치와 오른쪽에서 탐색하는 위치가 엇갈리지 않을 때 까지 2번으로 돌아가서 위 과정을 반복한다.  
5. 엇갈린 기점을 기준으로 **두 개의 부분리스트로 나누어** 1번으로 돌아가 **해당 부분리스트의 길이가 1이 아닐 때 까지** 1번 과정을 반복한다. (이를 `분할(Divied)`이라 한다.)  
6. 인접한 부분리스트끼리 합쳐 탐색을 종료한다.(이를 `Conqure(정복)`이라 한다.)  
  
***더 쉽게 말하면 아래와 같다.***
1. 임의의 피벗 값(기준 값)을 설정한다.  
2. 리스트의 길이가 n일때 **리스트의 시작점 = 0(최초), 도착점 = n - 1(최초)을 설정한다.** 시작점, 도착점에서 출발하며 **피벗 값**을 기준으로 피벗보다 **작은**원소는 **왼쪽**으로, 큰 원소는 **오른쪽**으로 옮겨준다.  
3. 시작점이 도착점보다 작으면(시작점 < 도착점) **새로운 시작점과 도착점을 설정한다.** 그 다음 파티션을 분할하여 1번부터 3번까지의 과정을 반복한다.  
**이때, [시작점 - 도착점] 사이의 리스트 원소가 1개인 파티션은 반복하지 않는다.**

# 예시
만약, `[7, 4, 2, 8, 3, 5, 1, 6, 10, 9]`가 리스트에 있다면 아래와 같은 과정을 거친다.  

<p align="center"><img src="/assets/img/blog/정보/퀵 1.png"></p>

- 피벗의 값과 시작점, 끝점을 설정한다.  
- 시작(Start) : 7, 끝(End) : 9, 피벗(Pivot) : 5  

<p align="center"><img src="/assets/img/blog/정보/퀵 2.png"></p>

- 왼쪽에서는 피벗(Pivot)보다 **큰** 값을, 오른쪽에서는 피벗(pivot)보다 **작은** 값을 찾는다.  
- 피벗(Pivot) : 5, 시작(Start, 큰값) : 7, 끝(End, 작은값) : 1  

<p align="center"><img src="/assets/img/blog/정보/퀵 3.png"></p>

- 왼쪽, 오른쪽에서 큰 값과 작은 값을 찾았다면 두 개의 값을 SWAP(교체)한다.  
- 피벗(Pivot) : 5, 시작(Start) : 1, 끝(End) : 7  

<p align="center"><img src="/assets/img/blog/정보/퀵 4.png"></p>

- 왼쪽(Start), 오른쪽(End)에 해당하는 포인트(Point)들을 왼쪽 포인트는 오른쪽으로, 오른쪽 포인트는 왼쪽으로 한 칸씩 이동하고 위 과정을 반복한다.  
- 한 칸 이동한 후 결과 -> 왼쪽(Start) : 4, 오른쪽(End) : 5  

<p align="center"><img src="/assets/img/blog/정보/퀵 5.png"></p>

- 똑같이 피벗(Pivot)보다 크고 작은 값을 찾는다.  
- 시작(Start, 큰값) : 8, 끝(End, 작은값) : 3  

<p align="center"><img src="/assets/img/blog/정보/퀵 6.png"></p>

- 두 값을 SWAP 한다.  
- 시작(Start) : 3, 끝(End) : 8

<p align="center"><img src="/assets/img/blog/정보/퀵 7.png"></p>

- 위와 같이 Start와 End를 우측, 좌측으로 한 칸씩 이동한다. 이때, End 포인트가 Start 포인트보다 **작으면** 해당 **파티션의 정렬을 종료한다.**  

<p align="center"><img src="/assets/img/blog/정보/퀵 8.png"></p>

- 종료한 파티션의 Start, End 포인트를 기준으로 새로운 Start, End 포인트를 설정하여 나눠진 파티션에 대해 정렬을 **재귀적으로 반복한다.**  
- **파티션이 더 이상 쪼개지지 않을 때 까지 위 과정을 반복한다.**  

## 코드

```java
public class QuickSort {
    public static void main(String[] args){
        int[] arr = {7, 4, 2, 8, 3, 5, 1, 6, 10, 9}; // 배열 선언
        // 배열, 시작점, 끝점에 해당하는 인덱스를 파라미터로 보낸다.
        quickSort(arr, 0, arr.length-1);
        for(int i = 0 ; i < arr.length; i++){
            System.out.print(arr[i]+" ");
        }
    }

    public static void quickSort(int[] arr, int start, int end){
        // 배열, 시작, 끝에 해당하는 변수를 보낸 뒤 리턴값을 받는다.
        int part = partition(arr, start, end);
        if(start<part-1){ // 시작점이 끝점 - 1보다 작으면
            quickSort(arr,start,part-1);
        }
        if(end>start){ // 끝점이 시작점보다 크면
            quickSort(arr, part, end);
        }
    }

    public static int partition(int arr[], int start, int end){
        int pivot = arr[(start+end)/2]; // 피벗 설정 (0+9)/2 = 4
        // 끝점이 시작점보다 크면 해당 파티션의 정렬을 종료한다.
        while(start<=end){ // 시작점이 끝점보다 작거나 같지 않을 때 까지 반복
            while(arr[start] < pivot){ // 시작점에 해당하는 값이 피벗보다 작으면
                start++; // 피벗보다 클 때 까지 start를 증가시킨다. (오른쪽 -> 왼쪽)
            }
            while(arr[end]>pivot){ // 끝점에 해당하는 값이 피벗보다 크면
                end--; // 피벗보다 작을 때 까지 end를 감소시킨다. (왼쪽 -> 오른쪽)
            }
            if(start<=end){ // 시작점이 끝점보다 작거나 같다면
                // 시작점에 있는 값과 끝점에 있는 값을 SWAP 한다.
                int temp = arr[start];
                arr[start] = arr[end];
                arr[end] = temp;
                // 그 뒤 Start, End 포인트를 한 칸씩 이동시킨다.
                start++;
                end--;
            }
        }

        // 끝점이 시작점보다 작아 해당 파티션의 정렬이 끝나면 시작점에 해당하는 인덱스를 반환한다.
        return start;
    }
}
```

# 결과

```console
1 2 3 4 5 6 7 8 9 10
```

# 설명

1. quickSort(arr, 0, arr.length-1)로 배열, 시작(Start), 끝(End)을 보낸다.  
2. 초기 변수의 값은 Start = 0, End = 9로 시작한다.  
3. int part를 통하여 partition 메소드 파라미터에 다시 배열, 시작(Start), 끝(End)을 보낸다.  
4. partition 메소드에서 피벗(Pivot)을 설정하여 초기 피벗의 값은 `arr[4]`의 3이다.  
5. while(start<=end)를 통하여 시작점이 끝점보다 **작거나 같을 때 까지 반복하며 만약, 끝점이 시작점보다 크다면 해당 파티션의 정렬을 종료시킨다.**  
6. `while(arr[start] < pivot)`을 통하여 피벗이 시작점에 해당하는 값보다 작을 때 까지 시작점을 1씩 증가시키며  
7. `while(arr[end]>pivot)`을 통하여 피벗이 끝점에 해당하는 값보다 클 때 까지 끝 점을 1씩 감소시킨다.  
8. if(start<=end)을 통하여 시작점이 끝점보다 작거나 같을 경우 **시작점에 있는 값과 끝점에 있는 값을 서로 swap 시킨다.  
9. 그 뒤 시작, 끝 포인트를 한 칸씩 이동시켜 시작점이 끝점보다 작다면 시작점(Start)를 반환한다.  
10. 최초로 반환된 시작점의 값은 3이기에 quickSort 메소드에 있는 part의 값은 3이 된다.  
11. `if(start<part-1)`로 시작점이 2보다 작으면 `quickSort(arr,start,part-1)`를 통하여 배열, 시작에 해당하는 값 0, 끝에 해당하는 값 2를 보내 재귀시켜 정렬한다.  
12. `if(end>start)`로 끝점이 시작점보다 크다면 `quickSort(arr, part, end)`를 통하여 배열, 시작에 해당하는 값 3, 끝에 해당하는 값 9를 보내 재귀시켜 정렬한다.  
13. `11번`, `12번`에 해당하는 과정을 통해 **파티션(리스트)의 `분할(Divied)`이 이루어지며 `arr[0] ~ arr[2]`끼리 탐색, `arr[3] ~ arr[9]`끼리 탐색**을 하게 된다.  
14. **시작점과 끝점 사이의 원소의 개수가 1개일 경우는 위 과정을 반복시키지 않으며** `분할(Divied)`이 이루어지지 않을 때 까지 쪼개진다면 정렬을 완료시킨다.  
  
  
  
  
  
---
[참고 및 이미지 출처]  
[st-lab](https://st-lab.tistory.com/179)  
[gwang920](https://gwang920.github.io/algorithm%20non%20ps/qucikSort/)  