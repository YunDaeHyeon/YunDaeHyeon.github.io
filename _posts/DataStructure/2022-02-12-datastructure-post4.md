---
layout: post
title: "[Java, 자료구조] Set 인터페이스"
data:   2022-02-12 17:27:00+0900
categories: jekyll update
tags: [java, data structure]
---
# Set 인터페이스
- '집합'의 의미를 가진 인터페이스  
- 데이터를 중복해서 저장할 수 없다.  
- 입력 순서대로의 저장 순서를 보장하지 않는다. *(LinkedHashSet은 입력 순서대로 저장 순서를 보장하지만 데이터를 중복해서 저장할 수 없다.)*  
**List 계열은 index(노드, Node)로 관리하기에 add()와 같이 요소를 대입하면 순서대로 저장된다.**  
**Queue 계열은 우선순위 큐(PriorityQueue)를 제외하고 기본적으로 입력한 순서대로 객체가 연결되어 있다.**  
**Set의 경우는 일반적으로 입력받은 순서와 상관없이 데이터를 집합시키기 때문에 입력받은 순서가 보장될 수 없다.**  
  
(입력받는 순서가 보장이 되지 않는다는 불편함을 개선시키기 위해 만들어진 클래스가 `LinkedHashSet`이며 **데이터의 중복은 허용하지 않지만 입력 순서를 보장하고 싶다면 해당 클래스를 사용하자.**)

## Set 인터페이스를 구현하는 클래스
1. HashSet  
2. LinkedHashSet  
3. TreeSet  

## Set 인터페이스에 선언된 대표적인 메소드
<p align="center"><img src="/assets/img/blog/정보/셋 메소드.png"></p>
<center>
<small><i>출처 : https://st-lab.tistory.com/142</i></small>
</center>

**`Set`의 가장 큰 특징은 `데이터 중복 X`와 `입력 순서대로 저장순서를 보장하지 않는다.`이다.**

## HashSet
- 가장 기본적인 Set 컬렉션 클래스  
- 입력 순서를 보장하지 않고, 데이터의 중복도 보장하지 않는다.  
- Hash에 의해 데이터의 위치를 특정시켜 해당 데이터를 빠르게 색인(검색)할 수 있다.  
- Hash + Set 컬렉션이며 삽입, 삭제, 색인이 매우 빠르다.  
(예시 : 게임에서 닉네임을 만들 때, 혹은 아이디를 생성할 때 '중복확인'을 통해 중복된 닉네임이나 아이디인지 확인할 때!)  
- 자동 정렬을 지원하지 않는다.  
- 중복을 보장하지 않아 중복을 자동으로 제거한다.  

## LinkedHashSet
- Link + Hash + Set의 형태  
- LinkedList의 경우 FIFO의 형태를 가지고 있어 데이터의 입력 순서를 보장한다. 이를 보완하기 위해 **중복은 허용하지 않지만 순서를 보장하고 싶은 경우**에 사용한다.  

## TreeSet
- 입력 순서를 보장하지 않고 중복 데이터를 허용하지 않는다.  
- 자동 정렬을 지원한다.  
- 데이터 중복을 보장하지 않아 데이터 중복을 자동으로 제거한다.  
- 데이터의 '가중치에 따른 순서'대로 정렬된다.  
- `SortedSet 인터페이스`를 구현한다.  
- 데이터가 중복되지 않으면서 특정 규칙에 의해 정렬된 형태의 집합을 사용하고 싶을 때 사용한다. (정렬된 형태로 존재하다 보니 특정 구간의 집합 요소들을 탐색할 때 유용하다.)  

## 생성 방법
  
```java
// primitive Type은 생성이 불가능하다.
HashSet<T> hashset = new HashSet<>();
LinkedHashSet<T> linkedhashset = new LinkedHashSEt<>();
TreeSet<T> treeset = new TreeSet<>();

SortedSet<T> treeset = new TreeSet<>();
Set<T> hashset = new HashSet<>();
Set<T> linkedhashset = new LinkedHashSet<>();
Set<T> treeset = new TreeSEt<>();

```
---
[참고 및 리소스 출처]  
[Stranger's LAB](https://st-lab.tistory.com/142)  
[TCPschool](http://www.tcpschool.com/java/java_collectionFramework_concept)  