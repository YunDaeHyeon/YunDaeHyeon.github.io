---
layout: post
title:  "[Programmers, SQL 고득점 Kit] SELECT - 여러 기준으로 정렬하기"
date:   2022-03-01 17:45:00+0900
categories: jekyll update
tags: [programmers level 1]
---

<p align="center"><img src="/assets/img/blog/정보/프로그래머스.png"></p>

# 프로그래머스 SQL 고득점 SELECT LEVEL 1 - 여러 기준으로 정렬하기
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
  
동물 보호소에 들어온 모든 동물의 아이디와 이름, 보호 시작일을 이름 순으로 조회하는 SQL문을 작성해주세요. 단, 이름이 같은 동물 중에서는 보호를 나중에 시작한 동물을 먼저 보여줘야 합니다.  
예를 들어 `ANIMAL_INS` 테이블이 다음과 같다면  

```sql
ANIMAL_ID	ANIMAL_TYPE	DATETIME	        INTAKE_CONDITION        NAME    SEX_UPON_INTAKE
A349996	        Cat	        2018-01-22 14:32:00	Normal	                Sugar	Neutered Male
A350276	        Cat	        2017-08-13 13:50:00	Normal	                Jewel	Spayed Female
A396810	        Dog	        2016-08-22 16:13:00	Injured	                Raven	Spayed Female
A410668	        Cat	        2015-11-19 13:41:00	Normal	                Raven	Spayed Female
```

1. 이름을 사전 순으로 정렬하면 다음과 같으며, 'Jewel', 'Raven', 'Sugar'  
2.  'Raven'이라는 이름을 가진 개와 고양이가 있으므로, 이 중에서는 보호를 나중에 시작한 개를 먼저 조회합니다.  
따라서 SQL문을 실행하면 다음과 같이 나와야 합니다.  

```sql
ANIMAL_ID	NAME	DATETIME
A350276	        Jewel	2017-08-13 13:50:00
A396810	        Raven	2016-08-22 16:13:00
A410668	        Raven	2015-11-19 13:41:00
A349996	        Sugar	2018-01-22 14:32:00
```


# 정답
```sql
SELECT ANIMAL_ID, NAME, DATETIME from ANIMAL_INS ORDER BY NAME, DATETIME Desc
```

# 설명
문제에서 요구하는 기준은 2가지 이다.  
1. 모든 동물의 아이디와 이름, 보호 시작일을 **이름 순으로**조회하기  
2. 이름이 같은 동물 중에서는 **보호를 나중에 시작한** 동물이 먼저 보여져야 한다.  
즉, **이름 순으로 조회**한다는 것은 ASC에 해당하는 **오름차순 정렬** 수행 후, DESC에 해당하는 **내림차순 정렬**을 수행시키면 된다.  
  
`NAME`을 정렬한다.(ORDER BY `NAME`) 그 뒤(,) `DATETIME`도 내림차순 정렬(Desc) 시켜준다.  

---
[문제 출처]  
[프로그래머스](https://programmers.co.kr/)의 SQL 고득점 Kit  