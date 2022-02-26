---
layout: post
title: "[알고리즘] DFS(Depth-First Search)의 특징과 구현"
data:   2022-02-25 17:24:00+0900
categories: jekyll update
tags: [algorigtm]
---
# 그래프 탐색
**하나의 정점(Vertex)로 시작하여 모든 정점들을 한 번씩 방문하는 것을 말한다. (ex : 도시와 도로)**

# 깊이 우선 탐색(DFS, Depth-First Search)란?
**루트 노트에서 다음 분기로 넘어가기 전, 해당 분기를 완벽하게 탐색하는 것을 말한다.**  
즉, **넓게(wide) 탐색하기 전에 깊게(deep) 탐색하는 것을 말한다.** 주로 *모든 노드를 방문하고자 할 때* `DFS`를 많이 사용하며 `너비우선탐색(BFS)`보다 간단하게 구현할 수 있다. 하지만, 단순 검색 속도 자체는 **BFS에 비해 느리다.**  
또 **경로의 특징을 가지거나 미로와 같은 문제에 많이 사용된다.**

<p align="center"><img src="/assets/img/blog/정보/DFS 과정.png"></p>

# 특징
- 자기 자신을 호출하는 **재귀**의 형태를 가지고 있다.  
- Stack과 재귀를 사용하기에 **후입선출**의 구조를 가진다.  
- 전위 순회(Pre-Order Traversals)를 포함한 **다른 형태의 트리 순회는 모두 DFS의 한 종류이다.**  
- **어떠한 노드를 방문했었는지 반드시 검사해야한다.**

# 시간복잡도
**DFS는 그래프의 모든 간선을 탐색한다. `정점의 수를 N`, `간선의 수를 E`라 할 때 시간 복잡도는 다음과 같다.**  
- 인접리스트로 표현될 때 : O(N+E)  
- 인접행렬로 표현될 때  : O(N^2)  

# 구현
다음과 같은 그래프가 그러져 있다면 탐색 과정은 다음과 같다.

<p align="center"><img src="/assets/img/blog/정보/DFS graph.png"></p>

1. 시작정점 0을 방문하여 `노드 0`을 방문처리 한 뒤 인접 `노드 1`에 방문한다.  
2. `노드 1`을 방문처리하여 인접 `노드 3, 4`중 **오름차순(구현 방식에 따라 달라질 수 있다.)**을 기준으로 인접한 `노드 3`에 방문한다.  
3. `노드 3`을 방문처리하여 인접 `노드 2, 6`중 인접한 `노드 2`에 방문한다.  
4. `노드 2`를 방문처리하여 인접 `노드 5, 7`중 인접한 `노드 5`에 방문한다.  
5. `노드 5`를 방문처리하여 더 이상 인접한 노드가 없으므로 스택에서 `노드 5`를 꺼낸다.  
6. `노드 2`에 방문하지 않은 인접한 `노드 7`에 방문한다.  
7. `노드 7`을 방문처리하여 더 이상 인접한 노드가 없으므로 스택에서 `노드 7`을 꺼낸다.  
8. `노드 2`에 방문하지 않은 인접한 노드가 없으므로 스택에서 `노드 2`를 꺼낸다.  
9. `노드 3`에 방문하지 않은 인접한 `노드 6`에 방문한다.  
10. `노드 6`을 방문처리하여 더 이상 인접한 노드가 없으므로 스택에서 `노드 6`을 꺼낸다.  
11. `노드 3`에 방문하지 않은 인접한 노드가 없으므로 스택에서 `노드 3`을 꺼낸다.  
12. `노드 1`에 방문하지 않은 인접한 `노드 4`에 방문한다.  
13. `노드 4`를 방문처리하여 인접 `노드 8, 9`중 인접한 `노드 8`에 방문한다.  
14. `노드 8`을 방문처리하여 더 이상 인접한 노드가 없으므로 스택에서 `노드 8`을 꺼낸다.  
15. `노드 4`에 방문하지 않은 인접한 `노드 9`에 방문한다.  
16. `노드 9`를 방문처리한 뒤 더 이상 인접한 노드가 없으므로 스택에서 `노드 9`를 꺼낸다.  
17. 모든 스택이 비워지면 더 이상 방문할 노드가 없으므로 탐색을 종료한다.  
**최종 탐색 순서 : 0 -> 1 -> 3 -> 2 -> 5 -> 7 -> 6 -> 4 -> 8 -> 9**  

# 코드
그래프가 정상적으로 잘 만들어졌는지 확인하기 위하여 인접리스트, 인접 행렬까지 구현하였다.  
또한, DFS는 **재귀**와 같이 순환 알고리즘이다. **재귀로 구현하는 DFS와 명시적으로 Stack을 사용하여 두 가지 방법으로 구현하였다.**

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;

class CreateGraph{
    int verTex; // 정점의 개수
    int edge; // 간선의 개수
    int[][] graph; // 그래프 표현
    boolean[] visit; // 방문 여부
    private LinkedList<Integer>[] list; // 인접리스트 생성

    public CreateGraph(int vertex, int edge){
        verTex = vertex;
        this.edge = edge;
        graph = new int[vertex][vertex];
        visit = new boolean[vertex];
        list = new LinkedList[verTex];
        for(int i = 0 ; i < verTex; i++){
            list[i] = new LinkedList<>();
        }
    }

