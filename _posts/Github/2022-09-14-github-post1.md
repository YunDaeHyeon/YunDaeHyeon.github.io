---
layout: post
title:  "[Git] 이미 push된 파일 .gitignore 적용하는 방법"
date:   2022-09-14 0:25:00+0900
categories: jekyll update
tags: [Git]
---
<p align="center"><img src="/assets/img/blog/정보/github.png"></p>

# 뒤늦게 .gitignore에 추가할 파일이 생겼다면?
본래 `git`으로 관리하고 싶지 않은 파일은 `.gitignore` 파일에 명시하여 관리를 제외시킨다.  
하지만 종종 `.gitignore`에 올리기도 전에 `push`를 해버리는 경우가 존재한다.  
이때, **`.gitignore`을 수정해서 `push`에도 이미 그 파일은 git에 의해 관리가 되고있다.**  

# 해결밭법
`git rm -r --cached .`명령을 실행하여 캐시를 지운 뒤 `add`, `commit`, `push`를 진행하면 끝!  

```console
$ git rm -r --cached .
$ git add .
$ git commit -m "올리고 싶은 메세지 ex) Apply .gitignore"
$ git push
```