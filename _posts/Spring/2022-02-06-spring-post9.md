---
layout: post
title:  "[Spring] Spring Security란?"
date:   2022-02-06 00:12:00+0900
categories: jekyll update
tags: [spring]
---
# 스프링 시큐리티(Spring Security)
**스프링 시큐리티(Spring Security)**는 포괄적인 보안 서비스를 제공하는 스프링의 오픈 플랫폼이다.  
즉, Spring 기반 애플리케이션의 보안을 담당하는 스프링 하위 프레임워크이며 **스프링 시큐리티(Spring Security)**는 
강력하고 많은 보안 관련 옵션들을 제공하여 개발자가 보안 로직을 작성하지 않아도 되는 편리함이 존재한다.  
(*하지만 너무 어렵다...*)  
**스프링 시큐리티(Spring Security)**는, **'인증(Authntication)'**과 **'권한(Authorization)'**으로 이루어져 있으며
해당 두 가지의 부분을 Filter의 흐르에 따라 처리하고 있다.  
<p align="center"><img src="/assets/img/blog/정보/spring security.png"></p>

# 인증(Authntication) / 권한(Authorization)
  
## 인증(Authntication)
 > 사용자가 본인인지 확인한다. (해당 애플리케이션에 접속하려는 사람은 누구인가?)  
 > Session과 Token을 관리한다. (UsernamePassword를 통한 인증)  
 > SNS로그인을 통한 인증 **위임** 가능.  

## 권한(Authorization)
 > **권한**을 설정한다. (사용자가 어떤 일을 할 수 있는가?)  
 > 특정 페이지/리소스에 접근할 수 있는지 **권한**을 판단한다.  
 > 여러가지 어노테이션(ex: Secured, PrePostAuthorize 등)을 이용하여 쉽게 **권한** 체크 가능.  
 > ***비즈니스 로직이 복잡하면, AOP를 이용하여 권한을 체크해야 한다...***  

# 스프링 시큐리티(Spring Security) Dependency 추가
```java
implementation 'org.springframework.boot:spring-boot-starter-security'
implementation 'org.thymeleaf.extras:thymeleaf-extras-springsecurity5'
! Gradle
```
  
```java
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
! Maven
```
  
[참고]  
[seongwon97님](https://velog.io/@seongwon97/Spring-Security-Spring-Security%EB%9E%80)  
