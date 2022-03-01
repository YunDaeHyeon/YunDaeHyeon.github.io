---
layout: post
title:  "[Programmers, SQL 고득점 Kit] SUM, MAX, MIN - 중복 제거하기"
date:   2022-03-01 19:54:00+0900
categories: jekyll update
tags: [programmers]
---

<p align="center"><img src="/assets/img/blog/정보/프로그래머스.png"></p>

# 프로그래머스 SQL 고득점 SUM, MAX, MIN LEVEL 2 - 중복 제거하기
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
  
동물 보호소에 들어온 동물의 이름은 몇 개인지 조회하는 SQL 문을 작성해주세요. 이때 이름이 NULL인 경우는 집계하지 않으며 중복되는 이름은 하나로 칩니다.  
예를 들어 `ANIMAL_INS` 테이블이 다음과 같다면  

```sql
ANIMAL_ID	ANIMAL_TYPE	    DATETIME	        INTAKE_CONDITION	NAME	    SEX_UPON_INTAKE
A562649	        Dog	            2014-03-20 18:06:00	Sick	                NULL	    Spayed Female
A412626	        Dog	            2016-03-13 11:17:00	Normal	                *Sam	    Neutered Male
A563492	        Dog	            2014-10-24 14:45:00	Normal	                *Sam	    Neutered Male
A513956	        Dog	            2017-06-14 11:54:00	Normal	                *Sweetie    Spayed Female
```

보호소에 들어온 동물의 이름은 NULL(없음), *Sam, *Sam, *Sweetie입니다. 이 중 NULL과 중복되는 이름을 고려하면, 보호소에 들어온 동물 이름의 수는 2입니다. 따라서 SQL문을 실행하면 다음과 같이 나와야 합니다.   

```sql
# 컬럼의 이름은 일치하지 않아도 괜찮다.
count
2
```


# 정답
```sql
SELECT COUNT(*) FROM ANIMAL_INS
```

# 설명
COUNT 함수를 이용하여 테이블에 몇 개의 레코드가 있는지 조회한다.  
  
  

---
[문제 출처]  
[프로그래머스](https://programmers.co.kr/)의 SQL 고득점 Kit  