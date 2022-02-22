---
layout: post
title: "[Java, 자료구조] 그래프(Graph) 구현하기 - 인접행렬(AdjacencyMatrix)"
data:   2022-02-22 17:12:00+0900
categories: jekyll update
tags: [java, data structure]
---
# 그래프(Graph)란?
**객체 사이의 연결 관계를 표현할 수 있는 자료구조이다.**  
**그래프는 인접행렬(AdjacencyMatrix)과 인접리스트(AdjacencyList)로 표현할 수 있다.**  
오일러는 특정 지역을 **`정점(Node, Vertex)`**, 정점을 잇는 선을 **`간선(Edge)`**이라 표현하였다.  
<p align="center"><img src="/assets/img/blog/정보/그래프.png"></p>

**그래프(G)는 정점(Vertex)의 집합 `V`와 이들을 연결하는 간선(Edge)들의 집합 E로 구성된 자료구조다.**  
**`G=(V, E)`**가 성립된다.  
**`V(G)`는 그래프 G의 정점(Vertex)의 집합을 말하며 `E(G)`는 그래프 G의 간선(Edge)의 집합을 말한다.** 이때, 정점의 **위치 정보**나 **간선의 순서**와 같은 정보는 그래프의 정의에 포함되지 않는다.  

# 그래프(Graph)의 종류
그래프는 Edge에 추가적인 속성(가중치 등)을 부여한다. 따라서 존재할 수 있는 간선이나 정점의 형태에 제약이 가능하다. 때문에 여러 속상을 함께 그래프가 존재한다.

## 방향그래프, 유향 그래프
<p align="center"><img src="/assets/img/blog/정보/방향.png"></p>

방향 그래프의 경우 위 사진에서 정점 1과 정점 5를 표현하면 `<1, 5>`로 표현할 수 있다. 즉, `<V1, V2>`는 V1 -> V2를 말하며 외부로 향하는 간선을 `진출차수, 외차수`라 하며 내부로 오는 간선을 `진입차수, 내차수`라 한다.  
<p align="center"><img src="/assets/img/blog/정보/무향.png"></p>

뜻 그대로 **방향이 없는 그래프** 이다.  
**무방향 그래프에서 모든 정점의 *차수(간선에 의해 직접적으로 연결된 정점의 수)*를 합하면 간선 수의 2배이다.**  
- 단순경로 : 경로 중에서 반복되는 간선이 없는 경로  
- 사이클 : 단순경로의 시작 정점과 종료 정점이 같은 경로  
  
무향(무방향)그래프에 있는 모든 정점 쌍에 대하여 **항상 경로가 존재하면** 그래프(G)는 연결되었다고 한다. 그렇지 않은 그래프는 **비연결그래프**라 표현한다.
즉, 그래프에 있는 정점이 서로 모두 연결된 그래프를 **`완전 그래프`**라 표현하며 ***무향 완전 그래프의 정점 개수가 `n`일 때, 하나의 정점은 `n-1`개의 다른 정점으로 연결된다. 따라서 정점의 개수가 n이면 `n(n-1)/2`가 성립되어 간선(E)의 개수를 구할 수 있다.***  
  
**무방향 그래프의 인접 행렬은 대칭 행렬이 된다. 무방향 그래프의 간선(A, B)가 있다면 정점 A에서 정점 B로의 연결뿐만 아니라 정점 B에서 정점 A로의 연결을 동시에 의미하기 때문이다.**  
*하지만, 방향 그래프의 인접 행렬은 일반적으로 대칭이 아니다.*

**가중치 그래프**는 간선에 `weight`, 혹은 `cost`라 불리는 속성을 부여한다. 이는 다양한 정보를 표현하는데 사용된다. **최소 스패닝 트리, 최단 경로 문제에서 자주 사용된다.**

# 오일러 경로(Eulerian Tour)
**그래프(G)에 존재하는 모든 간선(E)을 1번만 통과하면서 처음 정점(V)으로 돌아오는 경로를 말한다**. 모든 정점(V)에 연결된 간선(E)의 개수가 *짝수*일 때만 오일러 경로가 존재한다는 것을 `오일러 정리`라고 한다.  

# 그래프(Graph)로 해결할 수 있는 문제들
**그래프는 트리(Tree)와는 다르게 부모와 자식 관계에 대한 제약이 없어 훨씬 다양한 구조를 표현할 수 있다.**
1. 도로  
2. 미로  
3. 선수과목 탐색  
4. 철도망의 안전 분석(역(V), 철로(E))  
5. 소셜 네트워크 분석(사람(V), 관계도(E), BFS)  

