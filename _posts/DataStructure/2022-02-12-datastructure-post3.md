---
layout: post
title: "[Java, 자료구조] Queue 인터페이스"
data:   2022-02-12 16:45:00+0900
categories: jekyll update
tags: [java, data structure]
---
# Queue 인터페이스
- 선형 자료구조로 순서가 있는 데이터를 기반으로 '`선입선출, FIFO : First-in First-out`'을 위해 만들어진 인터페이스 이다. (첫 번째로 들어온 요소가 제일 먼저 나간다.)  
- Stack(스택)의 반대 개념  
- 가장 앞쪽에 있는 요소를 Head(헤드), 가장 뒤에 있는 요소를 Tail(꼬리)라고 부른다.
- Queue를 상속하고 있는 **`Deque(덱)`**이라는 인터페이스가 있다. **`Queue`는 단방향 삽입, 삭제가 가능하지만 `Dqeuq`는 양방향 삽입 삭제가 가능하다.**  
<p align="center"><img src="/assets/img/blog/정보/큐.png"></p>
<center>
<small><i>출처 : https://coding-factory.tistory.com/602</i></small>
</center>
  
## Queue와 Deque 인터페이스를 구현하는 클래스
1. LinkedList  
2. ArrayDeque  
3. PriorityQueue
  
*분명 `LinkedList`는 List 인터페이스에서 구현이 끝났는데 왜 Queue에도 나오는거지? 라는 생각을 했다. 컬렉션의 구현도를 보니 **`LinkedList`는 List와 Deque을 구현한다. 또 Deque는 Queue 인터페이스를 구현한다.** 이를 통해 LinkedList는 리스트(List), 덱(Deque), 큐(Queue)를 사용할 수 있다. 왜 이렇게 어려울까..*

## Queue와 Deque 인터페이스에 선언된 대표적인 메소드
<p align="center"><img src="/assets/img/blog/정보/큐 메소드.png"></p>
<center>
<small><i>출처 : https://st-lab.tistory.com/142</i></small>
</center>
  
자바에서 지원하며 대중적으로 많이 사용하는 일반적인 **큐(Queue)**는 **LinkedList로 생성한 뒤 Queue로 선언하여 사용한다.  

```java
Queue<T> queue = new LinkedList<>();
Deque<T> deque = new LinkedList<>();
```

## PriorityQueue란?
- **우선순위 큐**를 말한다.
LinkedList는 Queue로 사용할 수 있지만, 해당 큐는 선입선출의 전제로 짜여있다. `PriorityQueue`는 **데이터 우선순위**에 기반하여 우선순위가 높은 데이터가 먼저 나온다. 이때 따로 정렬방식을 지정하지 않는다면 **낮은 숫자가 높은 우선순위를 가진다. 즉 sort()와 같은 순서로 데이터 우선순위를 가진다.** 이러한 특징 때문에 `PriorityQueue`는 주어진 데이터 중 **최댓값과 최솟값을 구하거나 꺼내올 때 매우 유용하다.**  
  
***! 만약, PriorityQueue을 사용하기 위해 사용자가 정의한 객체를 타입으로 사용하고자 할 시 반드시 `Comparator` 또는 `Comparable`을 통해 정렬 방식을 구현해야 한다.***

## 생성 방법

```java
// List 인터페이스와 같이 primitive type으로 구현은 불가능하다.
ArrayDeque<T> arraydeque = new ArrayDeque<>();
PriorityQueue<T> priorityQueue = new PriorityQueue<>();

Deque<T> arraydeque = new ArrayDeque<>();
Deque<T> linkedlistdeque = new LinkedList<>();

Queue<T> arraydeque = new ArrayDeque<>();
Queue<T> linkedlistdeque = new LinkedList<>();
Queue<T> priorityqueue = new PriorityQueue<>();
```
  
  
  
---
[참고 및 리소스 출처]  
[Stranger's LAB](https://st-lab.tistory.com/142)  
[TCPschool](http://www.tcpschool.com/java/java_collectionFramework_concept)  