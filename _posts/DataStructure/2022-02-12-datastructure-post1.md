---
layout: post
title: "[Java, 자료구조] 컬렉션 프레임워크"
data:   2022-02-12 15:45:00+0900
categories: jekyll update
tags: [java, data structure]
---
# 컬렉션 프레임워크(Collection Framework)
자바에서 컬렉션 프레임워크(Collection Framework)는 다수의 데이터를 효과적으로 처리할 수 있는 방법을 제공하는 클래스의 집합을 말한다. 자바는 `java.util`패키지에 `Collection`을 구현한 클래스와 인터페이스를 지원한다. 쉽게 말해 데이터를 저장하는 자료 구조와 데이터를 처리하는 알고리즘을 구조화하여 클래스로 지원을 해준다.  
자바에서 지원하는 `Collection`은 크게 3가지 인터페이스로 나뉘어 있다.

# 주요 인터페이스
컬렉션 프레임워크(Collection Framework)에서는 데이털르 저장하는 자료 구조에 따라 핵심 인터페이스를 정의하고 있다.  
1. **`List`** 인터페이스  
2. **`Set`** 인터페이스  
3. **`Map`** 인터페이스
*List와 Set은 Collection 인터페이스를 상속받지만 Map은 Collection을 상속받지 않는다.*

# 주요 인터페이스 상속 관계

<p align="center"><img src="/assets/img/blog/정보/컬렉션1.png"></p>  
<p align="center"><img src="/assets/img/blog/정보/컬렉션2.png"></p>

# 주요 자료구조 분류법
## 선형 자료구조(Linear Data Structure)
데이터가 **일렬**로 연결된 형태. 흔히 사용하는 int[] 배열을 생각하자. 대표적인 선형 자료구조는 `리스트(List)`, `큐(Queue)`, `덱(Deque)`이 존재한다.  
<p align="center"><img src="/assets/img/blog/정보/선형.png"></p>
<center>
<small><i>출처 : https://goodgid.github.io/DS-Linear-and-NonLinear/</i></small>
</center>

## 비선형 자료구조(Nolinear Data Structure)
데이터가 **일렬**로 나열된 것이 아닌, 각 요소가 여러 개의 요소와 연결된 형태. *(대표적으로 거미줄이 있음.)* 대표적인 비선형 자료구조는 `그래프(Graph)`와 `트리(Tree)`가 있다.  
<p align="center"><img src="/assets/img/blog/정보/비선형.png"></p>
<center>
<small><i>출처 : https://goodgid.github.io/DS-Linear-and-NonLinear/</i></small>
</center>

## 집합(Set)
집합의 의미를 가진 자료구조 `Set`은 보통 집합 자료 구조로 본다. **집합(Set)의 경우는 데이터가 연결 된 형식이 아닌 table에 가까운 자료구조다.**

## 정리
### 자료구조
<p align="center"><img src="/assets/img/blog/정보/컬렉션3.png"></p>
<center>
<small><i>출처 : https://goodgid.github.io/DS-Linear-and-NonLinear/</i></small>
</center>

### 컬렉션 인터페이스에서 제공하는 주요 메소드
<p align="center"><img src="/assets/img/blog/정보/컬렉션 정리.png"></p>
<center>
<small><i>출처 : http://www.tcpschool.com/java/java_collectionFramework_concept</i></small>
</center>
  
  
---
[참고 및 리소스 출처]  
[Stranger's LAB](https://st-lab.tistory.com/142)  
[TCPschool](http://www.tcpschool.com/java/java_collectionFramework_concept)  