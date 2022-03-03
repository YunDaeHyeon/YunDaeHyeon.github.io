---
layout: post
title:  "[Programmers, SQL 고득점 Kit] IS NULL - NULL 처리하기"
date:   2022-03-03 11:40:00+0900
categories: jekyll update
tags: [programmers level 2]
---

<p align="center"><img src="/assets/img/blog/정보/프로그래머스.png"></p>

# 프로그래머스 SQL 고득점 Kit
## IS NUL LEVEL 2 - NULL 처리하기

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
  
입양 게시판에 동물 정보를 게시하려 합니다. 동물의 생물 종, 이름, 성별 및 중성화 여부를 아이디 순으로 조회하는 SQL문을 작성해주세요. 이때 프로그래밍을 모르는 사람들은 NULL이라는 기호를 모르기 때문에, 이름이 없는 동물의 이름은 "No name"으로 표시해 주세요.

예를 들어 ANIMAL_INS 테이블이 다음과 같다면
```sql
ANIMAL_ID	ANIMAL_TYPE	DATETIME	        INTAKE_CONDITION	NAME	SEX_UPON_INTAKE
A350276	    Cat	        2017-08-13 13:50:00	Normal	            Jewel	Spayed Female
A350375	    Cat	        2017-03-06 15:01:00	Normal	            Meo	    Neutered Male
A368930 	Dog	        2014-06-08 13:20:00	Normal	            NULL	Spayed Female
```
마지막 줄의 개는 이름이 없기 때문에, 이 개의 이름은 "No name"으로 표시합니다. 따라서 SQL문을 실행하면 다음과 같이 나와야 합니다.
```sql
ANIMAL_TYPE	NAME	SEX_UPON_INTAKE
Cat	        Jewel	Spayed Female
Cat	        Meo	    Neutered Male
Dog	        Noname	Spayed Female
```
# 정답
```sql
SELECT ANIMAL_TYPE, IFNULL(NAME, "No name") AS NAME, SEX_UPON_INTAKE FROM ANIMAL_INS
```
  
# 설명
해당 문제는 `NULL`값에 해당되는 데이터가 **"No name"**이라는 문자열로 치환되어 출력시키도록 하는 문제이다.  
따라서 `IFNULL`을 사용하여 만약 해당 레코드에 `NULL`이 있다면 **"No name"**으로 바뀌어 출력시키도록 하였다.  
  

---
[문제 출처]  
[프로그래머스](https://programmers.co.kr/)의 SQL 고득점 Kit  