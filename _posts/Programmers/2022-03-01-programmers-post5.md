---
layout: post
title:  "[Programmers, SQL 고득점 Kit] SELECT - 동물의 아이디와 이름"
date:   2022-03-01 17:35:00+0900
categories: jekyll update
tags: [programmers level 1]
---

<p align="center"><img src="/assets/img/blog/정보/프로그래머스.png"></p>

# 프로그래머스 SQL 고득점 SELECT LEVEL 1 - 동물의 아이디와 이름
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
  
동물 보호소에 들어온 모든 동물의 아이디와 이름을 ANIMAL_ID순으로 조회하는 SQL문을 작성해주세요. SQL을 실행하면 다음과 같이 출력되어야 합니다.   

```sql
ANIMAL_ID	NAME
A349996 	Sugar
A350276 	Jewel
A350375 	Meo
A352555 	Harley
A352713 	Gia
A352872 	Peanutbutter
A353259 	Bj
# 이하 생략
```


# 정답
```sql
SELECT ANIMAL_ID, NAME from ANIMAL_INS Order by ANIMAL_ID
```

# 설명
단순하게 **모든 동물의 아이디와 이름**만 출력하면 되는 문제이기에 SELECT 뒤에 `ANIMAL_ID`와 `NAME` 필드를 적는다.    

---
[문제 출처]  
[프로그래머스](https://programmers.co.kr/)의 SQL 고득점 Kit  