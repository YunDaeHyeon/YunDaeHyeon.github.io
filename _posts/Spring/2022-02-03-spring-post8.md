---
layout: post
title:  "[Spring] IoC(Inversion of Control), DI(Dependency Injection)"
date:   2022-02-03 18:17:00+0900
categories: jekyll update
tags: [spring]
---
# 의존성 역전(IoC, Inversion of Control)
객체의 의존성을 역전시켜 객체 간의 결합도를 줄이고 유연한 코드를 작성하게 하여 **가독성, 코드의 중복, 유지보수**를 극대회 시키는 방법.  
일반적으로 자바에서는 **각 객체들이 프로그램의 흐름을 결정하고 각 객체를 개발자가 직접 생성하고 조작**하였다. 만약, Apple 객체에서 Banana 객체에 있는 메소드를 사용하고 싶다면, Banana 객체를 Apple 객체 내에서 생성하고 메소드를 호출해야 했다. 하지만, **IoC**가 적용된 경우 **객체의 생성을 특별한 관리 위임 주체 (Spring)**에게 맡겨 사용자는 **객체를 직접 생성하지 않고,** 객체의 생명주기를 컨트롤하는 주체는 다른 주체가 된다.  
***즉, 사용자의 객체 제어권을 다른 주체에게 넘기는 것을 IoC라고 한다!***  

# 의존성 주입(DI, Dependency Injection)
객체를 **직접 생성하는 것이 아닌, 외부에서 생성한 후 주입시켜주는 방식**을 말한다.  
즉, 객체 자체가 아니라 Framework에 의해 객체의 의존성이 주입되는 것을 말한다.

---
```java
class Main{
    private Sample sample = new Sample();
}
```
위 코드는 Main이라는 클래스에서 Sample이라는 객체를 불러온다는 의미이며 여기서 **의존**은 ***new 키워드를 통해 생성된다.*** 이때, 객체 **sample**의 제어권은 **클래스 Main**에게 있다.  
```java
class Main{
    private Sample sample;
    public Main(Sample sample){
        this.sample = sample;
    }
}

class MainTest{
    Sample sample = new Sample();
    Main main = new Main(sample);
}
```
위 코드의 **sample**에 대한 제어권은 **Main**이 아니라 **MainTest**에게 존재한다. 즉, 해당 코드처럼 의존성을 **역전**시켜 객체에 대한 제어권을 직접 **가지지 않는 것**을 **의존성 역전(IoC, Inversion of Control)**이라 하며 의존성을 **외부**에서 주입시키는 것을 **의존성 주입(DI, Dependency Injection)**이라 한다.  
---
  
[참고]  
[leveloper](https://leveloper.tistory.com/33)  
[심플하게 개발](https://limmmee.tistory.com/13)  