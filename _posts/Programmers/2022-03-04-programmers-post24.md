---
layout: post
title:  "[Programmers, SQL 고득점 Kit] String, Date - 이름에 el이 들어가는 동물 찾기"
date:   2022-03-04 17:04:00+0900
categories: jekyll update
tags: [programmers level 2]
---

<p align="center"><img src="/assets/img/blog/정보/프로그래머스.png"></p>

# 프로그래머스 SQL 고득점 Kit
## String, Date LEVEL 2 - 이름에 el이 들어가는 동물 찾기

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
보호소에 돌아가신 할머니가 기르던 개를 찾는 사람이 찾아왔습니다. 이 사람이 말하길 할머니가 기르던 개는 이름에 'el'이 들어간다고 합니다. 동물 보호소에 들어온 동물 이름 중, 이름에 "EL"이 들어가는 개의 아이디와 이름을 조회하는 SQL문을 작성해주세요. 이때 결과는 이름 순으로 조회해주세요. 단, 이름의 대소문자는 구분하지 않습니다. 예를 들어 ANIMAL_INS 테이블이 다음과 같다면  
```sql
ANIMAL_ID	ANIMAL_TYPE	    DATETIME	        INTAKE_CONDITION	NAME	SEX_UPON_INTAKE
A373219	        Cat	            2014-07-29 11:43:00	Normal	                Ella	Spayed Female
A377750	        Dog	            2017-10-25 17:17:00	Normal	                Lucy	Spayed Female
A353259	        Dog	            2016-05-08 12:57:00	Injured	                Bj	 Neutered Male
A354540	        Cat	            2014-12-11 11:48:00	Normal	                Tux	 Neutered Male
A354597	        Cat	            2014-05-02 12:16:00	Normal	                Ariel	Spayed Female
```
- 이름에 'el'이 들어가는 동물은 Elijah, Ella, Maxwell 2입니다.  
- 이 중, 개는 Elijah, Maxwell 2입니다.  
따라서 SQL문을 실행하면 다음과 같이 나와야 합니다.
```sql
ANIMAL_ID	NAME
A355753	        Elijah
A382192	        Maxwell 2
```
# 정답
```sql
SELECT ANIMAL_ID, NAME FROM ANIMAL_INS WHERE
NAME LIKE '%el%' AND ANIMAL_TYPE = 'Dog'
ORDER BY NAME
```
  
# 설명
이름에 'el'이 포함되어 있는 경우만 조회시키면 되는 간단한 문제이다. `LIKE` 절을 사용하여 문제를 풀면 쉽게 정답이다. **이때, 개(Dog)의 경우만 조회를 시켜야 한다. 그렇지 않으면 오답이다.**

```sql
NAME LIKE '%el%' AND ANIMAL_TYPE = 'Dog'
ORDER BY NAME
```
---
[문제 출처]  
[프로그래머스](https://programmers.co.kr/)의 SQL 고득점 Kit  