---
layout: post
title:  "[Android] MVVM 패턴과 ACC"
date:   2022-03-07 17:24:00+0900
categories: jekyll update
tags: [Android]
---
<p align="center"><img src="/assets/img/blog/정보/안드로이드.png"></p>

# MVVM 패턴?
`Android` 개발을 해보셨던 분들이라면 개발 초기에 `Activity`에 이벤트, 핸들러, 네트워킹 등 모든 동작 코드가 마구잡이로 섞여있어 이른바 `스파게티` 코드가 되어 추후 유지보수하거나 기능을 추가할 때 정신이 저 멀리 안드로메다까지 간 경험이 있을 것이다. 이러한 문제를 해결하기 위해 **`MVVM`이라는 `MVC`패턴과 흡사한 패턴을 알아보자.**

# ACC란?
**ACC는 Android Architecture Component의 약자로 `Android Jetpack`의 구성요소이다!** 즉, MVVM 패턴을 쉽게 사용할 수 있도록 도와주는 라이브러리 도구 모음집이다.  

# MVC와 MVVM?
기본적으로 `MVC`는 서비스 개발에 많이 편리했지만 규모가 커지면 커질수록 `Activity`자체가 무거워지고 `View`와 `Model`의 의존성이 높아진다. 또한 `Controller`가 당연히 **살려달라고** 비명을 지르는 상황이 발생한다. 이러한 단점을 보완한 것이 `MVVM` 패턴이며 기존 `Controller`의 역할을 분리시켜 좀 더 체계적으로 만들어주는 디자인 패턴이다.  
**MVVM은 Model, View, ViewModel로 이루어져 있다.**  

# MVVM (Mode, View, ViewModel)
<p align="center"><img src="/assets/img/blog/정보/MVVM.png"></p>

## View
- `Activity`와 `Fragment`가 `View`의 역할을 수행한다.  
- 사용자의 경험(Action)을 받는다. (텍스트 입력, 버튼 클릭 등)  
- ViewModel의 데이터를 관찰(Observing)하여 **UI**를 갱신한다.  

## ViewModel
- `View`가 요청한 데이터를 `Model`로 요청한다.  
- `Model`로부터 **요청한 데이터를 받는다.**  

## Model
- `ViewModel`이 요청한 **데이터를 반환한다.**  
- 주로 Room, Realm과 같은 DB나 `Retrofit`을 통한 **백엔드 API 호출(네트워킹)이 기본적이다.  
  
위와 같은 특징 때문에 `View`는 `DB`에 직접 접근을 하지 않고 **UI** 업데이트만 관찰한다. 즉, `View`가 원하는 데이터는 `ViewModel`이 가지고 있기에 `View`는 `ViewModel`의 데이터를 관찰(Observing)한다.  

## MVVM의 장점
1. View가 ViewModel의 데이터를 관찰하므로 UI 업데이트가 간편하다.  
2. ViewModel이 데이터를 관리하므로 `Memory Leak` 발생 가능성을 배제한다. (이는 View가 Model에 접근하지 않아 `Activity`나 `Fragment` 생명주기에 의존하지 않기 때문이다.)  
3. 기능별로 **모듈화가 잘 되어 유지 보수에 용이하다.**

# ACC (Android Architecture Component)
<p align="center"><img src="/assets/img/blog/정보/ACC.png"></p>
  
## ViewModel
- 화면 변화에도 변하지 않고 사라지지 않는 데이터를 가진다.  

## Live DAta
- View가 ViewModel을 관찰할 때, 그 관찰 대상이 되는 **데이터 홀드 클래스**이다. *`Live Data`는 `Activity`와 `Fragment`의 생명주기를 인지하지 못하므로 화면이 **활성화**되어 있을 때만 동작한다.*  

## Repository
- `ViewModel`과 데이터를 주고받기 위해 `데이터 API`를 포함하는 클래스이다. **사용자 동작에 따라 필요한 데이터나 외부 백엔드 서버 등 데이터를 가져온다.** 이 `Repository` 덕분에 `ViewModel`이 데이터를 관리할 필요가 없다.  

## RoomDatabase
- `Room`은 `SQLite`를 사용함에 있어 쿼리문 작성 없이 간편하게 `Insert나 Delete` 등의 쿼리를 날리게 도와주는 `ORM` 라이브러리이다. (Realm도 가능!)
  
  
  
  
  
---  
[참고 및 출처]  
[H34RO](https://velog.io/@haero_kim/Android-%EA%B9%94%EC%8C%88%ED%95%98%EA%B2%8C-MVVM-%ED%8C%A8%ED%84%B4%EA%B3%BC-AAC-%EC%95%8C%EC%95%84%EB%B3%B4%EA%B8%B0)  
[하루플스토리](https://haruple.tistory.com/212)  