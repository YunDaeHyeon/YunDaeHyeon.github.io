---
layout: post
title: "[자료구조] BFS의 탐색 결과가 최단 경로인 이유"
data:   2022-02-22 17:57:00+0900
categories: jekyll update
tags: [data structure]
---
# BFS
BFS는 **너비 우선 탐색** 알고리즘이다. 주로 **최단 거리를 찾고자 할 때**많이 사용되는 탐색 알고리즘이다. 대표적으로 **비 가중치 그래프에서 BFS가 사용되며 가중치 그래프에서는 다익스트라(Dijkstar) 알고리즘이 사용된다.** 자료구조 공부를 하면서 `DFS`와 달리 `BFS`가 **최단 거리**를 보장하는 이유에 대해 궁금해서 알아보았다.  

# 증명
간선 (E1, E2)가 있다고 가정하자. BFS 탐색 과정에서 간선 (E1, E2)를 통하여 정점 `E2`를 발견하여 큐에 넣었다고 하자. 또한 각 정점의 최단 거리가 `distance`라는 벡터에 들어간다고 하자.  
이때, 루트 노트(시작점)에서 `E2`까지의 최단거리 `distance[E2]`는 **시작점에서 `E1`까지의 최단 거리 `distance[E1]`에 1을 더한 것이다.**  

이유는 아래와 같다.  
시작점(루트 노트)부터 `E1`까지의 경로에서 (E1, E2) 간선 하나를 더 붙이면 `distance[E1] + 1`길이의 경로가 나와 `distance[E2]`가 `distance[E1] + 1`보다 클 수 없을 것이다. 이를 **귀류법으로 증명할 수 있다.**  
 시작점에서 `E2`까지의 **최단경로가 `distance[E1] + 1`보다 짧다고 가정할 시, 해당 경로상에서 `E2`바로 이전의 정점은 거리가 `distance[E1]`보다 작아야 한다. (큐는 시작점에서 가까운 순서부터 들어간다.) 하지만 그러려면 `E2`는 `E1`보다 이전에 방문했어야 하고 (E2, E1)을 통해서 `E2`를 방문했다는 가정과 모순이 된다. 따라서, 시작점(루트 노트)에서 `E2`까지의 최단 거리는 시작점(루트 노트)에서 `E1`까지의 최단 거리에다가 1을 더한 것임을 알 수 있다.**

# 예시
<p align="center"><img src="/assets/img/blog/정보/BFS 증명.png"></p>

위와 같은 그래프가 있다고 가정하자.
해당 그래프의 BFS 탐색 결과는 *1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7 -> 8*이다. **BFS가 해당 그래프의 노드를 방문하는 순서를 살펴보자.**  
1. 시작노드 `1`을 방문한다.  
2. 시작노드 `1`에 근접한 노드를 모두 **방문하고 순서대로 큐에 넣는다.(2, 3 순으로 큐에 들어간다.)**  
3. 시작노드 `1`에 근접한 노드들을 모두 방문하였다면 큐 맨 앞에 있는 노드를 뺀다. **(큐는 선입선출의 구조를 가지고 있기에 2가 먼저 빠진다.)**  
4. 노드 `2`에 근접한 노드 `4`를 큐에 넣고 **다시 큐에서 하나를 뺀다.(3이 빠진다.)**  
5. 노드 `3`에 근접한 노드 `5, 6`을 순서대로 큐에 넣는다.  

위와 같은 탐색 방법으로 진행된다.    
즉, **처음에 시작 노드를 방문하고 그 다음에는 시작 노드와 인접한 노드를 순서대로 방문한다.** 위 그래프에서는 시작노드 `1`과 그에 인접한 노드 `2`, `3`이 해당한다. 이들은 **시작 노드에서 거리가 `1`인 노드라고 할 수 있다.** 즉, BFS는 깊이가 아닌 너비. 즉, **거리**로 텀색을 진행한다.  
그 뒤 방금 거리 1에 해당하는 노드들에 인접한 노드들을 탐색하는데 그들은 **시작점에서 거리가 2인 노드들이다. (위 그래프로 따지면 노드 `4, 5, 6`이 해당)** 또 다음으로 방문하는 노드들은 시작점에서 거리가 3인 노드들 `7, 8`이다.  
이런 식으로 **거리가 1인 노드를 방문하고, 거리가 2인 노드를 방문하고, 거리가 3인 노드를 모두 방문하는 순서대로 탐색이 진행된다.**
  
