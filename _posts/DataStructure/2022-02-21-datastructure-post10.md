---
layout: post
title: "[Java, 자료구조] LinkedList를 이용한 큐(Queue) 구현하기 (2/2) - Queue 구현"
data:   2022-02-21 18:29:00+0900
categories: jekyll update
tags: [java, data structure]
---
**해당 게시글은 [[Java, 자료구조] LinkedList를 이용한 큐(Queue) 구현하기 (1/2) - 인터페이스](https://daegom.com/main/datastructure-post9/)와 이어진다.**  
해당 포스팅은 자료구조 중 하나인 **Queue**에 대해 이해하기 위해 스터디 목적으로 직접 List를 구현해보는 포스팅이다. Java에서 지원하는 Queue 클래스에 대한 사용법이 아니다. 실습을 진행할 때 사용된 코드는 모두 [Stranger's Lab](https://st-lab.tistory.com/184?category=856997)님의 게시글과 설명을 참고로 하여 구현하였다.

## 구현할 메소드
- public boolean offer(E value) `@Override` : 큐 tail(rear)에 데이터를 추가한다. 정상적으로 삭제되었다면 true를 반환한다.  
- public E poll() `@Override` : 큐 head(front)에 있는 데이터를 삭제한다. 삭제할 요소가 없다면 `null`을 반환한다.  
- public E remove() : 큐 head(front)에 있는 데이터를 삭제한다. 삭제할 요소가 없다면 `NoSuchElementException`을 반환한다.  
- public E peek() `@Override` : 큐 head(front)에 있는 데이터를 가져온다. 가져올 요소가 없다면 `null`을 반환한다.  
- public E element() : 큐 head(front)에 있는 데이터를 가져온다. 가져올 요소가 없다면 `NoSuchElementException`을 반환한다.  
- public int size() : 큐에 있는 요소의 개수를 가져온다.  
- public boolean isEmpty() : 큐가 비어있는지 판별한다. 큐가 비어있다면 true를, 비어있지 않다면 false를 반환한다.  
- public boolean contains(Object value) : 찾고자 하는 데이터가 큐에 있는지 찾는다. 찾고자 하는 데이터가 큐에 있다면 true를, 없다면 false를 반환한다.  
- public void clear() : 큐를 초기화 시킨다.  

## 1. QueueLinkedList와 변수, 생성자
```java
import java.util.NoSuchElementException;

public class QueueLinkedList<E> implements Queue<E> {
    private Node<E> head; // 큐에서 가장 앞에 있는 노드 객체(front)를 가리킨다.
    private Node<E> tail; // 큐에서 가장 뒤에 있는 노드 객체(rear)를 가리킨다.
    private int size; // 큐에 있는 노드의 개수

    QueueLinkedList(){
        this.head = null;
        this.tail = null;
        this.size = 0;
    }
```
[변수 설명]  
- Node\<E\> head : 큐에서 가장 앞에 있는 노드 객체(front)를 가리킨다.  
- Node\<E\> tail : 큐에서 가장 뒤에 있는 노드 객체(rear)를 가리킨다.  
- int size : 큐에 있는 노드의 개수를 말한다.
생성자가 처음 호출되는 시점은 큐가 비어있는 상태이니 아무런 데이터가 존재하지 않기에 **큐에 있는 노드의 개수(size)의 값은 0이며 head(front)와 tail(rear)또한 존재하지 않는다.**  

## 2. offer 구현
```java
    @Override
    public boolean offer(E value) {
        Node<E> newNode = new Node<E>(value);
        
        if(size == 0){ // 만약 큐가 비어있을 경우
            head = newNode;
        }else{ // 큐가 비어있지 않다면 마지막 노드(tail)가 새로운 데이터(newNode)를 가리키도록 한다.
            tail.next = newNode;
        }
        // tail을 새로운 데이터(newNode)로 바꿔준다.
        tail = newNode;
        size++; // 새로운 데이터가 입력되었으니 큐의 크기는 증가한다.
        return true;
    }
```
1. 큐 tail(rear)에 새로운 데이터(newNode)를 삽입한다.  
2. 기존 tail(rear)의 래퍼런스를 새로운 데이터(newNode)를 가리키도록 바꾼다.  
3. tail(rear)를 새로운 데이터(newNode)로 바꾼다.  
4. 성공적으로 제거되었다면 true를 반환한다.  
  
## 3. poll, remove 구현
```java
    @Override
    public E poll() {
        // 삭제할 요소가 없다면 null을 던진다.
        if(size == 0){
            return null;
        }
        E element = head.data; // 삭제할 데이터를 반환하기 위해 임시 제네릭 변수에 저장한다.
        Node<E> nextNode = head.next; // 데이터가 삭제된다면 삭제되는 데이터의 다음 노드를 head(front)로 지정하기 위해 가져온다.

        // 기존 head(front)의 데이터와 래퍼런스를 삭제한다.
        head.data = null;
        head.next = null;

        // head(front)를 삭제된 데이터의 다음 노드로 변경한다.
        head = nextNode;
        size--; // poll을 통하여 요소가 하나 제거되었으니 큐의 크기는 감소한다.
        return element;
    }

    // remove는 위 주석과 같이 삭제할 요소가 없으면 예외를 던지기에 poll()메소드에서 null이 반환되는 것을 이용한다.
    public E remove(){
        E element = poll();
        if(element == null){
            throw new NoSuchElementException();
        }
        return element;
    }
```
1. 큐 head(front)의 데이터(data)와 래퍼런스(next)를 제거한다.(null로 변경)  
2. head(front)를 기존 head(front)의 다음 노드로 변경한다.  
3. 성공적으로 삭제되었다면 삭제된 데이터(data)를 반환한다.  
자바에서 제공하는 Queue API를 보면 remove()와 offer()를 둘 다 제공한다. 두 메소드의 차이는 **"삭제할 요소가 없을 때 던지는 반환값"**의 차이이다. **remove()의 경우 삭제할 요소가 없으면 NoSuchElementException을 던진다. poll()의 경우 삭제할 요소가 없으면 null을 던진다.**  

## 4. peek, element 구현
```java
    @Override
    public E peek() {
        if(size == 0){ // 큐에 데이터가 없다면
            return null;
        }
        return head.data; // 현 head(front)의 데이터(data)를 반환한다.
    }

    // peek는 위 주석과 같이 데이터가 없다면 peek()로부터 null이 반환되는 것을 이용한다.
    public E element(){
        E element = peek();
        if(element == null){
            throw new NoSuchElementException();
        }
        return element;
    }
```
1. peek를 호출했는데 큐에 데이터가 없다면 null을 던진다.  
2. 현 큐의 head(front)의 데이터(data)를 반환한다.  
**peek는 List의 element()와 대응된다. 따라서 peek는 값이 존재하지 않다면 null을 던지며 element()는 값이 없다면 예외를 던진다.**  

## 5. size 구현
```java
    public int size(){
        return size;
    }
```
해당 메소드는 큐에 있는 요소의 개수를 반환하기에 큐에 있는 노드의 개수를 말하는 **int size**를 반환한다.  

## 6. isEmpty 구현
```java
    public boolean isEmpty(){
        return size == 0;
    }
```
해당 메소드는 큐가 비어있는지, 비어있지 않는지에 대한 판별이기에 **size가 0이라면 큐가 비어있다는 의미이므로 `size == 0`을 작성한다.**  

## 7. contains 구현
```java
    public  boolean contains(Object value){
        for(Node<E> x = head; x != null; x = x.next){
            if(value.equals(x.data)){ // 찾고자 하는 데이터가 x의 데이터와 같을 시
                return true;
            }
        }
        return false; // 찾고자 하는 데이터가 큐에 존재하지 않는다.
    }
```
해당 메소드는 찾고자 하는 데이터가 큐에 존재하는지 판별하는 역할을 수행한다. 찾고자 하는 데이터가 존재한다면 true를, 존재하지 않는다면 false를 반환한다.  
`for(Node\<E\> x = head; x != null; x = x.next)`의 경우 큐의 가장 앞 노드 head(front)부터 큐의 끝까지 탐색하는 용도이다. head를 새로운 노드 객체 x에 저장하여 해당 x가 null이 아닐 동안(큐의 끝까지) 노드 객체 x에는 x의 래퍼런스를 대입한다는 의미이다. 즉, **큐의 head(front)부터 노드 x가 null이 될 때 까지 찾고자 하는 데이터(value)와 가져온 노드의 데이터(x.data)와 같은지 판별한다.**  

## 8. clear 구현
```java
    public void clear(){
        for(Node<E> x = head; x != null;){
            Node<E> next = x.next; // x가 가리키는 레퍼런스를 next 노드에 대입한다.
            x.data = null; // x의 데이터 삭제
            x.next = null; // x의 레퍼런스 삭제
            x = next; // 다음 노드로 변경
        }
        size = 0; // 큐의 모든 요소가 삭제되었으니 큐의 크기는 0
        // 큐의 모든 요소가 삭제되었으니 head(front)와 tail(rear)또한 존재하지 않기에 모두 null 처리
        head = null;
        tail = null;
    }
```
해당 메소드는 큐를 초기화(비우는)시키는 역할을 수행한다. **위 contains와 같이 큐의 처음부터 null이 아닐 때 까지(큐 전체) 모든 노드를 가져와 노드의 데이터(data)와 레퍼런스(next)를 null로 초기화시켜준다.** 굳이 노드 하나하나 null 처리하는 이유는 가비지 콜렉터(GC)의 효율을 위하여 명시한다.  

## 전체 코드
```java
import java.util.NoSuchElementException;

public class QueueLinkedList<E> implements Queue<E> {
    private Node<E> head; // 큐에서 가장 앞에 있는 노드 객체(front)를 가리킨다.
    private Node<E> tail; // 큐에서 가장 뒤에 있는 노드 객체(rear)를 가리킨다.
    private int size; // 큐에 있는 노드의 개수

    QueueLinkedList(){
        this.head = null;
        this.tail = null;
        this.size = 0;
    }
    /*
    offer 구현
    1. 큐 tail(rear)에 새로운 데이터(newNode)를 삽입한다.
    2. 기존 tail(rear)의 래퍼런스를 새로운 데이터(newNode)를 가리키도록 바꾼다.
    3. tail(rear)를 새로운 데이터(newNode)로 바꾼다.
    4. 성공적으로 제거되었다면 true를 반환한다.
     */
    @Override
    public boolean offer(E value) {
        Node<E> newNode = new Node<E>(value);
        // 만약 큐가 비어있을 경우
        if(size == 0){
            head = newNode;
        }else{ // 큐가 비어있지 않다면 마지막 노드(tail)가 새로운 데이터(newNode)를 가리키도록 한다.
            tail.next = newNode;
        }
        // tail을 새로운 데이터(newNode)로 바꿔준다.
        tail = newNode;
        size++; // 새로운 데이터가 입력되었으니 큐의 크기는 증가한다.
        return true;
    }

    /*
    poll 구현
    1. 큐 head(front)의 데이터(data)와 래퍼런스(next)를 제거한다.(null로 변경)
    2. head(front)를 기존 head(front)의 다음 노드로 변경한다.
    3. 성공적으로 삭제되었다면 삭제된 데이터(data)를 반환한다.
    자바에서 제공하는 Queue API를 보면 remove()와 offer()를 둘 다 제공한다.
    두 메소드의 차이는 "삭제할 요소가 없을 때 던지는 반환값"의 차이이다.
    remove()의 경우 삭제할 요소가 없으면 NoSuchElementException을 던진다.
    poll()의 경우 삭제할 요소가 없으면 null을 던진다.
     */
    @Override
    public E poll() {
        // 삭제할 요소가 없다면 null을 던진다.
        if(size == 0){
            return null;
        }
        E element = head.data; // 삭제할 데이터를 반환하기 위해 임시 제네릭 변수에 저장한다.
        Node<E> nextNode = head.next; // 데이터가 삭제된다면 삭제되는 데이터의 다음 노드를 head(front)로 지정하기 위해 가져온다.

        // 기존 head(front)의 데이터와 래퍼런스를 삭제한다.
        head.data = null;
        head.next = null;

        // head(front)를 삭제된 데이터의 다음 노드로 변경한다.
        head = nextNode;
        size--; // poll을 통하여 요소가 하나 제거되었으니 큐의 크기는 감소한다.
        return element;
    }

    // remove는 위 주석과 같이 삭제할 요소가 없으면 예외를 던지기에 poll()메소드에서 null이 반환되는 것을 이용한다.
    public E remove(){
        E element = poll();
        if(element == null){
            throw new NoSuchElementException();
        }
        return element;
    }

    /*
    peek 구현
    1. peek를 호출했는데 큐에 데이터가 없다면 null을 던진다.
    2. 현 큐의 head(front)의 데이터(data)를 반환한다.
    peek는 List의 element()와 대응된다. 따라서 peek는 값이 존재하지 않다면 null을 던지며 element()는 값이 없다면 예외를 던진다.
     */
    @Override
    public E peek() {
        if(size == 0){ // 큐에 데이터가 없다면
            return null;
        }
        return head.data; // 현 head(front)의 데이터(data)를 반환한다.
    }

    // peek는 위 주석과 같이 데이터가 없다면 peek()로부터 null이 반환되는 것을 이용한다.
    public E element(){
        E element = peek();
        if(element == null){
            throw new NoSuchElementException();
        }
        return element;
    }

    // 현재 큐에 있는 데이터의 개수를 반환한다.
    public int size(){
        return size;
    }

    // 현재 큐가 비어있는지 판별한다.
    // 큐가 비어있다면 true를, 비어있지 않다면 false를 반환한다.
    public boolean isEmpty(){
       return size == 0;
    }

    // 현재 찾고자 하는 데이터가 큐에 존재하는지 찾는다.
    // 만약, 찾고자 하는 데이터가 큐에 존재하면 true를, 존재하지 않는다면 false를 반환한다.
    public  boolean contains(Object value){
        // 큐의 head(front)부터 노드 x가 null이 될 때 까지 찾고자 하는 데이터(data)와 x의 데이터(data)가 같은지 판별한다.
        for(Node<E> x = head; x != null; x = x.next){
            if(value.equals(x.data)){ // 찾고자 하는 데이터가 x의 데이터와 같을 시
                return true;
            }
        }
        return false; // 찾고자 하는 데이터가 큐에 존재하지 않는다.
    }

    // 큐의 모든 데이터를 제거한다.
    // 가비지 콜렉터(GC)의 효율을 위하여 모든 데이터를 명시적으로 null 처리한다.
    public void clear(){
        for(Node<E> x = head; x != null;){
            Node<E> next = x.next; // x가 가리키는 레퍼런스를 next 노드에 대입한다.
            x.data = null; // x의 데이터 삭제
            x.next = null; // x의 레퍼런스 삭제
            x = next; // 다음 노드로 변경
        }
        size = 0; // 큐의 모든 요소가 삭제되었으니 큐의 크기는 0
        // 큐의 모든 요소가 삭제되었으니 head(front)와 tail(rear)또한 존재하지 않기에 모두 null 처리
        head = null;
        tail = null;
    }
}
```
  


---  
[참고 및 리소스 출처]  
[Stranger's LAB](https://st-lab.tistory.com/184?category=856997)  
[코딩팩토리](https://coding-factory.tistory.com/602)  