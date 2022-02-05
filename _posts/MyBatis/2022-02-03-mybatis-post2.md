---
layout: post
title:  "[MyBatis] 카멜 표기법(Camelcase)"
date:   2022-02-03 17:43:00+0900
categories: jekyll update
tags: [mybatis]
---
# 카멜 표기법
일반적으로 자바는 **카멜 표기법**을 사용한다.  
즉, *카멜 케이스(Camelcase)*라고 불리는 표기법은 각 단어의 첫 글자만 대문자로 표기하여 낙타 등처럼 보이도록 작성하는 표기법.  
(ex : private String userName, private String updaterId)  
자바의 **클래스 이름은 대문자**로 시작하고, 변수나 메소드 이름은 클래스의 이름과 구분하기 위하여 **소문자**로 시작한다.  

# 데이터베이스는?
데이터베이스의 경우 _(underscore)를 사용하는 스네이크(Snake_case) 표기법을 사용한다.  
즉, 자바는 **카멜** DB는 **스네이크**를 사용하여 이를 위한 조치가 필요하다.  
예를 들어 DB에서 user의 name을 조회하면 **user_name**이 조회되지만 자바(DTO)에서 user의 name은 **userName**으로 되어 있어
자바와 DB를 매핑해야하는 필요가 생긴다.

# 자바 <-> DB 매핑
```java
mybatis.configuration.map-underscore-to-camel-case=true
// 기본적으로 MyBatis의 mapunderscoreToCamelCase는 false로 설정되어 있기에 이를 true로 바꾼다.
# 위치 : application.properties에 추가
```
  
```java
@Bean
@ConfigurationProperties(prefix="mybatis.configuration")
public org.apache.ibatis.session.Configuration mybatisConfig(){
    return new org.apache.ibatis.session.Configuration();
}
# 위치 : DatabaseConfiguration.java에 추가
```
  
```java
sqlSessionFactoryBean.setConfiguration(mybatisConfig());
# 위치 : DatabaseConfiguration 클래스 sqlSessionFactory 메소드에 추가
```
  
[참고]  
[프로그래밍 인사이트(스프링부트 시작하기) - 김인우님](http://www.yes24.com/Product/Goods/70893395)