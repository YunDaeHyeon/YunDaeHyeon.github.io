---
layout: post
title: "[Java, 자료구조] LinkedList를 이용한 큐(Queue) 구현하기 (1/2) - 인터페이스"
data:   2022-02-21 10:15:40+0900
categories: jekyll update
tags: [java, data structure]
---
# 큐(Queue)란?
큐(Queue)는 **후입선출(LIFO, Last in First Out)의 구조를 가지고 있는 스택(Stack)과 달리** ***선입선출(FIFO, First in First Out)의 구조를 가지고 있다.*** 편의점 음료수 판매대, 놀이공원 어트렉션의 줄 처럼 가장 먼저 들어온 사람이 가장 먼저 나간다. 말 그래도 `대기열`을 말한다.  
`큐(Queue)`는 요소의 한쪽 끝(Rear)에서는 오직 **삽입**연산만 이루어지며 다른 한쪽 끝(Front)는 오직 **삭제**연산만 이루어지는 **유한순서**를 가지고 있다. **이는 구조적으로 먼저 삽입된 데이터가 가장 먼저 삭제되는 특징을 가지고 있다.(FIFO)**
<p align="center"><img src="/assets/img/blog/정보/큐.png"></p>
<center><small><i>사진 출처 : https://coding-factory.tistory.com/602</i></small> <br>
</center>
  
**위 리소스에서 `Enqueue`는 큐 맨 뒤에 데이터를 추가(Rear, 삽입연산)하며 `Dequeue`는 큐 맨 앞쪽의 데이터를 삭제한다.(Front, 삭제연산)**

# 특징
- Queue는 제일 먼저 들어간 데이터가 가장 먼저 나오는 ***선입선출(FIFO, First in First Out)의 구조를 가지고 있다.***  
- Queue는 한쪽 끝을 `Rear`로 정하여 오직 **삽입**연산만 진행하며 다른 한쪽 끝을 `Front`로 정하여 오직 **삭제**연산만 진행한다.  
- **넓이 우선 탐색(BFS)에서 사용된다.**  
- 컴퓨터 버퍼에서 주로 사용되며, 데이터 처리가 힘들 때 버퍼(Queue)를 만들어 데이터를 대기시킨다. (즉, 여러 데이터가 한꺼번에 입력될 때 처리하기 용이하다.)  
- Queue에서의 데이터 추가는 `offer`이며, 데이터 삭제는 `poll`이다.
- Queue에서 Queue의 가장 앞에 있는 원소의 이름은 `Front`이며, 가장 뒤에 있는 원소의 이름은 `Rear`이다. (Stack에서의 Top, Bottom이나 List의 Head, Tail과 같다.)  

---
해당 포스팅은 자료구조 중 하나인 **Queue**에 대해 이해하기 위해 스터디 목적으로 직접 Queue를 구현해보는 포스팅이다. Java에서 지원하는 Queue 클래스에 대한 사용법이 아니다. 실습을 진행할 때 사용된 코드는 대부분 [Stranger's Lab](https://st-lab.tistory.com/181)님의 게시글과 설명을 참고로 하여 구현하였다.  

# 단일 연결리스트(LinkedList)를 이용한 Queue 인터페이스 구현하기
대중적으로 자바에서 많이 사용되는 큐(Queue)는 `LinkedList`를 이용한다. *([LinkedList](https://daegom.com/main/datastructure-post7/)에 대한 설명)*  
```java
Queue<Integer> queue = new LinkedList<>();
```
`LinkedList`는 배열이 아닌 `Node`를 이용하여 구현이 된다. Node(노드)는 데이터와 다음 노드를 가리키는 변수로 이루어져 있다. 즉, 단일 연결리스트(SinglyLinkedList) 자체에 큐(Queue)의 기능을 대입한다.  
큐에 **[1, 2, 3, 4]**라는 데이터가 있다면 큐의 특성 상 데이터의 추가는 Rear(큐의 끝)에서 진행되며 8이라는 데이터를 해당 큐에 대입(offer) 큐 **[1, 2, 3, 4]**에서 원소 `4`가 Rear의 상태이기에 `4` 앞에 `8`이 추가되어 **[1, 2, 3, 4, 5]**가 된다. 이때, LinkedList의 Queue 구현은 `Node`를 이용하기에 4에 해당하는 노드는 tail이자 rear이다. 따라서 래퍼런스가 null값을 가지고 있기에 `5`라는 노드가 추가된다면 기존 tail이자 rear을 `5`로 바꿈과 동시에 기존 원소 `4`의 래퍼런스 값에 원소 `5`를 가리키는 래퍼런스를 대입한다. (offer에 대한 설명)  

# Queue 인터페이스에서 구현할 메소드
1. offer(E e) - return type : boolean - 큐의 마지막(rear)에 데이터를 추가한다.  
2. poll() - return type : E - 큐의 제일 첫 요소(front)를 삭제하고 삭제한 데이터를 반환한다.  
3. peek() - return type : E - 큐의 제일 첫 요소(front)를 제거하지 않고 반환한다.  
설명과 같이 `offer()`는 리스트에서 `add()`와 비슷하며 `poll()`은 `remove()`, `peek()`는 `element()`와 비슷한 역할을 수행한다.  

## 인터페이스 코드
```java
public interface Queue<E> { // 제네릭 선언

    // 큐의 가장 마지막(rear)에 요소를 추가한다.
    // 큐가 정상적으로 추가되면 true를, 그렇지 않다면 false를 반환한다.
    boolean offer(E e);

    // 큐의 첫 번째 요소(front)를 삭제하고 삭제 된 요소를 반환한다.
    E poll();

    // 큐의 첫 번째 요소(front)를 반환한다.
    E peek();
}
```
또한, `LinkedList`를 이용하여 `Queue` 구현하기 때문에 `Node`를 구현해야할 필요가 있다.  

## 노드(Node) 구현
```java
public class Node<E>{
    E data; // 노드의 데이터를 의미한다.
    Node<E> next; // 다음 노드를 가리키는 객체이다.
    Node(E data){
        // 첫 노드가 생성되면 데이터는 존재하지만 리스트에 노드는 자기 자신만 존재한다.
        // 따라서 다음 노드를 가리키는 래퍼런스가 존재하지 않아 최초 생성 노드의 래퍼런스(next)는 null로 초기화 시킨다.
        this.data = data;
        this.next = null;
    }
}
```
  
  
  
---  
[참고 및 리소스 출처]  
[Stranger's LAB](https://st-lab.tistory.com/181)  
[코딩팩토리](https://coding-factory.tistory.com/602)  