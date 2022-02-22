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

# 인접 리스트(AdjacencyMatrix)

# 인접 리스트의 공간 복잡도
- 메모리 : O(V+E)  
- 시간(모든 V 탐색) : O(V+E)  
- 시간(한 노드와 연결된 모든 노드, 차수) : O(E)  
- 시간(두 노드의 연결성 확인) : O(V)  