# 인접 행렬(AdjacencyMatrix)
수학에서 행렬(Matrix)는 그래프를 표현하는데 사용된다. 프로그래밍에서 **2차원 배열은 행렬의 성질을 가진다.** 인접행렬을 이용한 그래프 표현을 각 index를 Vertex라 생각하고 Vertex가 교차하는 지점(index)를 연결된 상태(가중치 혹은 1)라고 표시하면 된다.  
**인접 행렬은 간선(Edge)의 추가/삭제가 많이 발생하면 사용한다.**

# 인접 행렬의 공간 복잡도
- 메모리 : O(V^2)  
- 시간(모든 V 탐색) : O(V^2)  
- 시간(한 노드와 연결된 모든 노드, 차수) : O(V)  
- 시간(두 노드의 연결성 확인) : O(1)  

# 인접 행렬 구현 (방향 가중치 그래프 표현)
```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class AdjacencyMatrix {
    public static void main(String[] args) throws IOException {
        // V = 정점의 개수, E = 간선의 개수
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine(), " ");
        int V = Integer.parseInt(st.nextToken());
        int E = Integer.parseInt(st.nextToken());
        int[][] graph = new int[V+1][V+1]; // 노드 0이 존재하면 V를, 노드 1이 존재한다면 배열의 크기를 임의로 1 증가
        int row, col, cost; // cost는 가중치
        for(int i = 0; i < E; i++){
            st = new StringTokenizer(br.readLine(), " ");
            row = Integer.parseInt(st.nextToken());
            col = Integer.parseInt(st.nextToken());
            cost = Integer.parseInt(st.nextToken());
            graph[row][col] = cost;
        }
        for(int i = 1 ; i < V+1; i++){
            for(int j = 1 ; j < V+1; j++){
                System.out.print(graph[i][j]+" ");
            }
            System.out.println();
        }
        br.close();
    }
}
```

## 결과

```console
5 6     // 정점의 개수와 간선의 개수 입력
5 1 1   // 각각 정점, 정점, 가중치
1 2 6
1 3 4
2 1 6
3 5 1
5 3 2

// 출력
0 6 4 0 0 
6 0 0 0 0 
0 0 0 0 1 
0 0 0 0 0 
1 0 2 0 0
```
방향 가중치 그래프는 **대칭이 아님을 알 수 있다.**

# 인접 행렬 구현 (무방향 가중치 그래프 표현)
```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class AdjacencyMatrix {
    public static void main(String[] args) throws IOException {
        // V = 정점의 개수, E = 간선의 개수
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine(), " ");
        int V = Integer.parseInt(st.nextToken());
        int E = Integer.parseInt(st.nextToken());
        int[][] graph = new int[V+1][V+1]; // 노드 0이 존재하면 V를, 노드 1이 존재한다면 배열의 크기를 임의로 1 증가
        int row, col, cost; // cost는 가중치
        for(int i = 0; i < E; i++){
            st = new StringTokenizer(br.readLine(), " ");
            row = Integer.parseInt(st.nextToken());
            col = Integer.parseInt(st.nextToken());
            cost = Integer.parseInt(st.nextToken());
            graph[row][col] = cost;
            graph[col][row] = cost; // col과 row의 반대값도 대입해주면 된다.
        }
        for(int i = 1 ; i < V+1; i++){
            for(int j = 1 ; j < V+1; j++){
                System.out.print(graph[i][j]+" ");
            }
            System.out.println();
        }
        br.close();
    }
}
```

## 결과

```console
5 6       // 정점의 개수와 간선의 개수 입력
5 1 1     // 각각 정점, 정점, 가중치
1 2 6
1 3 4
2 1 6
3 5 1
5 3 2

// 출력
0 6 4 0 1 
6 0 0 0 0 
4 0 0 0 2 
0 0 0 0 0 
1 0 2 0 0
```
무방향 가중치 그래프는 **대칭임을 알 수 있다.**

만약, 가중치가 아닌 일반 방향 그래프를 표현하고자 하면 `graph[row][col] = cost`에 `cost`가 아닌 1을 대입하면 된다. 간선 A, B가 그래프에 존재하면 1로 표현되기 때문이다.  
즉, 정점 A와 정점 B를 연결하는 정점이 있는지 판별하려면 `G[A][B]`의 값을 조사하면 된다. 또 정점의 차수 또한 인접 행렬의 행이나 열을 조사하면 알 수 있기에 `O(n)`의 연산으로 알 수 있다. 정점 A에 대한 차수는 인접 배열 i번째 행에 있는 값을 모두 더하면 된다.