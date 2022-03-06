---
layout: post
title:  "[Android] Retrofit이란?"
date:   2022-03-07 00:35:00+0900
categories: jekyll update
tags: [Android]
---

<p align="center"><img src="/assets/img/blog/정보/레트로핏.png"></p>

안드로이드를 개발하다보면 로그인이나 회원가입 처리.. 게시판을 화면에 뿌려주기 위해 서버에 요청하는 경우가 어마어마하게 많다. 그럴때마다 `AsyncTask`를 이용해서 `HttpUrlConnection`을 이용했었다. 비록 구현하기 힘들고 한번 배우니 정말 애용했다. 토이프로젝트로 안드로이드 어플을 만들고자 다시 개발을 시작했는데.. 세상에 `Deprecated`되었다.. 찾아보니 Android에서 **2019년도에 사망선고를 받았다고 한다.** 지금 생각해보면 오류도 매우 많았다. 프래그먼트(Frangment)에서 AsyncTask를 실행시키고 뒤로만 가면 NPE가 안녕? 하며 튀어나오는 현상이라던가.. 이를 대체하기 위해 Java의 `HttpClient`라이브러리 중 하나인 `Retrofit`에 대해 정리한다.

# Retrofit이란?
**레트로핏(Retrofit)은 안드로이드 애플리케이션에서 통신(Networking) 기능에 사용하는 코드를 사용하기 쉽게 만들어놓은 라이브러리이며 `REST`기반의 웹 서비스를 통해 JSON 구조의 데이터를 쉽게 가져오고 업로드 할 수 있다.**
- `Retrofit`은 네트워크로 전달된 데이터를 어플이 필요한 형태로 받을 수 있다.  
- 기본적으로 `OKHTTP`를 의존한다.

## Square의 Retrofit 소개
레트로핏 개발사 : `Sqaure`  
  
```console
" Retrofit is a REST Client for Java and Android. It makes it relatively easy to retrieve and upload JSON (or other structured data) via a REST based webservice. In Retrofit you configure which converter is used for the data serialization. Typically for JSON you use GSon, but you can add custom converters to process XML or other protocols. Retrofit uses the OkHttp library for HTTP requests. "

" Retrofit은 Java 및 Android 용 REST 클라이언트이다. REST 기반 웹 서비스를 통해 JSON (또는 기타 구조화 된 데이터)을 검색하고 업로드하는 것이 비교적 쉽다. Retrofit에서는 데이터 직렬화에 사용되는 변환기를 구성한다. 일반적으로 JSON의 경우 GSon을 사용하지만 XML 또는 기타 프로토콜을 처리하기 위해 사용자 지정 변환기를 추가 할 수 있다. Retrofit은 HTTP 요청에 OkHttp 라이브러리를 사용한다. "
```
  
## Retrofit을 사용하는 이유
**Retrofit 은 안드로이드 앱에서 필요한 데이터를 서버로부터 가져오고 서버에 데이터를 전송하기 위한 코드를 작성할 때 사용하는 라이브러리이다.**  
***개발자가 서버와 통신하기 위한 코드를 작성하기 편리하게 라이브러리화 되어있다.***  
본래 통신 과정에서는 `HttpClient`나 `AsyncTask`를 이용하여 구현하였다. 하지만 `비동기` 처리 코드를 개발자가 직접적으로 구현해야하는 문제가 있어 모두 `Deprecated`되었다.. 그 대신 `Retrofit`을 사용하는 것이 쉽고 빠르고 편하기 때문에 사용한다.  
   
---
[출처 및 참고]  
[REST란?](https://gmlwjd9405.github.io/2018/09/21/rest-and-restful.html)  
[salix97](https://salix97.tistory.com/204)  
[galid1](https://galid1.tistory.com/m/617)  
[스틱코드](https://stickode.tistory.com/181)  