---
layout: post
title:  "[Programmers, SQL 고득점 Kit] SELECT - 어린 동물 찾기"
date:   2022-03-01 17:25:00+0900
categories: jekyll update
tags: [programmers level 1]
---

<p align="center"><img src="/assets/img/blog/정보/프로그래머스.png"></p>

# 프로그래머스 SQL 고득점 SELECT LEVEL 1 - 어린 동물 찾기
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
  
동물 보호소에 들어온 동물 중 젊은 동물(`INTAKE_CONDITION`이 Aged가 아니면)의 아이디와 이름을 조회하는 SQL 문을 작성해주세요. 이때 결과는 아이디 순으로 조회해주세요. 예를 들어 `ANIMAL_INS` 테이블이 다음과 같다면    

```sql
ANIMAL_ID	ANIMAL_TYPE	DATETIME	        INTAKE_CONDITION	    NAME	    SEX_UPON_INTAKE
A365172         Dog	        2014-08-26 12:53:00	Normal	                Diablo	    Neutered Male
A367012 	Dog     	2015-09-16 09:06:00	Sick	                Miller	    Neutered Male
A365302 	Dog	        2017-01-08 16:34:00	Aged	                Minnie	    Spayed Female
A381217 	Dog	        2017-07-08 09:41:00	Sick	                Cherokee	Neutered Male
```

이 중 젊은 동물은 Diablo, Miller, Cherokee입니다. 따라서 SQL문을 실행하면 다음과 같이 나와야 합니다.    
  
```sql
ANIMAL_ID	NAME
A365172	        Diablo
A367012	        Miller
A381217	        Cherokee
```

# 정답
```sql
SELECT ANIMAL_ID, NAME from ANIMAL_INS WHERE NOT INTAKE_CONDITION = 'Aged' Order by ANIMAL_ID
```

# 설명
`INTAKE_CONDITION`이 `Aged`가 아닌 데이터만 조회하기에 조건(WHERE)에 거짓(NOT)을 사용하여 **`INTAKE_CONDITION`가 `Aged`가 아닌 것들만(WHERE NOT INTAKE_CONDITION = 'Aged')** 아이디 순으로 정렬해서 출력해라.(Order by ANIMAL_ID)

---
[문제 출처]  
[프로그래머스](https://programmers.co.kr/)의 SQL 고득점 Kit  