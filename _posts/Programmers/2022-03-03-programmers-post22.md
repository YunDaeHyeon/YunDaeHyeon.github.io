---
layout: post
title:  "[Programmers, SQL 고득점 Kit] JOIN - 오랜 기간 보호한 동물(2)"
date:   2022-03-03 19:34:00+0900
categories: jekyll update
tags: [programmers level 4]
---

<p align="center"><img src="/assets/img/blog/정보/프로그래머스.png"></p>

# 프로그래머스 SQL 고득점 Kit
## JOIN LEVEL 4 - 오랜 기간 보호한 동물(2)

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
보호소에서 중성화 수술을 거친 동물 정보를 알아보려 합니다. 보호소에 들어올 당시에는 중성화(중성화를 거치지 않은 동물은 `성별 및 중성화 여부`에 `Intact`, 중성화를 거친 동물은 `Spayed` 또는 `Neutered`라고 표시되어 있습니다.)되지 않았지만, 보호소를 나갈 당시에는 중성화된 동물의 아이디와 생물 종, 이름을 조회하는 아이디 순으로 조회하는 SQL 문을 작성해주세요.  
예를 들어, ANIMAL_INS 테이블과 ANIMAL_OUTS 테이블이 다음과 같다면  
```sql
# ANIMAL_INS
ANIMAL_ID	 ANIMAL_TYPE	    DATETIME	            INTAKE_CONDITION	        NAME	    SEX_UPON_INTAKE
A367438	        Dog	            2015-09-10 16:01:00	    Normal	               Cookie	    Spayed Female
A382192	        Dog	            2015-03-13 13:14:00	    Normal	               Maxwell 2    Intact Male
A405494	        Dog	            2014-05-16 14:17:00	    Normal	               Kaila	    Spayed Female
A410330	        Dog	            2016-09-11 14:09:00	    Sick	               Chewy	    Intact Female
```
```sql
# ANIMAL_OUTS
ANIMAL_ID	 ANIMAL_TYPE	    DATETIME	            NAME	        SEX_UPON_OUTCOME
A367438	        Dog	            2015-09-12 13:30:00	    Cookie	        Spayed Female
A382192	        Dog	            2015-03-16 13:46:00	    Maxwell 2	        Neutered Male
A405494	        Dog	            2014-05-20 11:44:00	    Kaila	        Spayed Female
A410330	        Dog	            2016-09-13 13:46:00	    Chewy	        Spayed Female
```
- Cookie는 보호소에 들어올 당시에 이미 중성화되어있었습니다.  
- Maxwell 2는 보호소에 들어온 후 중성화되었습니다.  
- Kaila는 보호소에 들어올 당시에 이미 중성화되어있었습니다.  
- Chewy는 보호소에 들어온 후 중성화되었습니다.  
SQL문을 실행하면 다음과 같이 나와야 합니다.  
```sql
ANIMAL_ID	 ANIMAL_TYPE	    NAME
A382192	        Dog	            Maxwell 2
A410330	        Dog	            Chewy
```

# 정답
```sql
SELECT
ANIMAL_OUTS.ANIMAL_ID,
ANIMAL_OUTS.ANIMAL_TYPE,
ANIMAL_OUTS.NAME
FROM ANIMAL_INS INNER JOIN ANIMAL_OUTS
ON ANIMAL_INS.ANIMAL_ID = ANIMAL_OUTS.ANIMAL_ID
WHERE ANIMAL_INS.SEX_UPON_INTAKE LIKE 'Intact%' AND
(ANIMAL_OUTS.SEX_UPON_OUTCOME LIKE 'Spayed%' OR ANIMAL_OUTS.SEX_UPON_OUTCOME LIKE 'Neutered%')
ORDER BY ANIMAL_OUTS.ANIMAL_ID
```
***난이도가 또 급상승 하였다.. 푸느라 많이 힘들었다..***  

# 설명
문제에서 요구하는 것을 확인하자.  
1. **보호소에 들어올 당시에는 중성화 되지 않았으며 보호소를 나갈 당시에는 중성화가 된** 동물의 데이터 조회  
2. **조회하는 아이디 순으로 조회**  
1번의 경우 보호소에 들어올 당시에는 중성화 되지 않았으며 보호소를 나갈 당시에는 중성화가 되었다는 것은 곧 테이블 `ANIMAL_INS`, `ANIMAL_OUTS`에는 데이터가 존재한다는 의미이다. 두 개의 테이블이 조인된 상태에서 **공통되는 데이터**만 조회할 수 있도록 `INNER JOIN`을 사용한다.  
```sql
FROM ANIMAL_INS INNER JOIN ANIMAL_OUTS
ON ANIMAL_INS.ANIMAL_ID = ANIMAL_OUTS.ANIMAL_ID
```
중성화 되지 않았다는 것을 알기 위한 데이터는 `SEX_UPON_INTAKE`필드에 `Intact`라 표시되어 있고 중성화가 되었다는 것을 알기 위한 데이터는 `SEX_UPON_OUTCOME` 필드에 `Spayed` 혹은 `Neutered`으로 표시되어 있다. **보호소에 들어올 당시에는 중성화 되지 않았으며 보호소를 나갈 당시에는 중성화가 된** 데이터를 조회한다는 것은 곧 `ANIMAL_INS` 테이블의 `SEX_UPON_INTAKE` 필드에 존재하는 데이터가 `Intact`으로 시작하며 동시에 `ANIMAL_OUTS` 테이블의 `SEX_UPON_OUTCOME` 필드에 존재하는 데이터가 `Spayed`로 시작하거나 `Neutered`로 시작한다는 의미이다. 이를 위하여 **조건문과 LIKE 절을 사용하여 `Intact`, `Spayed`, `Neutered`로 시작하는 데이터가 존재할 경우만 조회시킨다.**  

```sql
WHERE ANIMAL_INS.SEX_UPON_INTAKE LIKE 'Intact%' AND
(ANIMAL_OUTS.SEX_UPON_OUTCOME LIKE 'Spayed%' OR ANIMAL_OUTS.SEX_UPON_OUTCOME LIKE 'Neutered%')
```
여기서 제출하면 당연히 오답이 되고 **조회하는 아이디 순으로 조회**시켜야 하기 때문에 **ANIMAL_ID**를 오름차순 정렬시킨 뒤 제출하면 정답이다.  
```sql
ORDER BY ANIMAL_OUTS.ANIMAL_ID
```
---
[문제 출처]  
[프로그래머스](https://programmers.co.kr/)의 SQL 고득점 Kit  