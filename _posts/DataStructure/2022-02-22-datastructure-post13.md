---
layout: post
title: "[Java, 자료구조] 그래프(Graph) 구현하기 - 인접리스트(AdjacencyList)"
data:   2022-02-22 17:57:00+0900
categories: jekyll update
tags: [java, data structure]
---
# 그래프(Graph)란?
그래프(Graph)에 대한 설명은 이전 포스팅에 서술하였다.  
[[Java, 자료구조] 그래프(Graph) 구현하기 - 인접행렬(AdjacencyMatrix)](https://daegom.com/main/datastructure-post12/)

# 인접 리스트(AdjacencyList)
인접 리스트(AdjacencyList)는 그래프를 표현함에 있어 각각의 정점에 인접한 정점들을 연결 리스트로 표현한 것이다. **각 연결 리스트의 노드(Node)들은 인접 정점을 저장하게 된다.** 각 연결 리스트들은 헤더 노드를 있다. 또한 헤더 노드들은 **하나의 배열**로 구성되어 있다. **정점의 번호만 알면 해당 번호를 배열의 `inddex`로 한다면 각 정점의 연결 리스트에 접근할 수 있다.

## 무방향 그래프의 경우
정점 V1와 정점 V2을 연결하는 간선 E(V1, V2)는 **정점 V1의 연결리스트에 인접 정점 V2로 한 번 표현되고, 정점 V2의 연결리스트에 인접 정점 V1으로 다시 표현된다.** 즉, **각각의 연결리스트에 정점들이 입력되는 순서에 따라 연결 리스트내의 정점들의 순서가 달라질 수 있다.**
만약, 정점 0, 1, 2, 3으로 이루어진 그래프가 아래의 형태로 존재한다면  
**0 1 1 1**  
**1 0 1 0**  
**1 1 0 1**  
**1 0 1 0**  
다음과 같이 연결 리스트로 표현할 수 있다.  
**0 -> 1 -> 2 -> 3(null)**  
**1 -> 0 -> 2(null)**  
**2 -> 0 -> 1 -> 3(null)**  
**3 -> 0 -> 2(null)**  
***이를 통해 정점의 개수가 n개이며 간선의 수가 e개인 무방향 그래프를 나타내기 위해서는 n개의 연결 리스트가 필요하며 n개의 헤더노드와 2e개의 노드가 필요함을 알 수 있다.***

# 인접 리스트의 공간 복잡도
- 메모리 : O(V+E)  
- 시간(모든 V 탐색) : O(V+E)  
- 시간(한 노드와 연결된 모든 노드, 차수) : O(E)  
- 시간(두 노드의 연결성 확인) : O(V)  

# 가중치가 없는 인접 리스트
```java
import java.io.IOException;
import java.util.ArrayList;
import java.util.LinkedList;

class Adj{
    private LinkedList<Integer>[] adj; // 인접 리스트 생성
    private int v; // 정점의 개수

    public Adj(int v){
        this.v = v;
        adj = new LinkedList[v+1];
        // 그래프가 1부터 시작하면 LinkedList[v+1]. ( 0은 비우기 )
        // 그래프가 0부터 시작하면 LinkedList[v]
        for(int i = 0; i < v+1; i++){
            adj[i] = new LinkedList<>();
        }
    }
    public void createEdge(int v1, int v2){
        adj[v1].add(v2);
        adj[v2].add(v1); // 하나를 빼면 방향 인접 리스트
    }
    public void print(){
        for(int i = 1; i < adj.length; i++)
            System.out.println("정점 "+i+"의 인접 노드 : "+adj[i]);
    }
}

//가중치가 없는 그래프(연결리스트)
public class AdjacencyList{
    public static void main(String[] args) throws IOException {
        int V = 5;
        Adj adj = new Adj(V);
        adj.createEdge(1, 2);
        adj.createEdge(1, 3);
        adj.createEdge(2, 4);
        adj.createEdge(3, 4);
        adj.createEdge(4, 5);

        adj.print();
    }
}
```

# 결과

```console
정점 1의 인접 노드 : [2, 3]
정점 2의 인접 노드 : [1, 4]
정점 3의 인접 노드 : [1, 4]
정점 4의 인접 노드 : [2, 3, 5]
정점 5의 인접 노드 : [4]
```
정상적으로 출력되었음을 알 수 있다.
인접 리스트는 `A[i] = i`와 연결된 정점이 `LinkedList`의 형태로 들어가있다. **이때, 정점이 아닌 간선이다!!**  
**가중치가 없는 인접리스트는 `A[1] = 2 -> 5`와 같은 형태로 작성되지만 가중치가 있는 인접리스트는 `A[2] = (1, 2)(3, 2)(4, 3)(5, 1)`의 형태로 저장된다.**

<p align="center"><img src="/assets/img/blog/정보/가중치 인접리스트.png"></p>
  



---  
[참고 및 리소스 출처]  
[공대사람](https://sarah950716.tistory.com/12)  
[wpioneer](https://wpioneer.tistory.com/128)  