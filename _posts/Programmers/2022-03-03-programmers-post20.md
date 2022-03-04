---
layout: post
title:  "[Programmers, SQL 고득점 Kit] JOIN - 있었는데요 없었습니다"
date:   2022-03-03 19:24:00+0900
categories: jekyll update
tags: [programmers level 3]
---

<p align="center"><img src="/assets/img/blog/정보/프로그래머스.png"></p>

# 프로그래머스 SQL 고득점 Kit
## JOIN LEVEL 3 - 있었는데요 없었습니다

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
관리자의 실수로 일부 동물의 입양일이 잘못 입력되었습니다. 보호 시작일보다 입양일이 더 빠른 동물의 아이디와 이름을 조회하는 SQL문을 작성해주세요. 이때 결과는 보호 시작일이 빠른 순으로 조회해야합니다.    
예를 들어, ANIMAL_INS 테이블과 ANIMAL_OUTS 테이블이 다음과 같다면  
```sql
# ANIMAL_INS
ANIMAL_ID	ANIMAL_TYPE	DATETIME	        INTAKE_CONDITION	NAME	    SEX_UPON_INTAKE
A350276	        Cat	        2017-08-13 13:50:00	Normal	                Jewel	    Spayed Female
A381217	        Dog	        2017-07-08 09:41:00	Sick	                Cherokee	Neutered Male
```
```sql
# ANIMAL_OUTS
ANIMAL_ID	ANIMAL_TYPE	DATETIME	        NAME	        SEX_UPON_OUTCOME
A350276	        Cat	        2018-01-28 17:51:00	Jewel	        Spayed Female
A381217	        Dog	        2017-06-09 18:51:00	Cherokee	Neutered Male
```
SQL문을 실행하면 다음과 같이 나와야 합니다.  

```sql
ANIMAL_ID	NAME
A381217	    Cherokee
```

# 정답
```sql
SELECT
ANIMAL_OUTS.ANIMAL_ID,
ANIMAL_OUTS.NAME
FROM ANIMAL_INS INNER JOIN ANIMAL_OUTS
ON ANIMAL_INS.ANIMAL_ID = ANIMAL_OUTS.ANIMAL_ID
WHERE ANIMAL_OUTS.DATETIME < ANIMAL_INS.DATETIME
ORDER BY ANIMAL_INS.DATETIME
```
  
# 설명
문제에서 요구하는 것은 다음과 같다.  
1. **보호 시작일보다 입양일이 더 빠른 데이터 조회**  
2. **결과는 보호 시작일이 더 빠른 순으로 조회**  
즉, 관리자의 실수로 인하여 보호 시작일보다 **입양일이 더 빠른** 데이터들을 조회시키면 된다.  
보호시작일에 해당하는 테이블 `ANIMAL_INS`과 입양일에 해당하는 테이블 `ANIMAL_OUTS` 테이블을 조인시키자.  
이때, 보호 시작일보다 입양일이 더 빠르다는 것은 **보호 시작과 동시에 입양을 간 데이터가 모두 있어야 한다는 의미이다.** 즉 `ANIMAL_INS`에 있는 데이터는 `ANIMAL_OUTS`에도 있어야 한다는 의미이다. 따라서 두 개의 테이블에서 **공통으로 존재하는 데이터만** 출력시키는 `INNER JOIN`을 시키면 된다.  
```sql
FROM ANIMAL_INS INNER JOIN ANIMAL_OUTS
ON ANIMAL_INS.ANIMAL_ID = ANIMAL_OUTS.ANIMAL_ID
```
문제에서 요구하는 것은 **보호 시작일보다 입양일이 더 빨라야(커야) 한다.** 만약 보호 시작일이 `2022년 03월 03일`이라면 입양일은 `2022년 01월 24`이어야 한다. **(입양일 < 보호시작일)**  
```sql
WHERE ANIMAL_OUTS.DATETIME < ANIMAL_INS.DATETIME
```
이대로 제출하면 **틀렸다고 나온다.**  
**결과는 보호 시작일이 빠른 순**으로 조회해야 한다. 보호시작일을 오름차순 정렬 시키면 정답이다.
```sql
ORDER BY ANIMAL_INS.DATETIME
```

---
[문제 출처]  
[프로그래머스](https://programmers.co.kr/)의 SQL 고득점 Kit  