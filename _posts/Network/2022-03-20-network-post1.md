---
layout: post
title:  "[Network] OSI 7계층과 Layer"
date:   2022-03-20 18:27:00+0900
categories: jekyll update
tags: [Network]
---
# OSI 7계층
**OSI 모형(Open Systems Interconnection Reference Model)은 국제표준화기구(ISO)에서 개발한 모델로, 컴퓨터 네트워크 프로토콜 디자인과 통신을 계층으로 나누어 설명한 것이다. 일반적으로 OSI 7 계층이라고 한다.**

# 프로토콜(Protocol)
인터넷 시대에서 도선으로 연결하여 사용자간 신호를 주고 받으면 될 것 같지만, 인터넷 사용자가 매우 많기에 여러가지 혼선이 발생하고 충돌이 생겨 일종의 규약을 만들었다. 이 규약이 **프로토콜**이다.  
  
프로토콜은 `syntax, sematic, timing`으로 구성되는데, 특히 `syntax` 즉 전송되는 데이터 형식 혹은 구조를 규정한 것이 송신축, 수신측, 모두 같은 `syntax`로 구성되어야 한다. 또한 `Sematic`은 전송되는 데이터를 구성하는 각 필드들의 의미를 나타낸다. 즉, 데이터의 각 필드의 값을 어떤 의미로 해석을 해야하며 그에 따라서 어떤 동작이 수행되어야 하는지 등을 규정한 것이다.  
`timing`은 데이터를 전송하는 시점과 데이터 전송속도를 규정한 것이다. 즉, 송신속도가 10Mbps라면 수신속도도 10Mbps이어야 한다. 이들은 각 layer를 통해 구현된다.  

# Layer란?
프로토콜은 모두 `Layer`이라는 단계로 구성된다.  
`Layer`은 기능별로 계층을 만들어 놓은 것이며, L1가 어떤 기능을 수행한다면 그것을 이용하여 L2가 기능을 하고 L3, L4, L5로 쭉 올라가서 통신을 완성하는 것이다.  
  
**이때, 한 서비스를 제공하는 한 개의 층으로 전자소자의 Module 개념과 비슷하나 Layer는 하위 Layer에 서비스에 의존한다.**

## 예시

<p align="center"><img src="/assets/img/blog/정보/네트워크 1.png"></p>

어떠한 컴퓨터에서 `Application layer`에 해당하는 어떤 신호를 발송하면 `Presentation Layer`에서 알집을 압축하는 것처럼 기능을 붙이고 그 후 `Session layer`, `Transport Layer`를 거쳐 `Network layer`로 간다.  
그 신호가 `Data link layer`, `physical layer`로 신호가 전송되고 옆 통신체계에서는 다시 `Data link layer`, `network layer`로 해석하며 다시 올라가서 원하는 주소로 매칭하여 전달하고자 하는 PC의 `Physical layer`로 간 뒤 `Data link`부터 쭉 올라가서 원래 전하고자했던 `Application layer`를 전달하게 된다.  
  
## Layer을 왜 사용하는가?
**기능별로 나누기 때문에 이해/구현이 편하며, 문제가 있을 시 한 개의 `layer`만 해결하면 되므로 발전시키기 좋고 관리가 편하다. 또한 인터넷 연결이 매우 복잡한 시스템이기 때문에, 구분되어있는 구조는 다른 `layer`의 동작에 영향을 주지 않는다.**  

# 표준안(Standard)
표준안(standards)는 `de facto standard`와 `de jure standard`로 구성되는데 `de facto standard`는 널리 사용되고 있기에 자연스럽게 표준안이 된 것이고 `de jure standard`는 국제기구에 의해서 공식적인 절차로 규정된 표준안이다. **ISO의 OSI 7 layer는` de jure standard`이고 TCP/IP는 `de facto standard`이다.**  
  
또한, 표준은 `Reference model`, `Service Architecture`, `Protocol Architecture`로 나뉜다.  
1. `Reference model`은 통신을 위한 7 Layer Model을 정의하는데 필요한 layer, service, service access point, name과 같은 개념을 정의한다. 각 layer에서는 하위 layer의 service를 사용하며, 상위 layer에게 service access point를 통해 service를 제공한다.  
  
2. `Service Architecture`에서는 layer에서 제공하는 service와 그와 같은 service를 제공하는 service access point interface를 규정한다. 단, 실제 service를 제공하는 protocol을 규정하지는 않는다.  
  
3. `Protocol Architecture`는 Service Architecture에서 구현하는데 사용한 protocol의 집합을 규정하고 동일한 Service Architecture를 구현하더라도 서로 다른 protocol Architecture를 사용할 수 있다.  
  
  
  
---  
[참조]  
[IT 톺아보기](https://depletionregion.tistory.com/83)  