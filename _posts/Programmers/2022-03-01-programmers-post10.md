---
layout: post
title:  "[Programmers, SQL 고득점 Kit] SUM, MAX, MIN - 동물 수 구하기"
date:   2022-03-01 19:51:00+0900
categories: jekyll update
tags: [programmers]
---

<p align="center"><img src="/assets/img/blog/정보/프로그래머스.png"></p>

# 프로그래머스 SQL 고득점 SUM, MAX, MIN LEVEL 2 - 동물 수 구하기
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
  
동물 보호소에 동물이 몇 마리 들어왔는지 조회하는 SQL 문을 작성해주세요.  
예를 들어 `ANIMAL_INS` 테이블이 다음과 같다면  

```sql
ANIMAL_ID	ANIMAL_TYPE	DATETIME	        INTAKE_CONDITION	NAME	        SEX_UPON_INTAKE
A399552	        Dog	        2013-10-14 15:38:00	Normal	                Jack	        Neutered Male
A379998	        Dog	        2013-10-23 11:42:00	Normal	                Disciple	Intact Male
A370852	        Dog	        2013-11-03 15:04:00	Normal	                Katie	        Spayed Female
A403564	        Dog	        2013-11-18 17:03:00	Normal	                Anna	        Spayed Female
```

동물 보호소에 들어온 동물은 4마리입니다. 따라서 SQL문을 실행하면 다음과 같이 나와야 합니다. 따라서 SQL문을 실행하면 다음과 같이 나와야 합니다.   

```sql
# 컬럼의 이름은 일치하지 않아도 괜찮다.
count
4
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