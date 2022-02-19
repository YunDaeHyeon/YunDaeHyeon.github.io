---
layout: post
title: "[Java, 자료구조] List 인터페이스"
data:   2022-02-12 16:26:00+0900
categories: jekyll update
tags: [java, data structure]
---
# 리스트 인터페이스(List Interface)
- 선형 자료구조  
- 순서가 있는 데이터를 목록으로 사용할 수 있다.  
- List를 통해 구현된 클래스는 `동적`의 크기를 가지며 배열처럼 사용할 수 있다.  

## 리스트 인터페이스를 구현하는 클래스
1. ArrayList  
2. LinkedList  
3. Vector(를 상속받은 Stack)

### ArrayList
- Object[] 배열을 사용하여 내부 구현을 통해 동적으로 관리한다.(ex: primitive type 배열[])  
- 요소 접근(Access Elements)에는 성능이 좋지만 중간의 요소가 ***삽입 혹은 삭제*가 일어나는 경우 한 칸씩 밀거나 당겨지기에 비효율적이다.**  

### LinkedList
- 데이터(item)와 주소로 이루어진 클래스를 만들어 서로 연결하는 방식 **(데이터와 주소로 이루어진 클래스를 `Node(노트)`라고 한다.)**  
- 각 노드는 이전의 노드와 다음 노드를 연결한다. (이중 연결 리스트, 객체끼리 연결)  
- 요소를 검색해야 할 경우 처음 노드부터 찾으려는 노드가 나올 때 까지 연결된 모든 노드를 방문해야한다. (성능의 저하)  
- 특정 노드를 삭제하거나 삽입해야 할 경우 해당 노드의 링크를 끊거나 연결만 해주면 되기에 **삽입과 삭제에서는 매우 좋은 효율을 보임.**  

### Vector
- 자주 사용하지 않는 클래스  
- ArrayList와 매우 흡사하다.  
- 여러 쓰레드가 동시에 접근하려면 순차적으로 처리하도록 하다보니 멀티 쓰레드에서는 안전하지만 단일 쓰레드에서도 순차적으로 처리하기에 성능이 ArrayList에 비해 떨어진다.  

### Stack
- ***LIFO(Last in First Out)*** 혹은, `후입선출`의 구조이며 마지막에 넣은 데이터가 제일 먼저 나온다는 의미이다.  
- Vector 클래스를 상속받으며 Vector에 있는 메소드를 이용하여 구현되고 있다.  
<p align="center"><img src="/assets/img/blog/정보/스택.png"></p>
<center>
<small><i>출처 : https://velog.io/@tiiranocode/%EC%9E%90%EB%A3%8C-%EA%B5%AC%EC%A1%B0-%EC%8A%A4%ED%83%9Dstack-%ED%81%90queue</i></small>
</center>

### 리스트 인터페이스에 선언된 대표적인 메소드
<p align="center"><img src="/assets/img/blog/정보/리스트 메소드.png"></p>
<center>
<small><i>출처 : https://st-lab.tistory.com/142</i></small>
</center>

### 구현 방법

```java
ArrayList<T> arraylist = new ArrayList<>();
LinkedList<T> linkedlist = new LinkedList<>();
Vector<T> vector = new Vector<>();
Stack<T> stack = new Stack<>();

List<T> arraylist = new ArrayList<>();
List<T> linkedlist = new LinkedList<>();
List<T> vector = new Vector<>();
List<T> stack = new Stack<>();
! primitive type으로 선언은 불가능하다.
```
  
  
  
---
[참고 및 리소스 출처]  
[Stranger's LAB](https://st-lab.tistory.com/142)  
[TCPschool](http://www.tcpschool.com/java/java_collectionFramework_concept)  