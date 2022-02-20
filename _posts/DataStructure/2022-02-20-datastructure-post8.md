---
layout: post
title: "[Java, 자료구조] Singly LinkedList(단일 연결리스트) 구현하기 (2/2) - Singly LinkedList 구현"
data:   2022-02-20 19:54:40+0900
categories: jekyll update
tags: [java, data structure]
---
**해당 게시글은 [[Java, 자료구조] Singly LinkedList(단일 연결리스트) 구현하기 (1/2) - 인터페이스](https://daegom.com/main/datastructure-post7/)와 이어진다.**  
해당 포스팅은 자료구조 중 하나인 **List**에 대해 이해하기 위해 스터디 목적으로 직접 List를 구현해보는 포스팅이다. Java에서 지원하는 List 클래스에 대한 사용법이 아니다. 실습을 진행할 때 사용된 코드는 모두 [Stranger's Lab](https://st-lab.tistory.com/167)님의 게시글과 설명을 참고로 하여 구현하였다.

# LinkedList
LinkedList는 ArrayList와 달리 `노드` 라는 객체를 이용하여 연결한다. 배열을 이용하는 것이 아닌, **하나의 객체에 데이터와 다른 노드를 가리키는 주소(레퍼런스)로 구성되어 여러 노드를 연결한다.**  
`SinglyLinkedList`는 뜻과 같이 **단일 연결 리스트**라는 의미이며 단방향으로 연결된 리스트를 말한다. 연결된 노드들에서 **삽입**을 수행하고자 하면 노드의 래퍼런스(링크)만 바꾸면 되며 **삭제**를 한다면 삭제할 노드의 이전 노드에서 래퍼선스(링크)를 끊고 다음 노드를 연결한다. 이같은 특성이 List의 특징 중 하나인 **데이터(요소, element) 사이에 빈 공간을 허용하지 않는다.**를 말한다. 또한 `LinkedList`에 있는 노드들 중 첫 번째 노드를 **`Head`**라 하며 제일 마지막 노드를 **`Tail`**이라 한다. *(스택의 TOP, BOTTOM 과 같은 의미.*

# 노드(Node) 구현
노드는 데이터(data)와 다음 노드에 대한 주소(레퍼런스)로 구성되어 있다. 즉, 2개의 변수로 이루어져 있다.

```java
class Node<E> { // 어떠한 타입이 들어올 지 모르기에 제네릭을 사용한다.
    E data; // 노드의 '데이터'를 의미
    Node<E> next; // 다음 노드 객체를 가리킨다. (주소, 래퍼런스)
    Node(E data){
        this.data = data;
        this.next = null;
    }
}
```

# SinglyLinkedList 클래스 구현하기
**SinglyLinkedList 클래스를 생성한 뒤 List를 implements 시킨다.**  
*또한, SinglyLinkedList \<E\>를 사용하는 제네릭 인터페이스 이기에 SinglyLinkedList 클래스도 제네릭 클래스로 명시한다.*

## 구현할 메소드
- private Node\<E\> search(int index)  
- public void addFirst(E value)  
- public boolean add(E value) `@Override`  
- public void addLast(E value)  
- public void add(int index, E value) `@Override`  
- public E remove()  
- public E remove(int index) `@Override`  
- public boolean remove(Object value) `@Override`  
- public E get(int index) `@Override`  
- public void set(int index, E value) `@Override`  
- public int indexOf(Object value) `@Override`  
- public boolean contains(Object item) `@Override`  
- public int size() `@Override`  
- public boolean isEmpty() `@Override`  
- public void clear() `@Override`  

## 1. 생성자와 변수 선언
```java
public class SinglyLinkedList<E> implements List<E> {
    private Node<E> head;
    private Node<E> tail;
    private int size; // 리스트에 있는 요소의 개수

    public SinglyLinkedList() {
        this.head = null;
        this.tail = null;
        this.size = 0;
    }
```
[변수 설명]  
head : Node 클래스의 객체이며 리스트의 첫 번째 노드를 말한다.  
tail : Node 클래스의 객체이며 리스트의 마지막 노드를 말한다.  
size : 리스트에 있는 노드의 개수를 말한다.  
  
생성자의 경우 처음으로 `SinglyLinkedList`가 생성된 시점은 리스트에 아무런 노드도 존재하지 않는다. 따라서 head와 tail을 null로 초기화 시키며 리스트에 있는 요소의 개수 또한 아무것도 없기에 0으로 초기화 시킨다. 생성자는 다음과 같이 사용할 수 있다.  
```java
SinglyLinkedList<Integer> list = new SinglyLinkedList<>();
```

## 2. search 구현
```java
    // 양방향 연결리스트가 아닌, 단일 연결리스트의 경우 특정 위치의 데이터를 추가/삭제/색인하기 위해서는 head부터 next변수를 통해 특정 위치까지 찾아가야 한다.
    // 이를 위해 search() 메소드를 구현한다.
    // 특정 위치의 노드를 반환한다.
    private Node<E> search(int index) {
        // 만약, 파라미터의 값이 리스트에 있는 요소의 개수의 범위가 아닐 경우 예외를 뿌린다.
        // 이때 index >= size 에서 index > size 가 아닌 이유는 연결된 노드들 중 tail의 레퍼런스는 null이다. 다음 노드로 연결을 해야하는데
        // 연결한 노드가 당연히 존재하지 않기에 null이다.
        if (index < 0 || index >= size) {
            throw new IndexOutOfBoundsException();
        }
        Node<E> x = head;
        for (int i = 0; i < index; i++) {
            x = x.next;
        }
        return x;
    }
```
양방향 연결리스트가 아닌, 단일연결리스트는 원하는 위치의 노드를 관리하기 위해서 리스트의 첫 번째 노드(Head)부터 특정 위치까지 찾아야 한다. **`search(int index)`**는 원하는 위치의 노드를 반환해주는 역할을 수행한다.  

## 3. add 구현
**add의 메소드는 다음과 같이 총 4가지이다.**  
- public void addFirst(E value)  
- public boolean add(E value) `@Override`  
- public void addLast(E value)  
- public void add(int index, E value) `@Override`  
Head에 노드 추가하기, Tail에 노드 추가하기, 특정 위치에 데이터 추가하기  
**자바의 LinkedList API에서 add()의 역할을 addLast()가 수행한다. 따라서 특정 위치에 데이터를 추가하기 위해서는 add(int index, E value)가 사용되며 리스트 가장 앞(Head)에 원하는 요소를 추가하려면 addFirst()를 수행한다.**  
```java
    // 리스트 가장 앞 부분(Head)에 노드를 추가한다.
    public void addFirst(E value){
        Node<E> newNode = new Node<E>(value); // newNode라는 새로운 노드를 생성한다.
        newNode.next = head; // 새 노드의 다음 노드로 head(가장 앞)노드를 연결
        head = newNode; // head가 가리키는 노드를 새 노드로 변경 (리스트 가장 앞에 원하는 요소 넣기)
        size++; // 새로운 노드가 추가되었으니 노드의 개수를 뜻하는 size 변수 증가

        // 다음에 가리킬 노드가 없는 경우(= 데이터가 새 노드밖에 없는 경우)
        // 데이터가 1개(새 노드)밖에 없으므로 새 노드는 처음 시작 노드이자 마지막 노드이다. (Head = Tail)
        if(head.next == null){
            tail = head;
        }
    }

    @Override
    public boolean add(E value) {
        addLast(value);
        return true;
    }

    // 리스트의 가장 마지막 부분에 추가한다. (Tail) - 기본값
    // Java API에서의 LinkedList API를 확인하면 add() 메소드를 호출할 시 add() 메소드는 addLast() 메소드를 호출한다.
    public void addLast(E value){
        Node<E> newNode = new Node<E>(value); // newNode라는 새로운 노드를 생성한다.
        if(size == 0){ // size(노드의 개수)가 0이라는 것은 처음으로 넣는 노드라는 의미이다. 따라서 Last가 아닌 addFirst로 값을 넘긴다.
            addFirst(value);
            return;
        }
        // 마지막 노드(Tail)의 다음 노드(Next)가 새 노드를 가리키도록 하고 Tail이 가리키는 노드를 새 노드로 바꿔준다.
        tail.next = newNode; // 노드는 기본적으로 데이터와 다음 노드를 가리키는 레퍼런스로 이루져 있지만, 마지막 노드(Tail)의 레퍼런스는 null이다.
        tail = newNode;
        size++;
    }

    // 특정 위치에 요소를 추가한다.
    @Override
    public void add(int index, E value) {
        // 특정 위치가 잘못되었을 경우 IndexOutOfBoundsException을 던진다.
        if(index > size || index < 0){
            throw new IndexOutOfBoundsException();
        }

        // 만약 특정 위치에 노드를 추가하려고 할 때 첫 번째 위치일 경우 addFirst에 값을 넘긴다.
        if(index == 0) {
            addFirst(value);
            return;
        }

        // 만약 특정 위치에 노드를 추가하려고 할 때 마지막 위치 경우 addLast 값을 넘긴다.
        if(index == size){
            addLast(value);
            return;
        }

        // 특정 위치에 노드가 추가된다면, 추가된 노드 앞과 뒤 노드의 래퍼런스를 수정할 필요가 있다.
        // 특정 위치의 뒤쪽 노드 가져오기
        Node<E> prev_Node = search(index - 1);

        // 특정 위치에 요소를 추가하기 전, 이미 그 위치에 있는 노드 가져오기
        Node<E> next_Node = prev_Node.next;

        // 특정 위치에 추가하려는 노드
        Node<E> newNode = new Node<E>(value);

        // 이전 노드가 가리키는 노드를 끊은 뒤 새 노드로 연결한다.
        // 또한 새 노드가 가리키는 노드는 next_Node로 설정한다.
        prev_Node.next = null; // 이전 노드가 가리키는 노드 (그 위치에 있는 노드)의 링크를 끊는다.
        prev_Node.next = newNode; // 그 뒤 그 위치에 있는 노드를 가져온다.
        newNode.next = next_Node; // 마지막으로 새 노드가 가리키는 노드를 해당 위치에 있던 노드로 연결한다.
        size++;
    }
```
**`tail.next = newNode;`의 경우 리스트에서 마지막 노드(tail)이 가리키는 레퍼런스는 `null`이다. 따라서 addLast()를 수행하였을 때 기존 tail 노드의 레퍼런스 값을 새로운 노드 객체로 연결해야한다.**  

## 4. remove 구현
**add의 메소드는 다음과 같이 총 3가지이다.**  
- public E remove()  
- public E remove(int index) `@Override`  
- public boolean remove(Object value) `@Override`  
Head 삭제하기, 특정 위치의 요소 삭제하기, 특정 요소(데이터)의 요소 삭제하기  
```java
    // 가장 앞에 요소(Head)를 삭제한다. 즉, 첫 번째 요소(노드)를 삭제하는 것이며 index로 생각하면 0을 말한다.
    public E remove(){
        Node<E> headNode = head; // 가장 앞에 있는 노드(요소, Head)를 가져온다.

        // 리스트에 아무런 요소가 없는데 삭제를 시도하려는 경우 NoSuchElementException을 던진다.
        if(headNode == null){
            throw new NoSuchElementException();
        }

        E element = headNode.data; // 삭제된 노드를 반환하기 위하여 Head의 데이터를 임시 변수에 저장한다.
        Node<E> nextNode = head.next; // head의 레퍼런스를 다음 노드로 지정한다.

        head.next = null; // 기존 head의 다음 레퍼런스를 삭제한다.
        head.data = null; // 기존 head의 데이터를 삭제한다.

        head = nextNode; // 기존 head가 새로운 노드를 가리키도록 바꾼다.
        size--; // 기존 head가 삭제되었으니 리스트의 크기는 감소된다.

        // 삭제된 요소가 리스트의 유일한 요소였을 경우 그 요소는 head이자 tail이기 때문에 삭제되면서 tail도 가리킬 요소가 없다.
        // 따라서 size가 0일 경우 tail도 null로 바꾼다.
        if(size == 0){
            tail = null;
        }
        return element; // 기존 head 노드의 데이터를 반환한다.
    }

    // 특정 위치의 요소를 삭제한다.
    // 삭제하려는 노드의 이전 노드(레퍼런스, Node.next)는 삭제하려는 노드의 다음 노드를 가리키도록 한다.
    // 1, 2, 3이 있는 경우 2를 삭제하고자 할 시, 1과 3을 하나로 이어야 한다.
    @Override
    public E remove(int index) {
        if(index == 0){ // 삭제하려는 위치가 첫 번째 노드일 경우 Head를 삭제하는 remove()로 던진다.
            return remove();
        }

        // 특정 위치가 잘못되었을 경우 IndexOutOfBoundsException을 던진다.
        if(index >= size || index < 0){
            throw new IndexOutOfBoundsException();
        }

        Node<E> prevNode = search(index - 1); // 삭제할 노드의 이전 노드를 가져온다. (index-1)
        Node<E> removedNode = prevNode.next; // 삭제할 노드를 뜻한다. (prevNode.next는 이전 노드가 가리키는 레퍼런스를 가져온다.)
        Node<E> nextNode = removedNode.next; // 삭제할 노드의 다음 노드를 가져온다. (removedNode.next는 삭제될 노드가 가리키는 레퍼런스를 뜻한다.)
        E element = removedNode.data; // 삭제되는 노드의 데이터를 반환하기 위한 임시 변수

        // 삭제할 노드의 이전 노드가 가리키는 레퍼런스를 삭제할 노드의 다음 노드가 가리키는 레퍼런스로 연결한다.
        prevNode.next = nextNode.next;

        // 만약 삭제했던 노드가 마지막 노드라면 tail를 prevNode(삭제하려는 노드의 이전 노드)로 변경한다.
        if(removedNode.next == null){
            tail = prevNode;
        }
        removedNode.next = null; // 삭제하고자 한 노드의 레퍼런스를 제거한다.
        removedNode.data = null; // 삭제하고자 한 노드의 데이터를 제거한다.
        size--; // 노드가 제거되었기에 노드의 개수는 1씩 감소한다.

        return element; // 삭제된 노드에 있는 데이터를 반환한다.
    }

    // 특정 데이터를 리스트에서 찾아 삭제한다.
    // 삭제하려는 요소가 존재하지 않는 경우 false를 반환하고 존재할 경우 remove(int index)와 동일한 삭제 방식을 사용한다.
    @Override
    public boolean remove(Object value) {
        Node<E> prevNode = head; // 이전 노드(prevNode)에 Head 노드를 대입한다.
        boolean hasValue = false;
        Node<E> x = head; // removedNode를 말하며 즉 삭제할 노드를 말한다.

        // value와 일치하는 노드를 찾는다.
        for(; x!=null; x = x.next){ // x가 null이 아닐 때 까지 Node x는 x의 레퍼런스를 뜻한다.
            if(value.equals(x.data)){ // 특정 데이터가 x의 데이터와 같다면
                hasValue = true;
                break;
            }
            prevNode = x;
        }

        // 일치하는 데이터가 리스트에 없다면 false를 반환한다.
        if(x == null){
            return false;
        }

        // 삭제하려는 노드가 head라면 Head를 제거하는 메소드 remove로 던진다.
        if(x.equals(head)){
            remove();
            return true;
        }else{
            // 이전 노드의 래퍼런스를, 삭제하려는 노드의 다음 노드로 연결한다.
            prevNode.next = x.next; // 이전 노드의 레퍼런스를 삭제하려는 노드의 다음 노드로 연결한다.

            // 만약, 삭제했던 노드가 마지막 노드라면 tail을 이전 노드로 바꾼다.
            if(x.next == null){
                tail = prevNode;
            }

            // 데이터 삭제
            x.next = null;
            x.data = null;
            size--;
            return true;
        }
    }
```
  
## 5. get 구현
```java
    @Override
    public E get(int index) {
        return search(index).data;
    }
```
get은 특정 위치에 있는 데이터를 가져온다. search()는 '노드' 자체를 반환하기에 data()를 사용하여 데이터를 가져온다.  

## 6. set 구현
```java
    @Override
    public void set(int index, E value) {
        Node<E> replaceNode = search(index);
        replaceNode.data = null; // 찾은 노드에 있던 데이터를 삭제한 뒤
        replaceNode.data = value; // 새로운 데이터로 바꾼다.
    }
```
set은 특정 위치에 있는 데이터를 새로운(원하는) 데이터로 바꾼다. search를 이용하여 '노드'자체를 가져와 해당 노드의 데이터를 바꾼다.  

## 7. indexOf 구현
```java
    @Override
    public int indexOf(Object value) {
        int index = 0;
        for(Node<E> x = head; x != null ; x = x.next){ // x에 첫 번째 노드를 가져온 뒤 x가 null이 될 때 까지 반복문을 수행한다.
            if(value.equals(x.data)){ // 찾고자 하는 데이터(value)가 기존에 있던 노드의 데이터(x.data)와 같을 시
                return index; // 해당 위치를 반환한다.
            }
            index++;
        }
        // 찾고자 하는 요소가 없다면
        return -1;
    }
```
indexOf는 찾고자 하는 데이터의 위치(index)를 반환한다. 또한 찾고자 하는 데이터가 중복이라면 **가장 먼저 마주치는 데이터의 위치(index)를 반환한다.**  
만약, 찾고자 하는 데이터가 없다면 **-1를 반환시킨다.**
  
## 8. contains 구현
```java
@Override
    public boolean contains(Object value) {
        return indexOf(value) >= 0; // -1이 아니라면 원하는 데이터가 존재한다는 의미.
    }
```
contains는 원하는 데이터가 리스트에 존재하는지 판별하는 메소드이다. 생각해보면 **indexOf의 경우 원하는 데이터의 위치를 반환한다. 따라서 -1이 반환된다면 찾고자 하는 데이터가 리스트에 존재하지 않는다는 의미이며 -1이 아니라면 원하는 데이터가 리스트에 존재한다는 의미이다.**  

## 9. size 구현
```java
@Override
    public int size() {
        return size;
    }
```
size는 단순하게 **리스트 안에 있는 노드의 개수**를 반환한다. 따라서 `SinglyLinkedList` 클래스의 `size` 변수를 반환시킨다.  

## 10. isEmpty 구현
```java
    // 현재 리스트에 요소가 단 하나도 존재하지 않는지 판별한다.
    // 리스트가 비어있다면 true를, 비어있지 않다면 false를 반환한다.
    @Override
    public boolean isEmpty() {
        return size == 0;
    }
```
isEmpty는 리스트에 노드가 존재하는지에 대한 판별 메소드이다. **`size` 변수는 리스트에 존재하는 노드의 개수를 가리킨다.** 리스트가 비어있다면 `true`를, 비어있지 않다면 `false`를 반환한다.  

## 11. clear 구현
```java
    @Override
    public void clear() {
        for(Node<E> x = head; x != null;){ // 아무런 노드가 존재하지 않을 때 가지 head를 가져오는 반복문을 수행한다.
            Node<E> nextNode = x.next; // 가져온 노드가 가리키는 노드를 대입한다.
            x.data = null; // 가져온 노드의 데이터를 제거한다.
            x.next = null; // 가져온 노드의 레퍼런스를 제거한다.
            x = nextNode; // 다시 x에 가져온 노드를 대입한다.
        }
        head = null; // 리스트에는 아무런 노드가 존재하지 않기에 head와 tail를 제거한다.
        tail = null;
        size = 0; // 리스트에는 아무런 노드가 존재하지 않기에 기존에 있던 변수를 0으로 초기화한다.
    }
}
```
clear는 리스트에 있는 모든 노드를 제거하는 메소드이다. **첫 번째 노드(Head)를 이용하여 마지막 노드(Tail)까지 노드의 객체 자체를 null 처리하기 보단 노드에 있는 데이터와 레퍼런스 자체를 하나하나 null 처리하는 것이 가비지 콜렉터(GC)가 명시적으로 해당 메모리를 사용하지 않는다고 인식하여 메모리 관리가 조금 더 효율적이다.**

## 전체 코드
```java
public class SinglyLinkedList<E> implements List<E> {
    private Node<E> head;
    private Node<E> tail;
    private int size;

    public SinglyLinkedList() {
        this.head = null;
        this.tail = null;
        this.size = 0;
    }

    private Node<E> search(int index) {
        if (index < 0 || index >= size) {
            throw new IndexOutOfBoundsException();
        }
        Node<E> x = head;
        for (int i = 0; i < index; i++) {
            x = x.next;
        }
        return x;
    }

    public void addFirst(E value){
        Node<E> newNode = new Node<E>(value);
        newNode.next = head; 
        head = newNode; 
        size++;
        if(head.next == null){
            tail = head;
        }
    }

    @Override
    public boolean add(E value) {
        addLast(value);
        return true;
    }

    public void addLast(E value){
        Node<E> newNode = new Node<E>(value); 
        if(size == 0){
            addFirst(value);
            return;
        }

        tail.next = newNode;
        tail = newNode;
        size++;
    }

    @Override
    public void add(int index, E value) {
        if(index > size || index < 0){
            throw new IndexOutOfBoundsException();
        }

        if(index == 0) {
            addFirst(value);
            return;
        }
        if(index == size){
            addLast(value);
            return;
        }
        Node<E> prev_Node = search(index - 1);
        Node<E> next_Node = prev_Node.next;
        Node<E> newNode = new Node<E>(value);

        prev_Node.next = null;
        prev_Node.next = newNode;
        newNode.next = next_Node;
        size++;
    }

    public E remove(){
        Node<E> headNode = head;

        if(headNode == null){
            throw new NoSuchElementException();
        }

        E element = headNode.data;
        Node<E> nextNode = head.next;
        head.next = null;
        head.data = null;

        head = nextNode;
        size--;

        if(size == 0){
            tail = null;
        }
        return element;
    }

    @Override
    public E remove(int index) {
        if(index == 0){
            return remove();
        }
        if(index >= size || index < 0){
            throw new IndexOutOfBoundsException();
        }

        Node<E> prevNode = search(index - 1); 
        Node<E> removedNode = prevNode.next;
        Node<E> nextNode = removedNode.next;
        E element = removedNode.data;

        prevNode.next = nextNode.next;

        if(removedNode.next == null){
            tail = prevNode;
        }

        removedNode.next = null;
        removedNode.data = null;
        size--;

        return element;
    }

    @Override
    public boolean remove(Object value) {
        Node<E> prevNode = head;
        boolean hasValue = false;
        Node<E> x = head;
        for(; x!=null; x = x.next){
            if(value.equals(x.data)){
                hasValue = true;
                break;
            }
            prevNode = x;
        }
        if(x == null){
            return false;
        }
        if(x.equals(head)){
            remove();
            return true;
        }else{
            prevNode.next = x.next;

            if(x.next == null){
                tail = prevNode;
            }

            x.next = null;
            x.data = null;
            size--;
            return true;
        }
    }

    @Override
    public E get(int index) {
        return search(index).data;
    }

    @Override
    public void set(int index, E value) {
        Node<E> replaceNode = search(index);
        replaceNode.data = null;
        replaceNode.data = value;
    }

    @Override
    public int indexOf(Object value) {
        int index = 0;
        for(Node<E> x = head; x != null ; x = x.next){
            if(value.equals(x.data)){
                return index;
            }
            index++;
        }
        return -1;
    }

    @Override
    public boolean contains(Object value) {
        return indexOf(value) >= 0;
    }

    @Override
    public int size() {
        return size;
    }

    @Override
    public boolean isEmpty() {
        return size == 0;
    }

    @Override
    public void clear() {
        for(Node<E> x = head; x != null;){
            Node<E> nextNode = x.next;
            x.data = null;
            x.next = null;
            x = nextNode;
        }
        head = null;
        tail = null;
        size = 0;
    }
}
```
  
  

---
[참고 및 리소스 출처]  
[Stranger's LAB](https://st-lab.tistory.com/167)  