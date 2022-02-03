---
layout: post
title:  "[Spring] 스프링 MVC"
date:   2022-02-03 16:15:00+0900
categories: jekyll update
tags: [spring]
---
# Spring MVC
스프링 MVC는 MVC2 모델의 방식이다.  
**모델(Model), 뷰(View), 컨트롤러(Controller)**가 나누어져 데이터를 처리하는 비즈니스 로직을 분리하여 작업성과 재사용성을 극대화한 패턴이다.(MVC2)  
즉, MVC2의 패턴은 아래와 같은 원리이다.  
1. 클라이언트에서 Request 발생 시 Controller가 Request를 받음.  
2. Controller는 Request에 해당하는 Model을 호출함.  
3. 호출된 Model은 Request에 필요한 데이터들을 처리하고 Controller에게 Request에 대한 Response를 보냄.  
4. Controller는 Response를 View에게 보냄.  
5. View는 Response를 클라이언트에 뿌려줌.  

**! (Request -> Controller -> Model -> Controller - View)**  
<p align="center"><img src="/assets/img/blog/정보/SpringMVC1.png"></p>
<center><small><i>Spring MVC의 DispatcherServlet을 도식화한 것</i></small> <br>
<i><small>출처 : htttp://min-it.tistory.com</small></i></center>

# 설명
**DisaptcherServlet**은 **컨트롤러(Contorller)**이다.  
1. 사용자의 요쳥을 DisaptcherServlet이 받는다.  
2. DisaptcherServlet은 핸들러 매핑(HandlerMapping)을 이용하여 사용자의 요청에 해당하는 Controller를 실행시킨다.  
3. 실행된 Controller는 적절한 Service 객체를 호출한다.  
4. 호출된 Service 객체는 데이터 처리(DB)를 위해 DAO를 이용하여 데이터를 요청한다.  
5. DAO는 요청된 데이터의 처리를 위해 MyBatis의 Mapper을 통하여 작업을 처리한다.  
6. 처리된 결과는 Mapper -> DAO -> Service -> Controller 순으로 다시 전달된다.  
7. Controller는 처리된 데이터를 View Resolver를 통하여 전달 받을 View가 존재하는지 탐색한다.  
8. 전달 받은 View가 존재한다면 View에게 처리된 데이터를 전달한다.  
9. View는 처리된 데이터를 DisaptcherServletdprp 전달한다.  
10. DisaptcherServlet은 전달받은 데이터를 클라이언트에게 Response한다.  

# Front Controller(DisaptcherServlet)
서버로 들어오는 모든 Request를 받아서 처리하며 각 컨트롤러들 사이의 중복된 코드 문제나 협업의 문제를 해결할 수 있다.  

# 자세한 M(Model) / V(View) / C(Controller) 설명
- Model
    : Application의 상태(데이터)를 나타낸다.  
    : POJO(Plain Old Java Object, 오래된 방식의 간단한 자바 오브젝트)로 구성된다.  
    : Java Beans이다.  
- View  
    : 디스플레이 데이터  
    : Model의 데이터 렌더링 처리를 한다.  
    : JSP, Thymeleaf, Groovy, FreeMarker 등 여러가지 템플릿 엔진(Template Engine)이 존재한다.  
- Controller  
    : View와 Model 사이의 인터페이스 역할을 수행한다.  
    : Model/View에 대한 사용자 입력과 요청을 수신하여 그에 해당하는 결과를 Model에 담아 View에 전달한다.  
    : MO(Model Object)와 Model을 화면에 출력할 View Name 반환.  
    : Controller -> Service -> DAO -> DB  
    : Servlet  

# Spring Framework의 대표적인 Class
## DispatcherServlet
- Spring Framework가 제공하는 Servlet 클래스  
- 사용자의 Request를 받는 역할  
- Dispatcher가 받은 Request는 HandlerMapping으로 전달  

## HandlerMapping
- 사용자의 Request를 처리할 Controller를 탐색(Controller URL Mapping)  
- Request URL에 해당하는 Controller 정보를 저장하는 Table을 가짐  
- 클래스에 @RequestMapping("/url") 어노테이션을 명시하면 해당 URL에 대한 Request가 발생했을 때 Table에 저장된 정보에 따라 해당 클래스 또는 메소드에 Mapping 처리.  

## ViewResolver
- Controller가 반환한 View Name(the Logical Names)에 prefix, suffix를 적용하여 View Object(The Physical View Files)를 반환  
- ex: **view name: home, prefix: /WEB-INF/views/, suffix: .jsp**는 **"/WEB-INF/views/home.jsp"**라는 위치의 View(JSP)에 Controller에 받은 Model를 전달한다는 의미  
- 해당 View에서 이 Model 데이터를 이용하여 적절한 페이지를 만든 후 사용자에게 뿌려준다.  

[참고]  
[TutorialsPoint](https://www.tutorialspoint.com/spring/spring_web_mvc_framework.htm)  
[gmlwjd9405.github.io](https://gmlwjd9405.github.io/2018/12/20/spring-mvc-framework.html)  
[min-it](https://min-it.tistory.com/7)
