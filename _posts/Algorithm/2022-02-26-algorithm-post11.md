---
layout: post
title: "[알고리즘] BFS(Breadth-first search)의 특징과 구현"
data:   2022-02-26 16:41:00+0900
categories: jekyll update
tags: [algorigtm]
---
# 그래프 탐색
**하나의 정점(Vertex)로 시작하여 모든 정점들을 한 번씩 방문하는 것을 말한다. (ex : 도시와 도로)**

# 너비 우선 탐색(BFS, Breadth-first search)란?
**루트노트에서 시작하여 인접한 노드를 먼저 탐색하는 기법이다. 시작 정점으로부터 `가까운 정점`을 먼저 방문하고 `멀리 떨어져 있는` 정점을 나중에 방문한다.**  
즉, **깊게(deep) 탐색하기 전에 넓게(wide) 탐색하는 것을 말한다.** 주로 *두 노드 사이의 최단 경로를 찾고싶을 때* 사용한다. **`DFS`는 모든 정점의 관계를 살펴야 하지만 BFS는 가까운 관계부터 탐색한다.**  
또 **경로의 특징을 가지거나 미로와 같은 문제에 많이 사용된다.** 만약, **그래프가 정말 크다면 `DFS`, 검색 대상의 규모가 크지 않고 시작 노드부터 원하는 노드까지 거리가 가까우면 `BFS`를 사용한다.**

<p align="center"><img src="/assets/img/blog/정보/BFS.png"></p>

# 특징
- 모든 경로를 동시에 탐색한다.  
- 직관적이지 않다. 즉, **BFS는 시작 노드에서 시작하여 거리에 따라 단계별로 탐색한다.**  
- BFS는 재귀적으로 동작하지 않는다.  
- **가중치가 없는** 그래프의 경우 시작 노드부터 원하는 노드의 최단 경로를 알아낼 수 있다. (가중치가 있을 때 최단거리는 **다익스트라 알고리즘**을 사용한다.)  
- `Queue`를 사용하기에 **선입선출**의 구조를 가지고 있다.  
- **어떠한 노드를 방문했었는지 반드시 검사해야한다.**  

# 시간복잡도
**DFS와 마찬가지로 그래프 내의 적은 숫자의 간선만을 가지는 `희소 그래프`의 경우 인접 행렬보다 인접 리스트를 사용하는 것이 유리하다. `정점의 수를 N`, `간선의 수를 E`라 할 때 시간 복잡도는 다음과 같다.**  
- 인접 리스트로 표현될 때 : O(N+E)  
- 인접 행렬로 표현될 때  : O(N^2)  

# 구현
다음과 같은 그래프가 그러져 있다면 탐색 과정은 다음과 같다.  
<p align="center"><img src="/assets/img/blog/정보/DFS graph.png"></p>
  
1. `노드 0`을 큐에 넣고 방문처리 한다.  
2. `노드 0`에 인접한 노드 `노드 1`을 큐에 넣는다.  
3. `노드 1`을 방문처리 한 뒤 인접한 노드 `노드 3, 4`를 모두 큐에 넣고 방문처리 한다.   
4. `노드 1`에 더 이상 인접한 노드가 없어 **큐(선입선출)에 먼저 들어온 `노드 3`부터 `노드 4`와 인접한 노드를 큐에 넣고 방문처리 한다.(2, 6, 8, 9의 순서로 큐에 들어가게 된다.)**  
5. `노드 3`과 `노드 4`에 더 이상 인접한 노드가 없기에 **큐(선입선출)에 먼저 들어온 `노드 2`부터 `노드 6`, `노드 8`, `노드 9`의 순서로 탐색을 진행한다.**  
6. `노드 2`에 인접한 `노드 5, 7`을 큐에 넣은 뒤 방문처리 한다.  
7. 더 이상 인접한 노드가 없기에 탐색을 종료한다.  
**최종 탐색 순서 : 0 -> 1 -> 3 -> 4-> 2 -> 6 -> 8 -> 9 -> 5 -> 7**  
  
# 코드
그래프가 정상적으로 잘 만들어졌는지 확인하기 위하여 인접리스트, 인접 행렬까지 구현하였다.  
또한, BFS는 **재귀적으로 동작하지 않는 알고리즘이다.** **큐(Queue)를 사용하기에 Queue와 LinkedList를 명시적으로 구현하여 코드를 작성하였다.**  

