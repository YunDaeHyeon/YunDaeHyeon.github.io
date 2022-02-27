---
layout: post
title: "[알고리즘, DFS/BFS] Java - 미로 탈출"
data:   2022-02-26 19:18:00+0900
categories: jekyll update
tags: [algorigtm]
---
# DFS/BFS - 미로 탈출
동빈이는 N x M 크기의 직사각형 형태의 미로에 갇혀 있다. 미로에는 여러 마리의 괴물이 있어 이를 피해 탈출해야 한다.  
동빈이의 위치는 (1, 1)이고 미로의 출구는 (N, M)의 위치에 존재하며 한 번에 한 칸씩 이동할 수 있다. 이때 괴물이 있는 부분은 0으로, 괴물이 없는 부분은 1로 표시되어 있다. 미로는 반드시 탈출 할 수 있는 형태로 제시된다. 이때, 동빈이가 탈출하기 위해 움직여야 하는 최소 칸의 개수를 구하시오. *칸을 셀 때는 시작 칸과 마지막 칸을 모두 포함해서 계산한다.*  
  
[ 입력 조건 ]  
- 첫째 줄에 두 정수 N, M이 주어진다. 다음 N개의 줄에는 각각 M개의 정수(0 혹은 1)로 미로의 정보가 주어진다. 각각의 수들은 공백 없이 붙어서 입력으로 제시된다. 또한 시작 칸과 마지막 칸은 항상 1이다.  
  
[ 출력 조건 ]  
- 첫째 줄에 최소 이동 칸의 개수를 출력한다.  
  
[ 입력 예시 ]  
5 6  
101010  
111111  
000001  
111111  
111111  
  
[ 출력 예시 ]  
10  
  
# 내가 작성한 코드
```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.LinkedList;
import java.util.Queue;
import java.util.StringTokenizer;

class Node{
    private int x;
    private int y;
    public Node(int x, int y){
        this.x = x;
        this.y = y;
    }

    public int getX(){
        return this.x;
    }

    public int getY(){
        return this.y;
    }
}

public class Question2 {
    public static int N, M;
    public static int[][] graph;
    public static int[] PositionX = {0, 0, -1, 1}; // 상 하 좌 우
    public static int[] PositionY = {-1, 1, 0, 0}; // 상 하 좌 우
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine(), " "); // N, M 입력받음
        N = Integer.parseInt(st.nextToken());
        M = Integer.parseInt(st.nextToken());
        graph = new int[N][M];
        for(int i = 0; i < graph.length; i++){
            String[] temp = br.readLine().split("");
            for(int j = 0; j < graph[i].length; j++){
                graph[i][j] = Integer.parseInt(temp[j]);
            }
        }
        dfs(0,0);
    }
    public static void dfs(int x, int y){
        Queue<Node> queue = new LinkedList<>();
        queue.offer(new Node(x, y));
        while(!queue.isEmpty()){
            Node node;
            node = queue.poll();
            x = node.getX();
            y = node.getY();
            for(int i = 0 ; i < PositionX.length; i++){
                int dx = x + PositionX[i]; // 이동
                int dy = y + PositionY[i];
                if(dx < 0 || dy < 0 || dx >= N || dy >= M) continue; // 이동한 값이 미로를 벗어나면
                if(graph[dx][dy] == 0) continue; // 이동한 값이 몬스터라면
                if(graph[dx][dy] == 1){
                    graph[dx][dy] = graph[x][y] + 1;
                    queue.offer(new Node(dx, dy));
                }
            }
        }
        System.out.println(graph[N-1][M-1]); // 최단 거리 반환
    }
}

```

# 결과
case 1 :  
```console
// 입력
5 6
100000
111110
011110
010010
011111
// 출력
10
```
case 2 :  
```console
// 입력
3 3
111
101
111
// 출력
5
```
case 3 :  
```console
// 입력
6 6
111111
010010
011110
010010
010010
011111
// 출력
11
```

# 설명
```java
class Node{
    private int x;
    private int y;
    public Node(int x, int y){
        this.x = x;
        this.y = y;
    }

    public int getX(){
        return this.x;
    }

    public int getY(){
        return this.y;
    }
}
```
BFS를 탐색하려고 했더니 검색 대상이 `X`와 `Y`로 이루어진 2차원 배열(행렬)이었다. BFS 탐색을 위해 `X`, `Y`로 이루어진 클래스 `Node`를 생성하여 `Node`자체를 큐에 넣고 빼는 식으로 구현하였다.  

```java
    public static int N, M;
    public static int[][] graph;
    public static int[] PositionX = {0, 0, -1, 1}; // 상 하 좌 우
    public static int[] PositionY = {-1, 1, 0, 0}; // 상 하 좌 우
```
`N, M`은 전역으로 사용하기 위하여 `static`으로 선언하였다.  
`graph`는 미로를 만들기 위한 2차원 배열(행렬)이다.  
`PositionX`, `PositionY`는 **상, 하, 좌, 우**에 해당하는 좌표 값이다.  

