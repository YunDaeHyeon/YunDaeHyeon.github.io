---
layout: post
title:  "[Programmers, SQL 고득점 Kit] GROUP BY - 고양이와 개는 몇 마리 있을까"
date:   2022-03-01 22:42:00+0900
categories: jekyll update
tags: [programmers]
---

<p align="center"><img src="/assets/img/blog/정보/프로그래머스.png"></p>

# 프로그래머스 SQL 고득점 Kit
## GROUP BY LEVEL 2 - 고양이와 개는 몇 마리 있을까

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
  
---
  
동물 보호소에 들어온 동물 중 고양이와 개가 각각 몇 마리인지 조회하는 SQL문을 작성해주세요. 이때 고양이를 개보다 먼저 조회해주세요.  
예를 들어 `ANIMAL_INS` 테이블이 다음과 같다면  

```sql
ANIMAL_ID	ANIMAL_TYPE	DATETIME	       INTAKE_CONDITION    NAME	    SEX_UPON_INTAKE
A373219	        Cat	        2014-07-29 11:43:00	Normal	            Ella	Spayed Female
A377750	        Dog	        2017-10-25 17:17:00	Normal	            Lucy	Spayed Female
A354540	        Cat	        2014-12-11 11:48:00	Normal	            Tux	        Neutered Male
```

고양이는 2마리, 개는 1마리 들어왔습니다. 따라서 SQL문을 실행하면 다음과 같이 나와야 합니다.   

```sql
ANIMAL_TYPE	count
Cat	        2
Dog	        1
```


# 정답
```sql
SELECT ANIMAL_TYPE, count(ANIMAL_TYPE) FROM ANIMAL_INS GROUP BY ANIMAL_TYPE ORDER BY ANIMAL_TYPE
```

# 설명
문제에서 요구하는 것은 아래와 같다.  
1. 고양이와 개가 **각각** 몇 마리인지 조회  
2. 고양이를 개보다 **먼저** 조회  
COUNT함수를 이용하여 고양이와 개의 수를 가져오고 GROUP BY를 통해 고양이와 개를 분리시켜 1번 조건을 만족시킨다. 다음으로 **고양이를 개보다 먼저 조회하기 위하여** ORDER BY를 통해 ANIMAL_TYPE을 정렬시킨다.  
  

---
[문제 출처]  
[프로그래머스](https://programmers.co.kr/)의 SQL 고득점 Kit  