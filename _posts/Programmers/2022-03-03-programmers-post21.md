---
layout: post
title:  "[Programmers, SQL 고득점 Kit] JOIN - 오랜 기간 보호한 동물(1)"
date:   2022-03-03 19:29:00+0900
categories: jekyll update
tags: [programmers level 3]
---

<p align="center"><img src="/assets/img/blog/정보/프로그래머스.png"></p>

# 프로그래머스 SQL 고득점 Kit
## JOIN LEVEL 3 - 오랜 기간 보호한 동물(1)

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
ANIMAL_OUTS 테이블은 동물 보호소에서 입양 보낸 동물의 정보를 담은 테이블입니다. ANIMAL_OUTS 테이블 구조는 다음과 같으며, ANIMAL_ID, ANIMAL_TYPE, DATETIME, NAME, SEX_UPON_OUTCOME는 각각 동물의 아이디, 생물 종, 입양일, 이름, 성별 및 중성화 여부를 나타냅니다. ANIMAL_OUTS 테이블의 ANIMAL_ID는 ANIMAL_INS의 ANIMAL_ID의 외래 키입니다.  

```sql
NAME	                TYPE	    NULLABLE
ANIMAL_ID	        VARCHAR(N)	FALSE
ANIMAL_TYPE	        VARCHAR(N)	FALSE
DATETIME	        DATETIME	FALSE
NAME	                VARCHAR(N)	TRUE
SEX_UPON_OUTCOME	VARCHAR(N)	FALSE
```
아직 입양을 못 간 동물 중, 가장 오래 보호소에 있었던 동물 3마리의 이름과 보호 시작일을 조회하는 SQL문을 작성해주세요. 이때 결과는 보호 시작일 순으로 조회해야 합니다.  
예를 들어, ANIMAL_INS 테이블과 ANIMAL_OUTS 테이블이 다음과 같다면  
```sql
# ANIMAL_INS
ANIMAL_ID	ANIMAL_TYPE	    DATETIME	        INTAKE_CONDITION	NAME	  SEX_UPON_INTAKE
A354597	        Cat	            2014-05-02 12:16:00	Normal	                Ariel	    Spayed Female
A373687	        Dog	            2014-03-20 12:31:00	Normal	                Rosie	    Spayed Female
A412697	        Dog	            2016-01-03 16:25:00	Normal	                Jackie	    Neutered Male
A413789	        Dog	            2016-04-19 13:28:00	Normal	                Benji	    Spayed Female
A414198	        Dog	            2015-01-29 15:01:00	Normal	                Shelly	    Spayed Female
A368930	        Dog	            2014-06-08 13:20:00	Normal		                    Spayed Female
```
```sql
# ANIMAL_OUTS
ANIMAL_ID	ANIMAL_TYPE	    DATETIME	            NAME	SEX_UPON_OUTCOME
A354597	        Cat	            2014-05-02 12:16:00	    Ariel	Spayed Female
A373687	        Dog	            2014-03-20 12:31:00	    Rosie	Spayed Female
A368930	        Dog	            2014-06-13 15:52:00		        Spayed Female
```
SQL문을 실행하면 다음과 같이 나와야 합니다.  
```sql
NAME	DATETIME
Shelly	2015-01-29 15:01:00
Jackie	2016-01-03 16:25:00
Benji	2016-04-19 13:28:00
```
※ 입양을 가지 못한 동물이 3마리 이상인 경우만 입력으로 주어집니다.
# 정답
```sql
SELECT
ANIMAL_INS.NAME,
ANIMAL_INS.DATETIME
FROM ANIMAL_INS LEFT OUTER JOIN ANIMAL_OUTS
ON ANIMAL_INS.ANIMAL_ID = ANIMAL_OUTS.ANIMAL_ID
WHERE ANIMAL_OUTS.ANIMAL_ID IS NULL
ORDER BY ANIMAL_INS.DATETIME LIMIT 3
```
  
# 설명
문제에서 요구하는 것들을 확인하자.  
1. **입양을 못간 동물 중 가장 오래 보호소에 있던 동물 조회**  
2. **가장 오래 보호소에 있던 동물 3마리의 이름과 보호 시작일 조회**  
3. **결과는 보호 시작일 순으로 조회**
우선, 입양을 가지 못하였다는 것은 입양을 보낸 정보가 담겨있는 `ANIMAL_OUTS`에는 데이터가 존재하지 않는다는 의미이다. 따라서 `ANIMAL_INS`에는 존재하지만 `ANIMAL_OUTS`에는 존재하지 않는 데이터만 조회시키기 위하여 `LEFT JOIN`과 `ANIMAL_OUTS`이 존재하지 않는 데이터가 조회될 경우만 조회될 수 있도록 `ANIMAL_OUTS`가 `NULL`인 경우만 조회시킨다.  

```sql
FROM ANIMAL_INS LEFT OUTER JOIN ANIMAL_OUTS
ON ANIMAL_INS.ANIMAL_ID = ANIMAL_OUTS.ANIMAL_ID
WHERE ANIMAL_OUTS.ANIMAL_ID IS NULL
```
1번 조건은 충족하였지만 **가장 오래 보호소에 있었던 동물**을 조회하기 위하여 조인한 테이블 `ANIMAL_INS`에서 날짜를 **오름차순으로 정렬**시키면 가장 위에 있는 데이터들이 **가장 오래 보호소에 있었던 동물**들이다. 또한 문제에서 **3마리**만 조회시키라는 조건이 있기에 `LIMIT`를 사용하여 3마리만 조회시키면 정답이다.  
  
```sql
ORDER BY ANIMAL_INS.DATETIME LIMIT 3
```
---
[문제 출처]  
[프로그래머스](https://programmers.co.kr/)의 SQL 고득점 Kit  