---
layout: post
title:  "[Spring] 스프링 빈(Spring Bean)"
date:   2022-02-03 18:00:00+0900
categories: jekyll update
tags: [spring]
---
# Bean이란?
**Spring IoC 컨테이너가 관리하는 자바 객체를 빈(Bean)이라 한다.**  
기존의 Java 객체는 new 연산자를 사용하여 필요한 객체를 생성한 후 사용을 하였다. Spring에서는 개발자가 직접 new를 이용하여 생성한 객체가 아닌, Spring에 의하여 관리당하는 자바 객체를 사용한다. 즉 **Spring에 의하여 생성, 관리, 제어하는 자바 객체를 Bean**이라고 한다. Spring Framework에서는 Spring Bean을 얻기 위하여 **ApplicationContext.getBean()와 같은 메소드를 사용하여 Spring에서 직접 자바 객체를 얻어 사용한다.**  

# Spring IoC 컨테이너에 Bean 등록하기
1. @Component 어노테이션 사용하기
**@Component 어노테이션이 등록되어 있으면 Spring이 해당 어노테이션을 확인하고 자체적으로 Bean으로 등록한다.**
2. Cofiguration에 직접 등록하기
**@Configuration과 @Bean** 어노테이션을 이용하여 직접 Bean을 등록한다.
```java
//DataConfiguration.java
@Configruation
public class DataBasesConfiguration{
    @Bean
    public DataController addController(){
        return new addController;
    }
}
```

---  
[참고]  
[melonicedlatte](http://melonicedlatte.com/2021/07/11/232800.html)