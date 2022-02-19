---
layout: post
title: "[Java, 자료구조] Stack 구현하기(1/2) - Stack Interface"
data:   2022-02-19 17:05:00+0900
categories: jekyll update
tags: [java, data structure]
---
# Stack이란?
**스택(Stack)은 자료구조 중 하나인 상자에 물건을 쌓아 올리듯이 데이터를 차곡차곡 쌓는 자료구조이다.**  
***스택(Stack)은 후입선출(Last in First Out)의 특징을 가지고 있으며 말 그래도 마지막에 들어온 데이터가 가장 먼저 나가는 구조이다.***
<p align="center"><img src="/assets/img/blog/정보/스택.png"></p>

# Stack의 특징
1. **후입선출(Last in First Out)**  
2. 시스템 해킹을 진행할 때 버퍼오버플로우 취약점을 이용한 공격을 할 때 Stack 메모리 영역에서 이루어진다.  
3. 인터럽트처리, 수식의 계산, 서브루틴의 복귀 번지 저장등에 쓰인다.  
4. 그래프의 **깊이 우선 탐색(DFS)**에서 사용된다.  
5. 재귀적(Recursion)함수를 호출할 때 사용한다.  
스택과 같은 **후입선출(Last in First Out)**의 구조는 보통 웹 페이지에서 `뒤로가기`, 혹은 `실행취소(Undo)`, 컴퓨터구조에서 `Stack Memory`가 대표적으로 쓰인다. 스택은 후입선출(Last in First Out)의 구조를 가지고 있기에 직전의 데이터를 빠르게 가지고 올 수 있으며 균형성 검사를 할 수 있어 **수식, 괄호와 같은 검사에서도 많이 쓰인다.**  
**Stack과 같은 모든 자료구조는 `동적할당`을 수행한다.** Java에서 제공되고 있는 Stack클래스는 `Vector`클래스를 상속받아 구현이 이루어지고 있다. 또한 기본적으로 Stack 클래스는 내부에서 최상위 타입 배열인 **`Object[] 배열`**을 사용하여 데이터를 관리한다.  
  
해당 포스팅은 자료구조 중 하나인 **Stack**에 대해 이해하기 위해 스터디 목적으로 직접 Stack을 구현해보는 포스팅이다. Java에서 지원하는 Stack 클래스에 대한 사용법이 아니다. 실습을 진행할 때 사용된 코드는 모두 [Stranger's Lab](https://st-lab.tistory.com/173)님의 게시글을 참고로 하여 구현하였다.

# 구현할 Stack 인터페이스의 메소드
1. push(E item) - return type : E - 스택의 TOP에 요소를 추가한다.  
2. pop() - return type : E - 스택의 TOP에 있는 요소를 제거하고 제거된 요소를 반환한다.  
3. peek() - return type : E - 스택의 TOP에 있는 요소를 제거하지 않고 반환한다.  
4. search(Object o) - return type : int - 스택의 TOP에서부터 지정된 객체가 있는 요소의 위치를 반환한다.(없으면 -1 반환)  
5. size() - return type : int - 현재 스택에 있는 요소의 개수를 반환  
6. clear() - return type : void - 스택에 있는 모든 요소를 제거  
7. empty() - return type : int - 현재 스택에 요소가 존재하지 않는다면(스택이 비어있다면) true, 그 이외는 false 반환  

# Stack 인터페이스 구현 (주석 없는 코드)

```java
public interface StackInterface<E> {
    E push(E item);
    E peak();
    int search(E value);
    int size();
    void clear();
    boolean empty();
}

```

# Stack 인터페이스 구현 (주석 있는 코드)

```java
package stack;

// 스택은 Vector 클래스를 상속받아 사용한다. 내부 구조는 Object[]이기에 전체적인 메소드 구조와 유사하다.
public interface StackInterface<E> { // 효율적인 자료구조를 구축하기 위해 제네릭 사용

    // 스택 top에 요소를 추가하는 push 구현
    E push(E item);

    // 스택 top에 있는 요소를 제거하는 pop 구현
    // @return 제거된 요소
    E pop();

    // 스택 top에 요소를 제거하지 않고 반환하는 peek 구현
    // @return 스택의 맨 위에 있는 요소
    E peak();

    // 스택의 top부터 특정 요소가 몇 번째 위치에 있는지 반환.
    // 중복되는 원소가 있을 경우 가장 위에 있는 요소의 위치가 반환된다.
    // @return 스택의 top부터 요소가 일치하는 첫 번째 위치를 반환.
    //          만약, 일치하는 요소가 없을 시 -1 반환.
    int search(E value);

    // 스택의 요소 개수를 반환
    // @return 스택의 요소 개수
    int size();

    // 스택에 있는 모든 요소 삭제
    void clear();

    // 스택에 요소가 비어있는지 확인
    // @return 스택에 요소가 없으면 true, 그 이외는 false
    boolean empty();
}
```
  
```java
public interface StackInterface<E> { ... }
```
해당 인터페이스 선언에서 제네릭 E를 선언하여 스택 클래스의 Ojbect를 구현한다.  
  
  
  
---  
[참고 및 리소스 출처]  
[Stranger's LAB](https://st-lab.tistory.com/173)  
[코딩팩토리](https://coding-factory.tistory.com/601)  