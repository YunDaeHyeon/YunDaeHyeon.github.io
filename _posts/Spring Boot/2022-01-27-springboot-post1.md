---
layout: post
title:  "[Spring Boot] 프로젝트 구조"
date:   2022-01-27 17:15:00+0900
categories: jekyll update
tags: [spring boot]
---
# Spring Boot 프로젝트의 구조

<p align="center"><img src="/assets/img/blog/스프링 부트/spring boot 1.png"></p>

# 주요 파일 및 구조

- **src/main/java**  
    *: 자바 소스 디렉터리*  
- **SampleApplication**  
    *: 애플리케이션을 시작할 수 있는 main 메소드가 존재하는 스프링 구성 메인 클래스*  
- **templates**  
    *: 스프링 부트에서 사용 가능한 여러 가지 뷰 템플릿(Thymeleaf, Velocity, FreeMarker etc..) 파일 위치*  
- **static**  
    *: 스타일 시트, 자바스크립트, 이미지 등의 정적 리소스 디렉터리*  
- **application.properties**  
    *: 애플리케이션 및 스프링의 설정 등에서 사용할 여러 가지 프로퍼티(Property)정의*  
- **Project and External Dependencies**  
    *: 그레이들에 명시한 프로젝트의 필수 라이브러리 모음*  
- **src**  
    *: JSP 등 리소스 디렉터리*  
- **build.gradle**  
    *: 그레이들 빌드 명세, 프로젝트에 필요한 라이브러리 관리, 빌드 및 배포 설정*  
  
*! test 폴더에는 JUnit을 이용한 테스트 클래스 존재.*  
*! gradle, gradlew, gradlew.bat은 그레이들 설정 및 실행 관련 스크립트 정보를 보유.*  

## SampleApplication 클래스
 - Spring Boot Application의 구성과 실행을 담당  
<p align="center"><img src="/assets/img/blog/스프링 부트/spring boot 2.png"></p>
  
**@SpringBootApplication**  
 - 스프링 부트의 핵심 어노테이션  
 - 스프링 부트의 어노테이션 3개로 구성되어 있음  

1. *@EnableAutoConfiguration*  
해당 어노테이션을 사용하면 스프링의 다양한 설정이 자동으로 완성된다.  
2. *@ComponentScan*  
기존 스프링은 빈(bean) 클래스를 사용하기 위해 XML에 일일이 빈을 선언해야함.  
해당 어노테이션은 컴포넌트 검색 기능을 활성화하여 자동으로 여러 가지 컴포넌트 클래스를 검색하고 검색된 컴포넌트 및 빈 클래스를 스프링 애플리케이션 컨텍스트에 등록하는 역할을 한다.  (ex : "안녕하세요"를 출력하기 위해 Controller을 작성할 때 @RestController을 사용하여 스프링 MVC 컨트롤러가 만들어지는 이유는 해당 어노테이션의 자동 검색 및 구성으로 인하여 발생하는 현상)  
3. *@Configuration*  
@SpringBootApplication에는 @springBootConfiguration이라는 어노테이션이 포함되어 있는데 해당 어노테이션에 @Configuration이 포함되어 있다.  
@Configuration이 붙은 Java 클래스는 자바 기반 설정 파일임을 의미함.  
자바 기반의 설정은 @Configuration 어노테이션이 붙은 클래스가 설정 파일임을 스프링 프레임워크에 알려줌.  

<br><small><i>정보 출처 : 인사이트-스프링부트 시작하기-김진우님</i></small>