---
  
그래서 BFS가 찾은 경로가 **최단 경로**가 맞는지 증명할 수 있다. ***시작노드 `1`에서 노드 `6`까지의 최단 경로를 구하고 싶다 하자.*** 당연히 우리가 볼때는 **1 -> 3 -> 6**이다. 이를 증명하면 아래와 같다.  
**BFS 탐색이 진행되면서 노드 `6`을 방문하는 순간이 있을 것이다.** 방문하는 순간에는 **거리가 1인 노드는 모두 방문했을 것이고 거리가 2인 노드를 방문하고 있을 것이다.** 만약, ***시작노드 `1`에서 `6`까지의 최단 거리가 1이라면 1인 노드들을 찾을 때 노드 `6`을 방문했을 것이다.*** 하지만, 거리가 2인 노드들을 방문했을 때 노드 `6`을 발견했다는 것은 **시작노드 `1`에서 노드 `6`까지의 최단 거리는 2일수 밖에 없다는 것이다.** 즉, 노드 `6`을 처음 방문한 그 순간이 `노드 6`을 가장 빠르게 찾은 것이므로 **이떄의 시작점에서부터 노드 `6`까지의 경로를 저장해 놓으면 그것이 최단 경로이다. 또한 이를 Predecessor(선행자, 트리에서 어떠한 순서로 순회할 때 특정 노드를 방문하기 직전에 방문한 노드를 말한다.) 변수로 이용한다.**  
이때, Predecessor는 *특정 노드를 방문하기 직전에 방문한 노드이기에 특정 노드의 거리가 i라면 Predecessor는 i-1이다.*  
위 그래프에서 보면 노드 `6`이 가장 빨리 발견되는 경로는 Predecessor 노드 `3`이며, 노드 `3`이 가장 빨리 발견되는 경로는 Predecessor 노드 `1`이다. 이때 노드 `1`의 Predecessor 노드는 존재하지 않기에 알고리즘이 종료된다. Predecessor를 통해 **각 노드는 어떤 노드를 통해서 오는게 가장 빠른 지를 저장하기에 비가중치 그래프 BFS에서 Predecessor를 통해 찾은 경로가 최단 경로라고 할 수 있다.**  
  
---
  
이해를 위해 다시 확인하자. 위 그래프에서 시작노드 `1`에서 노드 `8`까지의 최단 거리를 구해보자.  
1. 노드 `8`의 Predecessor 노드는 `5`이다.  
2. 노드 `5`의 Predecessor 노드는 `3`이다. Predecessor는 방문하기 직전의 노드이므로 노드의 거리가 방문한 노드보다 작아야하므로 노드 `4`가 아닌 노드 `3`이 Predecessor 노드가 된다.  
3. 노드 `3`의 Predecessor 노드는 `1`이다.  
4. 노드 `1`의 Predecessor 노드는 존재하지 않기에 시작노드이며 알고리즘이 종료된다.  
5. 해당 Predecessor 노드들을 나열하면 8 -> 5 -> 3 -> 1 이다. 이를 거꾸로 뒤집어주면 1 -> 3 -> 5 -> 8이 되므로 최단 경로를 구할 수 있다.  

**이런 식으로 원하는 노드부터 시작 노드까지 거꾸로 찾아가는 기법을 `BackTracking(백트래킹)`이라 부른다.**  
  
  
---
[참고]  
[kasterra](https://velog.io/@kasterra/%ED%95%B5%EC%8B%AC-%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-%EA%B7%B8%EB%9E%98%ED%94%84-%EC%B5%9C%EB%8B%A8-%EA%B2%BD%EB%A1%9C-%ED%83%90%EC%83%89)  
[codermun](https://codermun-log.tistory.com/294?category=927598)  
[nulls](https://nulls.co.kr/graph/141)  