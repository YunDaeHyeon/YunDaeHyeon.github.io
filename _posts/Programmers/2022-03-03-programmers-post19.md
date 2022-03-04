---
layout: post
title:  "[Programmers, SQL 고득점 Kit] JOIN - 없어진 기록 찾기"
date:   2022-03-03 19:16:00+0900
categories: jekyll update
tags: [programmers level 3]
---

<p align="center"><img src="/assets/img/blog/정보/프로그래머스.png"></p>

# 프로그래머스 SQL 고득점 Kit
## JOIN LEVEL 3 - 없어진 기록 찾기

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
천재지변으로 인해 일부 데이터가 유실되었습니다. 입양을 간 기록은 있는데, 보호소에 들어온 기록이 없는 동물의 ID와 이름을 ID 순으로 조회하는 SQL문을 작성해주세요.  
예를 들어, ANIMAL_INS 테이블과 ANIMAL_OUTS 테이블이 다음과 같다면  
```sql
# ANIMAL_INS
ANIMAL_ID	ANIMAL_TYPE	DATETIME	        INTAKE_CONDITION	NAME	SEX_UPON_INTAKE
A352713	        Cat	        2017-04-13 16:29:00	Normal	                Gia	 Spayed Female
A350375	        Cat	        2017-03-06 15:01:00	Normal	                Meo	 Neutered Male
```
```sql
# ANIMAL_OUTS
ANIMAL_ID	ANIMAL_TYPE	DATETIME	        NAME	SEX_UPON_OUTCOME
A349733	        Dog	        2017-09-27 19:09:00	Allie	Spayed Female
A352713	        Cat	        2017-04-25 12:25:00	Gia	Spayed Female
A349990 	Cat	        2018-02-02 14:18:00	Spice	Spayed Female
```
**ANIMAL_OUTS 테이블에서**  
- Allie의 ID는 ANIMAL_INS에 없으므로, Allie의 데이터는 유실되었습니다.  
- Gia의 ID는 ANIMAL_INS에 있으므로, Gia의 데이터는 유실되지 않았습니다.  
- Spice의 ID는 ANIMAL_INS에 없으므로, Spice의 데이터는 유실되었습니다.  
따라서 SQL문을 실행하면 다음과 같이 나와야 합니다.  

```sql
ANIMAL_ID	NAME
A349733	        Allie
A349990 	Spice
```

# 정답
```sql
SELECT
ANIMAL_OUTS.ANIMAL_ID,
ANIMAL_OUTS.NAME
FROM ANIMAL_INS RIGHT OUTER JOIN ANIMAL_OUTS
ON ANIMAL_OUTS.ANIMAL_ID = ANIMAL_INS.ANIMAL_ID
WHERE ANIMAL_INS.ANIMAL_ID IS NULL
ORDER BY ANIMAL_OUTS.ANIMAL_ID
```
  
# 설명
문제에서 요구하는 것들을 확인하자.  
1. **입양을 간 기록은 있는데 보호소에 들어온 기록이 없는 동물들**  
2. **1번의 조건을 만족하는 동물의 아이디와 이름을 조회**  
문제를 잘 읽어보면 **보호소에 들어온 시점을 기록한 테이블은 `ANIMAL_INS`이며 입양을 보낸 시점을 기록한 테이블은 `ANIMAL_OUTS`이다.** 입양을 간 기록은 있는데 **보호소에 들어온 기록이 없다는 것**은 `ANIMAL_OUTS`에는 데이터가 존재하지만 `ANIMAL_INS`에는 **데이터가 없다는**의미이다. 따라서 `RIGHT JOIN`을 사용하여 `ANIMAL_OUTS`에 있는 데이터만 조회를 시키며 `ANIMAL_INS`와 공통되는 데이터가 없어야 하기에 `ANIMAL_INS`데이터가 NULL인 데이터만 조회되도록 시키면 된다.  

```sql
FROM ANIMAL_INS RIGHT OUTER JOIN ANIMAL_OUTS
ON ANIMAL_OUTS.ANIMAL_ID = ANIMAL_INS.ANIMAL_ID
```
위 쿼리를 통하여 `ANIMAL_INS`테이블과 `ANIMAL_OUTS` 테이블을 조인시켜
```sql
WHERE ANIMAL_INS.ANIMAL_ID IS NULL
```
조건문을 사용하여 `ANIMAL_INS`에 데이터가 존재하지 않고 `ANIMAL_OUTS`에만 존재하는 데이터를 조회한다.  

---
[문제 출처]  
[프로그래머스](https://programmers.co.kr/)의 SQL 고득점 Kit  