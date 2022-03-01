---
layout: post
title:  "[Programmers, SQL 고득점 Kit] SUM, MAX, MIN - 최댓값 구하기"
date:   2022-03-01 18:14:00+0900
categories: jekyll update
tags: [programmers level 1]
---

<p align="center"><img src="/assets/img/blog/정보/프로그래머스.png"></p>

# 프로그래머스 SQL 고득점 SUM, MAX, MIN LEVEL 1 - 최댓값 구하기
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
  
가장 최근에 들어온 동물은 언제 들어왔는지 조회하는 SQL 문을 작성해주세요.  
예를 들어 `ANIMAL_INS` 테이블이 다음과 같다면  

```sql
ANIMAL_ID	ANIMAL_TYPE	DATETIME	        INTAKE_CONDITION	NAME	        SEX_UPON_INTAKE
A399552	        Dog	        2013-10-14 15:38:00	Normal	                Jack	        Neutered Male
A379998	        Dog	        2013-10-23 11:42:00	Normal	                Disciple	Intact Male
A370852	        Dog	        2013-11-03 15:04:00	Normal	                Katie	        Spayed Female
A403564	        Dog	        2013-11-18 17:03:00	Normal	                Anna	        Spayed Female
```

가장 늦게 들어온 동물은 Anna이고, Anna는 2013-11-18 17:03:00에 들어왔습니다.  따라서 SQL문을 실행하면 다음과 같이 나와야 합니다.   

```sql
# 컬럼 이름은 상관 없다.
시간
2013-11-18 17:03:00
```


# 정답
```sql
SELECT MAX(DATETIME) from ANIMAL_INS
```

# 설명
**MAX 함수**를 이용하여 `DATETIME` 필드에 있는 값들 중 가장 큰 값을 반환한다.  

---
[문제 출처]  
[프로그래머스](https://programmers.co.kr/)의 SQL 고득점 Kit  