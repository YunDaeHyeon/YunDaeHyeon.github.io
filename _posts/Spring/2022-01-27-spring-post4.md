---
layout: post
title:  "[spring] 그레이들(Gradle)"
date:   2022-01-27 00:39:00+0900
categories: jekyll update
tags: [spring]
---
# Gradle이란?
Gradle은 그루비(Groovy)를 기반으로 한 빌드 도구이며 *Ant*와 *Maven*과 같은 이전 세대 빌드 도구의 단점을 보완하고 장점을 취합하여 만든 오픈소스로 공개된 빌드 도구  

# 특징
Gradle은 *Ant*와 *Maven*이 가지고 있는 장점을 모아 만들었으며 의존성 관리를 위한 다양한 방법을 제공하고 Build 스크립트를 메이븐의 XML가 아닌 JVM에서 동작하는 스크립트 언어(Groovy기반) DSL를 사용한다.  
**메이븐(Maven)의 pom.xml을 Gradle용으로 변환할 수 있고 Maven의 중앙 저장소도 지원하기 때문에 라이브러리를 모두 가져다 사용할 수 있다.**  
또한 기본적으로 빌드 배포 도구이며 Maven에 비해 최대 100배 가량 빠른 속도를 자랑한다.  
**빌드툴인 Ant Builder와 Groovy 스크립트를 기반으로 구축되어 기존 Ant의 역할과 배포 스크립트의 기능을 모두 사용 가능하다.**

[학습한 내용의 출처]  
[madplay](https://madplay.github.io/post/what-is-gradle)