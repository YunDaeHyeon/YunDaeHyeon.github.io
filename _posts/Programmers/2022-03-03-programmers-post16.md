---
layout: post
title:  "[Programmers, SQL 고득점 Kit] IS NULL - 이름이 없는 동물의 아이디"
date:   2022-03-03 11:33:00+0900
categories: jekyll update
tags: [programmers level 1]
---

<p align="center"><img src="/assets/img/blog/정보/프로그래머스.png"></p>

# 프로그래머스 SQL 고득점 Kit
## IS NUL LEVEL 1 - 이름이 없는 동물의 아이디

# 문제
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
  
동물 보호소에 들어온 동물 중, 이름이 없는 채로 들어온 동물의 ID를 조회하는 SQL 문을 작성해주세요. 단, ID는 오름차순 정렬되어야 합니다.

예를 들어 ANIMAL_INS 테이블이 다음과 같다면
```sql
ANIMAL_ID	ANIMAL_TYPE	DATETIME	        INTAKE_CONDITION	NAME	SEX_UPON_INTAKE
A368930	    Dog	        2014-06-08 13:20:00	Normal	            NULL	Spayed Female
A524634	    Dog	        2015-01-02 18:54:00	Normal	            *Belle	Intact Female
A465637	    Dog	        2017-06-04 08:17:00	Injured	            *Commander	Neutered Male
```
이름이 없는 채로 들어온 동물의 ID는 A368930입니다. 따라서 SQL을 실행하면 다음과 같이 출력되어야 합니다.
```sql
ANIMAL_ID
A368930
```
# 정답
```sql
SELECT ANIMAL_ID FROM ANIMAL_INS WHERE NAME IS NULL ORDER BY ANIMAL_ID
```
  
# 설명
조건 `WHERE`에 NULL값에 해당하는 데이터를 출력시키는 `IS NULL`을 사용하여 문제를 푼다.
  

---
[문제 출처]  
[프로그래머스](https://programmers.co.kr/)의 SQL 고득점 Kit  