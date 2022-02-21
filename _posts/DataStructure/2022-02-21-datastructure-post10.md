---
layout: post
title: "[Java, 자료구조] LinkedList를 이용한 큐(Queue) 구현하기 (2/2) - Queue 구현"
data:   2022-02-21 17:28:00+0900
categories: jekyll update
tags: [java, data structure]
---
**해당 게시글은 [[Java, 자료구조] LinkedList를 이용한 큐(Queue) 구현하기 (1/2) - 인터페이스](https://daegom.com/main/datastructure-post9/)와 이어진다.**  
해당 포스팅은 자료구조 중 하나인 **Queue**에 대해 이해하기 위해 스터디 목적으로 직접 List를 구현해보는 포스팅이다. Java에서 지원하는 Queue 클래스에 대한 사용법이 아니다. 실습을 진행할 때 사용된 코드는 모두 [Stranger's Lab](https://st-lab.tistory.com/184?category=856997)님의 게시글과 설명을 참고로 하여 구현하였다.

# QueueLinkedList 클래스 구현
추가중