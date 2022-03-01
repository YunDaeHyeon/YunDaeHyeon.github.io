---
layout: post
title:  "[Programmers, SQL 고득점 Kit] SELECT - 아픈 동물 찾기"
date:   2022-03-01 17:20:00+0900
categories: jekyll update
tags: [programmers level 1]
---

<p align="center"><img src="/assets/img/blog/정보/프로그래머스.png"></p>

# 프로그래머스 SQL 고득점 SELECT LEVEL 1 - 아픈 동물 찾기
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
  
동물 보호소에 들어온 동물 중 아픈 동물(`INTAKE_CONDITION`가 *Sick*인 경우)의 아이디와 이름을 조회하는 SQL 문을 작성해주세요. 이때 결과는 아이디 순으로 조회해주세요. 예를 들어 `ANIMAL_INS` 테이블이 다음과 같다면  

```sql
ANIMAL_ID	ANIMAL_TYPE	DATETIME	        INTAKE_CONDITION	    NAME	    SEX_UPON_INTAKE
A365172         Dog	        2014-08-26 12:53:00	Normal	                Diablo	    Neutered Male
A367012 	Dog     	2015-09-16 09:06:00	Sick	                Miller	    Neutered Male
A365302 	Dog	        2017-01-08 16:34:00	Aged	                Minnie	    Spayed Female
A381217 	Dog	        2017-07-08 09:41:00	Sick	                Cherokee	Neutered Male
```

이 중 아픈 동물은 Miller와 Cherokee입니다. 따라서 SQL문을 실행하면 다음과 같이 나와야 합니다.  
  
```sql
ANIMAL_ID	NAME
A367012	        Miller
A381217	        Cherokee
```

# 정답
```sql
SELECT ANIMAL_ID, NAME from ANIMAL_INS WHERE INTAKE_CONDITION = "SICK" order by ANIMAL_ID
```

# 설명
`INTAKE_CONDITION`가 *Sick*인 필드만 조회하기에 조건식에 해당하는 **WHERE**을 사용한다.  
조건(WHERE) INTAKE_CONDITION = "SICK"인 경우에 정렬한다.(Order by) ANIMAL_ID(아이디 순으로)  

# 조건문
```sql
SELECT (속성1, 속성2, ...) FROM {Table} WHERE { 조건식 }
```
**조건식**에 의해 해당하는 행을 선택한 뒤, **(속성1, 속성2, ...)**에 의해 열을 선택한다.  
이때,
```sql
WHERE (조건1) AND (조건2)
```
**AND**를 사용할 시 **(조건1)과 (조건2)를 모두 만족하는 행의 모든 열을 선택한다.**  
- OR(조건) : 조건 하나라도 만족하면 참  
- NOT(조건) : 조건을 만족하지 않는다면  
- WHERE (속성1) LIKE 'A_' : 속성1중 'A+1`글자의 값을 가진 행의 열 선택  
- LIKE 'A%' : A로 시작하는 값을 가진 열 선택  
- LIKE '%A' : A로 끝나는 값을 가진 열 선택  
- LIKE '%A%' : A를 포함하는 값을 가진 열 선택  
- SELECT * FROM {Table} WHERE (속성1) BETWEEN (값1) And (값2) : {Table}에서 (속성1)의 값이 (값1)과 (값2) 사이인 행의 모든 열 선택   

---
[문제 출처]  
[프로그래머스](https://programmers.co.kr/)의 SQL 고득점 Kit  