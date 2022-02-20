---
layout: post
title: "[Java, 자료구조] Singly LinkedList(단일 연결리스트) 구현하기 (2/2) - Singly LinkedList 구현"
data:   2022-02-20 19:54:40+0900
categories: jekyll update
tags: [java, data structure]
---
# 추가중

```java
public class SinglyLinkedList<E> implements List<E> {
    private Node<E> head; // 리스트에 있는 요소 중 첫 번째 요소
    private Node<E> tail; // 리스트에 있는 요소 중 마지막 요소
    private int size; // 리스트에 있는 요소의 개수(크기가 아니다.)

    public SinglyLinkedList() {
        this.head = null;
        this.tail = null;
        this.size = 0;
        // 처음 생성자를 실행시켜 단일연결리스트를 구현하였을 때 아무런 요소가 존재하지 않기에 모든 값을 초기화 시킨다.
        // SinglyLinkedList<Integer> list = new SinglyLinkedList<>();
    }

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
        Node<E> x = head; // 리스트에 있는 첫 번째 노드(head)부터 시작한다.
        for (int i = 0; i < index; i++) {
            x = x.next; // 노드 x의 다음 노드를 x에 저장한다.
        }
        return x;
    }

    /*
    자바에 내장되어 있는 LinkedList에서는 add()의 역할을 addLast()메소드가 수행하며
    특정 위치에 추가는 add(int index, E element) 메솓, 가장 첫 부분에 추가는 addFirst()가 한다.
     */

    // 리스트 가장 앞부분(Head)에 요소를 추가한다.
    public void addFirst(E value){
        Node<E> newNode = new Node<E>(value); // 노드 생성
        newNode.next = head; // 새 노드의 다음 노드로 head 노드를 연결
        head = newNode; // head가 가리키는 노드를 새 노드로 변경
        size++;

        // 다음에 가리킬 노드가 없는 경우(= 데이터가 새 노드밖에 없는 경우)
        // 데이터가 1개(새 노드)밖에 없으므로 새 노드는 처음 시작노드이자 마지막 노드이다. (Head = Tail)
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
        Node<E> newNode = new Node<E>(value); // 새 노드 생성
        // 처음 넣는 노드일 경우 addFirst로 추가
        if(size == 0){
            addFirst(value);
            return;
        }

        // 마지막 노드(Tail)의 다음 노드(Next)가 새 노드를 가리키도록 하고 Tail이 가리키는 노드를 새 노드로 바꿔준다.
        tail.next = newNode;
        tail = newNode;
        size++;
    }

    // 특정 위치에 요소를 추가한다.
    // 넣으려는 위치의 앞 뒤로 링크를 업데이트 해야한다.
    @Override
    public void add(int index, E value) {
        // 잘못된 인덱스를 참조할 경우
        if(index > size || index < 0){
            throw new IndexOutOfBoundsException();
        }

        // 추가하려는 index가 가장 앞에 추가하려는 경우 addFirst 호출
        if(index == 0) {
            addFirst(value);
            return;
        }

        // 추가하려는 index가 마지막 위치일 경우 addLast 호출
        if(index == size){
            addLast(value);
            return;
        }

        // 추가하려는 위치의 이전 노드 구하기
        Node<E> prev_Node = search(index - 1);

        // 추가하려는 위치의 노드
        Node<E> next_Node = prev_Node.next;

        // 추가하려는 노드
        Node<E> newNode = new Node<E>(value);

        // 이전 노드가 가리키는 노드를 끊은 뒤 새 노드로 연결한다.
        // 또한 새 노드가 가리키는 노드는 next_Node로 설정한다.
        prev_Node.next = null;
        prev_Node.next = newNode;
        newNode.next = next_Node;
        size++;
    }

    // 가장 앞에 요소(Head)를 삭제한다. 즉, 첫 번째 요소를 삭제하는 것이며 index로 생각하면 0을 말한다.
    public E remove(){
        Node<E> headNode = head;

        // 리스트에 아무런 요소가 없는데 삭제를 시도하려는 경우
        if(headNode == null){
            throw new NoSuchElementException();
        }

        // 삭제된 노드를 반환하기 위한 임시 변수 지정
        E element = headNode.data;

        // head의 다음 노드 지정
        Node<E> nextNode = head.next;

        // head 노드의 데이터들을 모두 삭제
        head.next = null;
        head.data = null;

        // head가 다음 노드를 가리키도록 업데이트 한다.
        head = nextNode;
        size--;

        // 삭제된 요소가 리스트의 유일한 요소였을 경우 그 요소는 head이자 tail이기 때문에 삭제되면서 tail도 가리킬 요소가 없다.
        // 따라서 size가 0일 경우 tail도 null로 바꾼다.
        if(size == 0){
            tail = null;
        }
        return element;
    }

    // 특정 index의 요소를 삭제한다.
    // 삭제하려는 노드의 이전 노드의 next 변수를, 삭제하려는 노드의 다음 노드를 가리키도록 한다.
    @Override
    public E remove(int index) {
        // 삭제하려는 노드가 첫 번째 원소일 경우
        if(index == 0){
            return remove();
        }

        // 잘못된 범위에 대한 예외
        if(index >= size || index < 0){
            throw new IndexOutOfBoundsException();
        }

        Node<E> prevNode = search(index - 1); // 삭제할 노드의 이전 노드
        Node<E> removedNode = prevNode.next; // 삭제할 노드
        Node<E> nextNode = removedNode.next; // 삭제할 노드의 다음 노드
        E element = removedNode.data; // 삭제되는 노드의 데이터를 반환하기 위한 임시 변수

        // 이전 노드가 가리키는 노드를, 삭제하려는 노드의 다음 노드로 변경한다.
        prevNode.next = nextNode.next;

        // 만약 삭제했던 노드가 마지막 노드라면 tail를 prevNode(삭제하려는 노드의 이전 노드)로 변경한다.
        if(removedNode.next == null){
            tail = prevNode;
        }

        // 데이터를 삭제한다.
        removedNode.next = null;
        removedNode.data = null;
        size--;

        return element;
    }

    // 특정 요소를 리스트에서 찾아 삭제한다.
    // 삭제하려는 요소가 존재하지 않는 경우 false를 반환하고 존재할 경우 remove(int index)와 동일한 삭제 방식을 사용한다.
    @Override
    public boolean remove(Object value) {
        Node<E> prevNode = head;
        boolean hasValue = false;
        Node<E> x = head; // removedNode를 말하며 즉 삭제할 노드를 말한다.

        // value와 일치하는 노드를 찾는다.
        for(; x!=null; x = x.next){
            if(value.equals(x.data)){
                hasValue = true;
                break;
            }
            prevNode = x;
        }

        // 일치하는 요소가 없다면 false를 반환한다.
        if(x == null){
            return false;
        }

        // 삭제하려는 노드가 head라면 remove()를 사용한다.
        if(x.equals(head)){
            remove();
            return true;
        }else{
            // 이전 노드의 래퍼런스를, 삭제하려는 노드의 다음 노드로 연결한다.
            prevNode.next = x.next;

            // 만약, 삭제했던 노드가 마지막 노드라면 tail을 prevNode로 바꾼다.
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

    // 특정 위치에 있는 요소를 반환한다.
    // search()는 '노드'를 반환하며 get(int index)는 '노드의 데이터'를 반환한다.
    @Override
    public E get(int index) {
        return search(index).data;
    }

    // 특정 위치(index)에 있는 데이터를 새로운 데이터(value)로 교체한다.
    // search()를 이용해 해당 노드를 찾고 해당 노드의 데이터만 변경한다.
    @Override
    public void set(int index, E value) {
        Node<E> replaceNode = search(index);
        replaceNode.data = null;
        replaceNode.data = value;
    }

    // 찾고자 하는 요소(내용, value)의 위치(index)를 반환한다.
    // 찾고자 하는 요소가 중복이 될 때, 가장 먼저 마주치는 요소의 index를 반환한다.
    // 찾고자 하는 요소가 없으면 -1을 반환한다.
    @Override
    public int indexOf(Object value) {
        int index = 0;
        for(Node<E> x = head; x != null ; x = x.next){
            if(value.equals(x.data)){
                return index;
            }
            index++;
        }
        // 찾고자 하는 요소가 없다면
        return -1;
    }

    // 사용자가 찾고자 하는 요소(value, 내용)이 존재하느냐에 대한 판별 메소드
    // 찾고자 하는 요소가 존재하면 true를, 존재하지 않는다면 false를 반환한다.
    // indexOf()로 값을 전달하며 -1이 반환되지 않는다면 요소가 존재한다는 뜻이며 -1이 반환되면 요소가 존재하지 않는다는 의미이다.
    @Override
    public boolean contains(Object value) {
        return indexOf(value) >= 0;
    }

    // 리스트 안에 있는 요소의 개수를 반환한다. (길이가 아니다.)
    @Override
    public int size() {
        return size;
    }

    // 현재 리스트에 요소가 단 하나도 존재하지 않는지 판별한다.
    // 리스트가 비어있다면 true를, 비어있지 않다면 false를 반환한다.
    @Override
    public boolean isEmpty() {
        return size == 0;
    }

    // 리스트에 있는 모든 요소를 지운다.
    // 객체 자체를 null 하기 보단 모든 노드를 하나하나 null 처리를 해주는 것이 GC가 명시적으로 해당 메모리를
    // 사용하지 않는다고 인지하기에 메모리 관리 효율 측면에서 조금이나마 더 좋다.
    @Override
    public void clear() {
        for(Node<E> x = head; x != null;){
            Node<E> nextNode = x.next;
            x.data = null;
            x.next = null;
            x = nextNode;
        }
        head = tail = null;
        size = 0;
    }
}
```


---
[참고 및 리소스 출처]  
[Stranger's LAB](https://st-lab.tistory.com/167)  