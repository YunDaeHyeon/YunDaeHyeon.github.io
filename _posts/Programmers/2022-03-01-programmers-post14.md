---
layout: post
title:  "[Programmers, SQL 고득점 Kit] GROUP BY - 입양 시각 구하기(1)"
date:   2022-03-01 23:39:00+0900
categories: jekyll update
tags: [programmers level 2]
---

<p align="center"><img src="/assets/img/blog/정보/프로그래머스.png"></p>

# 프로그래머스 SQL 고득점 Kit
## GROUP BY LEVEL 2 - 입양 시각 구하기(1)

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
  
보호소에서는 몇 시에 입양이 가장 활발하게 일어나는지 알아보려 합니다. 09:00부터 19:59까지, 각 시간대별로 입양이 몇 건이나 발생했는지 조회하는 SQL문을 작성해주세요. 이때 결과는 시간대 순으로 정렬해야 합니다.  
SQL문을 실행하면 다음과 같이 나와야 합니다.  

```sql
HOUR	    COUNT
9	    1
10	    2
11	    13
12	    10
13	    14
14	    9
15	    7
16	    10
17	    12
18	    16
19	    2
```


# 정답
```sql
SELECT HOUR(DATETIME), COUNT(DATETIME) FROM ANIMAL_OUTS WHERE HOUR(DATETIME) >= 9 
AND HOUR(DATETIME) < 20 GROUP BY HOUR(DATETIME) ORDER BY HOUR(DATETIME)
```
  
# 설명
문제에서 요구하는 조건을 살펴보자. 
1. 09:00부터 19:59까지의 시간만 출력  
2. 몇 건의 입양이 시간대별로 발생하였는지 조회  
3. 결과는 시간대 순으로 정렬  
차근차근 작성해보자.  
먼저 결과를 보면 `HOUR`이라는 레코드가 있다.  또한, **시간**만 출력하여 결과를 출력하였다. 똑같이 **시간만을 이용하여 출력하기 위해** `HOUR()`함수를 사용하였다. AS 구문은 사용하지 않는다.  

```sql
SELECT HOUR(DATETIME), COUNT(DATETIME) FROM ANIMAL_OUTS
```

이때 문제를 보면 **09:00부터 19:59까지의 시간에 해당하는 입양 횟수를 카운트한다.** 즉, 조건식을 실행한 뒤 그룹화를 진행해야한다. 이를 위하여 `WHERE` 구문을 사용하여 조건식을 실행한 뒤 입양 횟수를 조회하기 위하여 `GROUP BY`를 통한 그룹화를 진행한다.  

```sql
SELECT HOUR(DATETIME), COUNT(DATETIME) FROM ANIMAL_OUTS WHERE HOUR(DATETIME) >= 9 
AND HOUR(DATETIME) < 20 GROUP BY HOUR(DATETIME)
```
그 뒤 제출하면 오답이 출력된다. 그 이유는 **정렬이 이루어지지 않았기 때문이다.**  

```sql
SELECT HOUR(DATETIME), COUNT(DATETIME) FROM ANIMAL_OUTS WHERE HOUR(DATETIME) >= 9 
AND HOUR(DATETIME) < 20 GROUP BY HOUR(DATETIME) ORDER BY HOUR(DATETIME)
```
시간대 순으로 오름차순 정렬을 수행시킨 뒤 제출하면 정답이다.  
  
  
  

---
[문제 출처]  
[프로그래머스](https://programmers.co.kr/)의 SQL 고득점 Kit  