```java
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine(), " "); // N, M 입력받음
        N = Integer.parseInt(st.nextToken());
        M = Integer.parseInt(st.nextToken());
        graph = new int[N][M];
        for(int i = 0; i < graph.length; i++){
            String[] temp = br.readLine().split("");
            for(int j = 0; j < graph[i].length; j++){
                graph[i][j] = Integer.parseInt(temp[j]);
            }
        }
```
`BufferedReader`을 사용하여 `StringTokenizer`로 입력받은 문자열을 나누었고 `.split()`메소드를 사용하여 미로의 정보를 입력받았다.  

```java
dfs(0,0);
```
미로의 처음과 끝은 항상 **`1`**이다. 첫 시작지 x, y를 dfs메소드에 보낸다.  

```java
    public static void dfs(int x, int y){
        Queue<Node> queue = new LinkedList<>();
        queue.offer(new Node(x, y));
        while(!queue.isEmpty()){
            Node node;
            node = queue.poll();
            x = node.getX();
            y = node.getY();
            for(int i = 0 ; i < PositionX.length; i++){
                int dx = x + PositionX[i]; // 이동
                int dy = y + PositionY[i];
                if(dx < 0 || dy < 0 || dx >= N || dy >= M) continue; // 이동한 값이 미로를 벗어나면
                if(graph[dx][dy] == 0) continue; // 이동한 값이 몬스터라면
                if(graph[dx][dy] == 1){
                    graph[dx][dy] = graph[x][y] + 1;
                    queue.offer(new Node(dx, dy));
                }
            }
        }
        System.out.println(graph[N-1][M-1]); // 최단 거리 반환
    }
```

```java
        Queue<Node> queue = new LinkedList<>();
        queue.offer(new Node(x, y));
```
탐색을 진행해야할 대상이 2개이므로 `Node`자체를 큐에 넣기 위하여 `Queue`를 `Node`타입으로 선언한 뒤 매개변수로 들어온 x와 y를 큐에 넣는다.  

```java
        while(!queue.isEmpty()){
            Node node;
            node = queue.poll();
            x = node.getX();
            y = node.getY();
```
while문은 **큐가 텅 빌 때까지 반복한다.**  
또한, `poll()`을 통하여 node 객체에 큐를 뺀 뒤 x와 y에 대입한다.  

```java
            for(int i = 0 ; i < PositionX.length; i++){
                int dx = x + PositionX[i]; // 이동
                int dy = y + PositionY[i];
                if(dx < 0 || dy < 0 || dx >= N || dy >= M) continue; // 이동한 값이 미로를 벗어나면
                if(graph[dx][dy] == 0) continue; // 이동한 값이 몬스터라면
                if(graph[dx][dy] == 1){
                    graph[dx][dy] = graph[x][y] + 1;
                    queue.offer(new Node(dx, dy));
                }
            }
```
현재 위치를 대상으로 **상, 하, 좌, 우를 한 번씩은 탐색을 해야하니 움직이는 횟수만큼 반복문을 실행한다.** 그 뒤, dx, dy에 현재 x와 y값과 상, 하, 좌, 우에 해당하는 변수들을 더하여 **움직인 후의 좌표를 구한다.** *(x, y : 현재 좌표 / dx, dy : 움직인 후 좌표)*  

```java
if(dx < 0 || dy < 0 || dx >= N || dy >= M) continue;
```
움직인 후 좌표가 배열(미로)보다 크거나 작으면 무시하고 한 번 진행한다.  

```java
if(graph[dx][dy] == 0) continue;
```
움직인 후 좌표가 0(벽, 몬스터)일 경우 무시하고 한 번 진행한다.  

```java
    if(graph[dx][dy] == 1){
        graph[dx][dy] = graph[x][y] + 1;
        queue.offer(new Node(dx, dy));
    }
```
움직인 후 좌표가 1이면 움직인 후 좌표에 해당하는 배열 값에 **현재 좌표 + 1**을 하여 1씩 증가한다. 그 뒤 **움직인 후 좌표를 큐에 넣는다.**  
이때 가장 중요한 코드가 **`graph[dx][dy] = graph[x][y] + 1`**이다. 해당 코드로 인하여 BFS의 최단 거리 탐색이 이루어진다.  
`graph[x][y] + 1`는 일종의 predecessor 변수이다. 즉, **특정한 노드를 방문하면 그 `이전노드`의 거리에 1을 더한 값을 삽입한다.**  
아래와 같이 3x3으로 이루어진 배열이 있다.  
1 1 0 -> (1,1) (1,2) (1,3)  
0 1 0 -> (2,1) (2,2) (2,3)  
0 1 1 -> (3,1) (3,2) (3,3)  
이때, 맨 처음의 위치는 (1,1)이라고 문제에 언급되어 있다.  
`if(graph[dx][dy] == 1)`을 통하여 (1,1)에서 상, 하, 좌, 우로 이동하면 (1,2)로 이동한다. `graph[x][y] (현재위치)`에 해당하는 값은 1이다. `graph[dx][dy] (움직인 후 위치)`에 **현재 위치 +1**한 값을 넣어 **이전노드의 거리에 1을 더한 값을 움직힌 후 노드에 삽입하면 최단 경로의 값들이 1씩 증가하는 형태가 된다.**  
1 2 0  
0 3 0  
0 4 5  
이러한 형식으로 최단 거리를 BFS 탐색을 사용하여 구할 수 있다.  
  
  
  

---    
[문제 출처]  
[나동빈님](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9791162243077)