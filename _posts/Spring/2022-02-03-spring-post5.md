---
layout: post
title:  "[Spring] MVC 패턴이란?"
date:   2022-02-03 00:44:00+0900
categories: jekyll update
tags: [spring]
---
# MVC란?
**: 모델-뷰-컨트롤러(Model-View-Controller, MVC)**  
**모델-뷰-컨트롤러 이하 MVC**는 소프트웨어 디자인 패턴 중 하나이다.  
해당 패턴은 UI로부터 **비즈니스 로직**을 분리하여 Application의 시각적 요소나 비즈니스 로직을 서로 영향 없이 쉽게 고칠 수 있는 애플리케이션을 만들 수 있다.
<p align="center"><img src="/assets/img/blog/정보/MVC1.png"></p>
<center><small><i>MVC의 관계 다이어그램</i></small> <br></center>
<p align="center"><img src="/assets/img/blog/정보/MVC2.png"></p>
<center><small><i>Web Application의 일반적인 MVC 다이어그램</i></small> <br></center>
  
# 모델
모델(Model)이란 어떠한 동작을 수행하는 코드를 말하며 *모델의 상태에 변화가 있을 때 **컨트롤러**와 **뷰**에 이를 통보한다. **컨트롤러**는 **모델의 변화**에 따른 적용 가능한 명령을 추가 / 제거 / 수정할 수 있다.*

# 뷰(View)
MVC에서 ***모델은 여러 개의 뷰(View)를 가질 수 있다.*** **뷰**는 모델(보여줄 값)을 **컨트롤러**부터 받아와 사용자에게 보여주는 역할을 한다. 이 뜻은 사용자가 볼 결과물을 생성하기 위해 **모델**로부터 정보를 얻어옴을 뜻한다.  

# 컨트롤러(Controller)
MVC에서 View는 여러 개의 컨트롤러(Controller)를 가지고 있다. 사용자는 **컨트롤러**를 사용하여 **모델**의 상태를 바꾸며 **컨트롤러**가 관련된 **뷰**에 명령을 보냄으로써 **모델**의 표시 방법을 바꿀 수 있다.  

# 요약
 - Model : 어플리케이션의 모든 정보, 데이터, DB를 뜻한다.  
 - View : 사용자에게 뿌려지는 UI 즉, 화면을 말한다. View는 모델(Model)로부터 정보를 얻고 화면에 표시한다.  
 - Controller : 어플리케이션의 모든 데이터들과 비즈니스 로직 사이의 상호 동작을 통제하고 관리한다. 이는 모델(Model)과 뷰(View)를 관리하는 것이며 모델(Model)과 뷰(View)가 서로 직접적으로 연관되지 않도록 관리한다.  

> **모델(Model), 뷰(View), 컨트롤러(Controlller)로 나누어진 패턴을 MVC패턴이라고 부르며 MVC1, MVC2로 나누어 진다.**

# MVC1 패턴
<p align="center"><img src="/assets/img/blog/정보/MVC3.png"></p>
<center><small><i>JSP와 Bean 위주로 이루어진 MVC1 패턴</i></small> <br></center>

MVC1은 **JSP(Java Server Page)**가 MVC의 View와 Controller를 모두 담당한다. 이 형태는 JSP라는 도구 하나로 모든 Request와 Response를 처리할 수 있어 난이도가 쉽지만 프로젝트가 점점 커지고 방대해질수록 극도로 유지보수가 어려워지는 단점이 있다.  
쉽게 말하면 유저의 Request에 Response하기 위하여 모든 MVC를 사용하기에 매우 효율성이 떨어지며 관리가 힘들다는 문제가 있다.  

# MVC2 패턴
<p align="center"><img src="/assets/img/blog/정보/MVC4.png"></p>
<center><small><i>서블릿(Servlet)이 컨트롤러를 담당하며 컨트롤러와 뷰가 분리되어 있는 모습의 MVC2 패턴</i></small> <br></center>

MVC2는 많은 사람들이 사용하는 패턴이다. 해당 패턴은 클라이언트(User)의 Request를 **컨트롤러(Servlet)**이 받아 뷰와 모델에 던지는 방식이다. 가장 큰 이점은 **컨트롤러와 뷰**가 분리되어 있기에 재사용성과 유지보수에 매우 큰 힘을 보여준다.  이는 큰 프로젝트에 매우 적합한 패턴임을 알 수 있다.  
하지만, 구조가 복잡하여 처음 접하기에는 많이 어려울 수 있지만 이러한 구성까지 전부 알 필요 없이 MVC2 구축을 도와주는 각종 프레임워크들이 매우 많이 존재한다.  

---
[참고]  
[wikipedia](https://ko.wikipedia.org/wiki/%EB%AA%A8%EB%8D%B8-%EB%B7%B0-%EC%BB%A8%ED%8A%B8%EB%A1%A4%EB%9F%AC)
[chanhuiseok](https://chanhuiseok.github.io/posts/spring-3/)