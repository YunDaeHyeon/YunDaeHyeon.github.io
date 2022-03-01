---
layout: post
title:  "[Programmers, SQL 고득점 Kit] SELECT - 모든 레코드 조회하기"
date:   2022-03-01 15:01:00+0900
categories: jekyll update
tags: [programmers]
---

<p align="center"><img src="/assets/img/blog/정보/프로그래머스.png"></p>

# 프로그래머스 SQL 고득점 SELECT LEVEL 1 - 모든 레코드 조회하기
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
  
동물 보호소에 들어온 모든 동물의 정보를 ANIMAL_ID순으로 조회하는 SQL문을 작성해주세요. SQL을 실행하면 다음과 같이 출력되어야 합니다.  
  
```sql
ANIMAL_ID   ANIMAL_TYPE DATETIME                INTAKE_CONDITION    NAME    SEX_UPON_INTAKE
A349996     Cat         2018-01-22 14:32:00	    Normal            Sugar	Neutered Male
A35027      Cat         2017-08-13 13:50:00	    Normal            Jewel	Spayed Female
A350375     Cat         2017-03-06 15:01:00	    Normal            Meo     Neutered Male
A352555     Dog         2014-08-08 04:20:00	    Normal            Harley	Spayed Female
```

# 정답
```sql
SELECT * from ANIMALS_INS Order By ANIMAL_ID
```

# 설명
`SELECT * from`에서 `*`으로 모든 필드를 대상으로 `ANIMALS_INS` 테이블에서 조회를 한다.  
= 선택(select)한다. 모든 필드를(*) 어디에서(from) ANIMALS_INS 테이블에서
어떻게(Order By, 가져온 필드의 데이터를 정렬) ANIMAL_ID 순으로.  

# ORDER BY절
SELECT문을 사용할 때 오름차순 혹은 내림차순으로 데이터를 정렬하기 위해 사용하는 구절이다.  
**ORDER BY**는 항상 SELECT문의 맨 마지막에 위치한다.  

# 기본 구조
```sql
SELECT * from {table_name} ORDER BY {column_name} (ASC, DESC)
```
ASC : 기본값이며 오름차순이다.  
DESC : 내림차순이다.
  
  
  
---
[문제 출처]  
[프로그래머스](https://programmers.co.kr/)의 SQL 고득점 Kit