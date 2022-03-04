---
layout: post
title:  "[Programmers, SQL 고득점 Kit] String, Date - 오랜 기간 보호한 동물(2)"
date:   2022-03-04 17:24:00+0900
categories: jekyll update
tags: [programmers level 3]
---

<p align="center"><img src="/assets/img/blog/정보/프로그래머스.png"></p>

# 프로그래머스 SQL 고득점 Kit
## String, Date LEVEL 3 - 오랜 기간 보호한 동물(2)

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
입양을 간 동물 중, 보호 기간이 가장 길었던 동물 두 마리의 아이디와 이름을 조회하는 SQL문을 작성해주세요. 이때 결과는 보호 기간이 긴 순으로 조회해야 합니다.  
예를 들어, ANIMAL_INS 테이블과 ANIMAL_OUTS 테이블이 다음과 같다면  
```sql
# TABLE : ANIMAL_INS
ANIMAL_ID	ANIMAL_TYPE	DATETIME	        INTAKE_CONDITION	NAME	    SEX_UPON_INTAKE
A354597	        Cat	        2014-05-02 12:16:00	Normal	                Ariel	    Spayed Female
A362707	        Dog	        2016-01-27 12:27:00	Sick	                Girly Girl	Spayed Female
A370507	        Cat	        2014-10-27 14:43:00	Normal	                Emily	    Spayed Female
A414513	        Dog	        2016-06-07 09:17:00	Normal	                Rocky	    Neutered Male
```
```sql
# TABLE : ANIMAL_OUTS
ANIMAL_ID	ANIMAL_TYPE	DATETIME	            NAME	        SEX_UPON_OUTCOME
A354597	        Cat	        2014-06-03 12:30:00	    Ariel	        Spayed Female
A362707	        Dog	        2017-01-10 10:44:00	    Girly Girl	        Spayed Female
A370507	        Cat	        2015-08-15 09:24:00	    Emily	        Spayed Female
```
따라서 SQL문을 실행하면 다음과 같이 나와야 합니다.
```sql
ANIMAL_ID	NAME
A362707	        Girly Girl
A370507	        Emily
```
※ 입양을 간 동물이 2마리 이상인 경우만 입력으로 주어집니다.
  
# 정답
```sql
SELECT
ANIMAL_OUTS.ANIMAL_ID,
ANIMAL_OUTS.NAME
FROM ANIMAL_INS RIGHT OUTER JOIN ANIMAL_OUTS
ON ANIMAL_OUTS.ANIMAL_ID = ANIMAL_INS.ANIMAL_ID
ORDER BY DATEDIFF(ANIMAL_OUTS.DATETIME, ANIMAL_INS.DATETIME) DESC LIMIT 2
```
  
# 설명
문제에서 요구하는 것들 확인하자.  
1. **입양을 간 동물 중, 보호기간이 가장 길었던 동물 `2마리` 조회**  
2. 결과는 **보호기간이 긴** 순으로 조회  
입양을 갔다는 것은 `ANIMAL_INS` 테이블과 `ANIMAL_OUTS`테이블이 서로 공통되는 데이터와 `ANIMAL_OUTS`테이블에만 데이터가 존재해야 하기에 `RIGHT JOIN`을 이용하여 두 개의 테이블을 조인한다.  
  
```sql
FROM ANIMAL_INS RIGHT OUTER JOIN ANIMAL_OUTS
ON ANIMAL_OUTS.ANIMAL_ID = ANIMAL_INS.ANIMAL_ID
```
또한 **보호기간이 가장 길었던 동물**을 알아내기 위해서는 **입양을 보낸 날짜에서 보호 시작일을 빼면 된다.**  하지만, 두 레코드 모두 `날짜형식`이기에 `DATEDIFF`함수를 이용하여 두 날짜의 차를 구한다. 두 날짜의 차가 보호한 기간이다.  
```sql
ORDER BY DATEDIFF(ANIMAL_OUTS.DATETIME, ANIMAL_INS.DATETIME)
```
이때, **보호기간이 긴**순으로 조회를 해야하며 2마리만 조회해야하기 때문에 `DATEDIFF`함수로 조회된 결과를 **내림차순**시키면 최상위 데이터들이 보호기간이 가장 **긴** 데이터 들이다. `LIMIT` 절을 사용하여 최상위 데이터 2개를 출력시키면 정답이다.  
```sql
ORDER BY DATEDIFF(ANIMAL_OUTS.DATETIME, ANIMAL_INS.DATETIME) DESC LIMIT 2
```
---
[문제 출처]  
[프로그래머스](https://programmers.co.kr/)의 SQL 고득점 Kit  