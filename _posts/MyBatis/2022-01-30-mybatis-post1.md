---
layout: post
title:  "[MyBatis] 문서 루트 요소 \"mapper\"은(는) DOCTYPE 루트 \"null\"과(와) 일치해야 합니다."
date:   2022-01-30 01:30:00+0900
categories: jekyll update
tags: [mybatis, error]
---
# Spring 프로젝트 Run 중 오류
문서 루트 요소 "mapper"은(는) DOCTYPE 루트 "null"과(와) 일치해야 합니다.  

대부분 해당 오류는 Mapper에 
```html
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
```
  
해당 코드가 없어서 발생하는 오류이다.  
*이거 한 줄 빼먹어서 2시간동안 삽질만 하다니 ..*
