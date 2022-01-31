---
layout: post
title: "[Spring Boot] 타임리프(Thymeleaf)란?"
data:   2022-01-30 22:10:00+0900
categories: jekyll update
tags: [spring boot]
---
# 타임리프(Thymeleaf)

**타임리프(Thymeleaf)**는 **템플릿 엔진**의 일종이며 웹과 웹 환경이 아닌 양쪽에서 텍스트, HTML, XML, JS, CSS 등을 생성할 수 있는 **템플릿 엔진**이다.  
**타임리프(Thymeleaf)**는 **스프링 MVC**와의 통합 모듈을 제공하고 Application에서 JSP로 만든 기능들을 대체할 수 있다.  
즉, JSP 처럼 HTML 태그에 속성을 추가하여 동적으로 값을 처리할 수 있다.  
```html
<input type = "text" value = "test" th:value="${item}">
=> th:value는 Thymeleaf의 문법이다.
```

# 템플릿 엔진
임의로 지정된 템플릿 양식과 데이터가 합쳐져 HTML 문서를 출력하는 소프트웨어를 말하며, **서버 템플릿 엔진**, **클라이언트 템플릿 엔진**으로 나누어진다. 이때, **타임리프(Thymeleaf)**는 **서버 템플릿 엔진**에 속한다.

# 타임리프(Thymeleaf) 사용
ThymeleafViewResolver를 등록하여 타임리프를 사용한다.  

```java
implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'
* Gradle(build.gradle)
```
  
```java
<dependency> <groupId>org.springframework.boot</groupId> <artifactId>spring-boot-starter-thymeleaf</artifactId> </dependency>
* Maven(pom.xml)
```
  
```html
<html xmlns:th="http://www.thymeleaf.org">
! 타임리프를 사용할 HTML에 추가.
```

# 문법
1. @{...} URL 링크 표현식  
2. \|...\| 리터널 대체  
3. ${...} 변수  
4. th:each 반복 출력  
5. *{...} 선택 변수  
6. #{...} 메세지를 뜻하며 properties 같은 외부 자원에서 해당 코드에 해당하는 문자열을 불러온다.
  
---
참고  
    1. [연로그](https://yeonyeon.tistory.com/153)