    // 그래프 방문처리 초기화
    public void graphInit(){
        // 방문 초기화
        Arrays.fill(visit, false);
    }

    // 정점과 간선을 연결
    public void makeGraphEdge(int from, int to){
        // 양방향으로 간선 연결하기
        graph[from][to] = 1;
        graph[to][from] = 1;
        // 인접리스트 표현하기
        list[from].add(to);
        list[to].add(from);
    }

    // 인접 행렬 만들기
    public void createMatrix(){
        for(int i = 0 ; i < verTex; i++){
            for(int j = 0 ; j < verTex; j++){
                System.out.print(graph[i][j]+" ");
            }
            System.out.println();
        }
    }

    // 인접 리스트 만들기
    public void createList(){
        for(int i = 0; i < list.length; i++){
            Collections.sort(list[i]);
            System.out.print(list[i]+" ");
        }
    }

    // 스택을 이용한 DFS 구현
    public void useStack(int edge){
        Stack<Integer> stack = new Stack<>();
        stack.push(edge);
        visit[edge] = true; // 방문처리
        System.out.print(edge+" ");

        while(!stack.isEmpty()){// 스택이 비어있을 때 까지
            int v = stack.peek(); // 스택에서 제거하지 않고 꺼낸다.
            boolean flag = false; // 방문 여부 판별
            for(int i = 0 ; i < verTex; i++){
                if(graph[v][i] == 1 && !visit[i]){
                    // 정점들이 서로 연결되어있고 방문하지 않았다면
                    visit[i] = true; // 방문처리
                    flag = true;
                    stack.push(i); // 스택에 넣기
                    System.out.print(i+ " ");
                    break;
                }
            }
            if(!flag){
                // 방문하지 않았다면 스택에서 꺼낸다.
                stack.pop();
            }
        }
    }

    // 재귀를 이용한 DFS 구현
    public void useRecursion(int edge){
        visit[edge] = true;
        System.out.print(edge+" ");
        for(int i = 0 ; i < verTex; i++){
            if(graph[edge][i] == 1 && !visit[i]){
                visit[i] = true;
                useRecursion(i);
            }
        }
    }
}

public class DFS {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st;

        System.out.print("정점(VerTex)의 개수 입력 : ");
        int vertex = Integer.parseInt(br.readLine());
        System.out.print("간선(Edge)의 개수 입력 : ");
        int edge = Integer.parseInt(br.readLine());

        CreateGraph createGraph = new CreateGraph(vertex, edge);
        System.out.println("간선(Edge) 입럭(from<->to)");

        for (int i = 0; i < edge; i++) {
            st = new StringTokenizer(br.readLine(), " ");
            createGraph.makeGraphEdge(Integer.parseInt(st.nextToken()), Integer.parseInt(st.nextToken()));
        }
        System.out.print("Stack을 사용한 탐색 결과 : ");
        createGraph.useStack(0);
        System.out.println();

        // 스택 방문 초기화
        createGraph.graphInit();

        System.out.print("재귀를 사용한 탐색 결과 : ");
        createGraph.useRecursion(0);
        System.out.println();

        System.out.println("== 그래프 인접행렬 표현 == ");
        createGraph.createMatrix();

        System.out.println("== 그래프 인접리스트 표현 ==");
        createGraph.createList();
    }
}
```

# 결과
```console
// 입력
정점(VerTex)의 개수 입력 : 10
간선(Edge)의 개수 입력 : 9
간선(Edge) 입럭(from<->to)
0 1
1 3
1 4
3 2
3 6
2 5
2 7
4 8
4 9

// 출력
Stack을 사용한 탐색 결과 : 0 1 3 2 5 7 6 4 8 9 
재귀를 사용한 탐색 결과 : 0 1 3 2 5 7 6 4 8 9 
== 그래프 인접행렬 표현 == 
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
== 그래프 인접리스트 표현 ==
[1] [0, 3, 4] [3, 5, 7] [1, 2, 6] [1, 8, 9] [2] [3] [2] [4] [4] 
```
**정상적으로 출력이 잘 되는 것을 확인할 수 있다.**  
<p align="center"><img src="/assets/img/blog/정보/DFS graph2.png"></p>

```console
// 입력
정점(VerTex)의 개수 입력 : 6
간선(Edge)의 개수 입력 : 5
간선(Edge) 입럭(from<->to)
0 1
0 2
1 3
2 4
2 5

// 출력
Stack을 사용한 탐색 결과 : 0 1 3 2 4 5 
재귀를 사용한 탐색 결과 : 0 1 3 2 4 5 
== 그래프 인접행렬 표현 == 
0 1 1 0 0 0 
1 0 0 1 0 0 
1 0 0 0 1 1 
0 1 0 0 0 0 
0 0 1 0 0 0 
0 0 1 0 0 0 
== 그래프 인접리스트 표현 ==
[1, 2] [0, 3] [0, 4, 5] [1] [2] [2] 
```
**다른 그래프로 실행해도 정상적으로 탐색이 잘 된다.**  
  
  
  
---  
[참고 및 리소스 출처]  
[gmlwjd9405](https://gmlwjd9405.github.io/2018/08/14/algorithm-dfs.html)  