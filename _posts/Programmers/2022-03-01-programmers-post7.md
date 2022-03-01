---
layout: post
title:  "[Programmers, SQL 고득점 Kit] SELECT - 상위 n개 레코드"
date:   2022-03-01 17:55:00+0900
categories: jekyll update
tags: [programmers]
---

<p align="center"><img src="/assets/img/blog/정보/프로그래머스.png"></p>

# 프로그래머스 SQL 고득점 SELECT LEVEL 1 - 상위 n개 레코드
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
  
동물 보호소에 가장 먼저 들어온 동물의 이름을 조회하는 SQL 문을 작성해주세요.  
예를 들어 `ANIMAL_INS` 테이블이 다음과 같다면  

```sql
ANIMAL_ID	ANIMAL_TYPE	DATETIME	        INTAKE_CONDITION	NAME	        SEX_UPON_INTAKE
A399552	        Dog	        2013-10-14 15:38:00	Normal	                Jack	        Neutered Male
A379998	        Dog	        2013-10-23 11:42:00	Normal	                Disciple	Intact Male
A370852	        Dog	        2013-11-03 15:04:00	Normal	                Katie	        Spayed Female
A403564	        Dog	        2013-11-18 17:03:00	Normal	                Anna	        Spayed Female
```

이 중 가장 보호소에 먼저 들어온 동물은 Jack입니다. 따라서 SQL문을 실행하면 다음과 같이 나와야 합니다.  
따라서 SQL문을 실행하면 다음과 같이 나와야 합니다.  

```sql
NAME
Jack
```


# 정답
```sql
SELECT NAME from ANIMAL_INS ORDER BY DATETIME LIMIT 1
```

# 설명
문제에서 요구하는 것은 **가장 먼저 들어온 동물**을 뽑아내는 것이다. `DATATIME` 필드를 **오름차순 정렬**시킨 뒤 상위 레코드를 뽑아내는 `LIMIT`구문을 사용한다.  
`ORDER BY DATETIME` = `DATETIME`을 **오름차순으로 정렬한다.**
`LIMIT 1` = **상위 레코드 1개를 뽑아낸다.**

---
[문제 출처]  
[프로그래머스](https://programmers.co.kr/)의 SQL 고득점 Kit  