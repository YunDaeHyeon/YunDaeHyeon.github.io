---
layout: post
title:  "[Programmers, SQL 고득점 Kit] SELECT - 역순 정렬하기"
date:   2022-03-01 17:00:00+0900
categories: jekyll update
tags: [programmers]
---

<p align="center"><img src="/assets/img/blog/정보/프로그래머스.png"></p>

# 프로그래머스 SQL 고득점 SELECT LEVEL 1 - 역순 정렬하기
## 문제
ANIMAL_INS 테이블은 동물 보호소에 들어온 동물의 정보를 담은 테이블입니다. ANIMAL_INS 테이블 구조는 다음과 같으며, ANIMAL_ID, ANIMAL_TYPE, DATETIME, INTAKE_CONDITION, NAME, SEX_UPON_INTAKE는 각각 동물의 아이디, 생물 종, 보호 시작일, 보호 시작 시 상태, 이름, 성별 및 중성화 여부를 나타냅니다.  

```sql
# ANIMAL_INS 테이블
NAME                TYPE        NULLABLE
ANIMAL_ID           VARCHAR(N)	FALSE
ANIMAL_TYPE         VARCHAR(N)	FALSE
DATETIME            DATETIME	FALSE
INTAKE_CONDITION    VARCHAR(N)	FALSE
NAME                VARCHAR(N)	TRUE
SEX_UPON_INTAKE     VARCHAR(N)	FALSE
```
  
---
  
동물 보호소에 들어온 모든 동물의 이름과 보호 시작일을 조회하는 SQL문을 작성해주세요. 이때 결과는 ANIMAL_ID 역순으로 보여주세요. SQL을 실행하면 다음과 같이 출력되어야 합니다.  
  
```sql
NAME	DATETIME
Rocky	2016-06-07 09:17:00
Shelly	2015-01-29 15:01:00
Benji	2016-04-19 13:28:00
Jackie	2016-01-03 16:25:00
*Sam	2016-03-13 11:17:00
```

# 정답
```sql
SELECT NAME, DATETIME from ANIMAL_INS order by ANIMAL_ID DESC
```

# 설명
선택한다(select) 누구를 대상으로(NAME, DATETIME), 어디에서(from), `ANIMAL_INS`테이블에서 정렬(order by)한다. 누구를 대상으로 `ANIMAL_ID`를 대상으로 내림차순(DESC)시킨다.  
  
  
---
[문제 출처]  
[프로그래머스](https://programmers.co.kr/)의 SQL 고득점 Kit  