```java
package bfs;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;

class CreateBFS{
    int vertex; // 정점의 수
    int edge; // 간선의 수
    int[][] graph; // 그래프
    boolean[] visited; // 방문 체크
    private LinkedList<Integer>[] list; // 인접리스트 생성

    public CreateBFS(int vertex, int edge){
        this.vertex = vertex;
        this.edge = edge;
        graph = new int[vertex][vertex];
        visited = new boolean[vertex];
        list = new LinkedList[vertex];
        for(int i = 0 ; i < vertex; i++){
            list[i] = new LinkedList<>();
        }
    }

    public void graphInit(){
        Arrays.fill(visited, false);
    }

    public void makeGraphEdge(int from, int to){
        // 양방향 간선 연결
        graph[from][to] = 1;
        graph[to][from] = 1;
        // 인접리스트 표현
        list[from].add(to);
        list[to].add(from);
    }

    public void createMatrix(){
        for(int i = 0 ; i < vertex; i++){
            for(int j = 0 ; j < vertex; j++){
                System.out.print(graph[i][j]+" ");
            }
            System.out.println();
        }
    }

    public void createList(){
        for(int i = 0 ; i < list.length; i++){
            Collections.sort(list[i]);
            System.out.print(list[i]+" ");
        }
    }

    // 파라미터는 시작지점 노드를 지정한다.
    public void useBFS(int startNode){
        // 선입선출
        Queue<Integer> queue = new LinkedList<>();
        queue.offer(startNode); // 처음 시작지점을 큐에 넣는다.
        visited[startNode] = true; // 시작 정점 방문 처리

        // 큐에 있는 모든 정점에 방문할 때 까지 (큐가 텅 빌때까지)
        while(!queue.isEmpty()){
            int temp = queue.poll(); // 큐에 있는 정점을 하나 뺀다.
            System.out.print(temp+" ");

            for(int i = 0; i < vertex; i++){
                if(graph[temp][i] == 1 && !visited[i]){ // graph[temp][i] == 1은 graph[from][to] == 1와 같은 의미
                    queue.offer(i);
                    visited[i] = true;
                }
            }
        }
    }

}

public class BFS {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        System.out.println("정점(vertex)과 간선(edge)의 개수를 입력하세요");
        StringTokenizer st = new StringTokenizer(br.readLine(), " ");
        int vertex = Integer.parseInt(st.nextToken()); // 정점의 개수 입력
        int edge = Integer.parseInt(st.nextToken()); // 간선의 개수 입력
        CreateBFS createBFS = new CreateBFS(vertex, edge);
        System.out.println("간선(Edge) 입력(from <-> to)");
        for(int i = 0 ; i < edge; i++){
            st = new StringTokenizer(br.readLine()," ");
            createBFS.makeGraphEdge(Integer.parseInt(st.nextToken()),Integer.parseInt(st.nextToken()));
        }
        System.out.print("BFS 탐색 결과 : ");
        createBFS.useBFS(0); // BFS 탐색 시작
        System.out.println();
        System.out.println("== 그래프 인접 행렬 표현 ==");
        createBFS.createMatrix();
        System.out.println("== 그래프 인접 리스트 표현 ==");
        createBFS.createList();
    }
}
```

# 결과
```console
정점(vertex)과 간선(edge)의 개수를 입력하세요
10 9    // 입력
간선(Edge) 입력(from <-> to)
0 1     // 입력
1 3
1 4
3 2
3 6
2 5
2 7
4 8
4 9

// 출력
BFS 탐색 결과 : 0 1 3 4 2 6 8 9 5 7 
== 그래프 인접 행렬 표현 ==
0 1 0 0 0 0 0 0 0 0 
1 0 0 1 1 0 0 0 0 0 
0 0 0 1 0 1 0 1 0 0 
0 1 1 0 0 0 1 0 0 0 
0 1 0 0 0 0 0 0 1 1 
0 0 1 0 0 0 0 0 0 0 
0 0 0 1 0 0 0 0 0 0 
0 0 1 0 0 0 0 0 0 0 
0 0 0 0 1 0 0 0 0 0 
0 0 0 0 1 0 0 0 0 0 
== 그래프 인접 리스트 표현 ==
[1] [0, 3, 4] [3, 5, 7] [1, 2, 6] [1, 8, 9] [2] [3] [2] [4] [4] 
```
**정상적으로 출력이 잘 되는 것을 확인할 수 있다.**  
<p align="center"><img src="/assets/img/blog/정보/DFS graph2.png"></p>

```console
정점(vertex)과 간선(edge)의 개수를 입력하세요
6 5     // 입력
간선(Edge) 입력(from <-> to)
0 1     // 입력
0 2
1 3
2 4
2 5

// 출력
BFS 탐색 결과 : 0 1 2 3 4 5 
== 그래프 인접 행렬 표현 ==
0 1 1 0 0 0 
1 0 0 1 0 0 
1 0 0 0 1 1 
0 1 0 0 0 0 
0 0 1 0 0 0 
0 0 1 0 0 0 
== 그래프 인접 리스트 표현 ==
[1, 2] [0, 3] [0, 4, 5] [1] [2] [2] 
```
**다른 그래프로 실행해도 정상적으로 탐색이 잘 된다.**  
  
  
  
  
---  
[참고 및 출처]  
[seeminglyjs](https://seeminglyjs.tistory.com/255)  
