---
layout: post
title:  "[Programmers, SQL 고득점 Kit] GROUP BY - 동명 동물 수 찾기"
date:   2022-03-01 23:00:00+0900
categories: jekyll update
tags: [programmers level 2]
---

<p align="center"><img src="/assets/img/blog/정보/프로그래머스.png"></p>

# 프로그래머스 SQL 고득점 Kit
## GROUP BY LEVEL 2 - 동명 동물 수 찾기

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
  
동물 보호소에 들어온 동물 이름 중 두 번 이상 쓰인 이름과 해당 이름이 쓰인 횟수를 조회하는 SQL문을 작성해주세요. 이때 결과는 이름이 없는 동물은 집계에서 제외하며, 결과는 이름 순으로 조회해주세요.  
예를 들어 `ANIMAL_INS` 테이블이 다음과 같다면  

```sql
ANIMAL_ID	ANIMAL_TYPE	DATETIME	        INTAKE_CONDITION      NAME	    SEX_UPON_INTAKE
A396810	        Dog	        2016-08-22 16:13:00	Injured	                Raven	    Spayed Female
A377750	        Dog	        2017-10-25 17:17:00	Normal	                Lucy	    Spayed Female
A355688	        Dog	        2014-01-26 13:48:00	Normal	                Shadow	    Neutered Male
A399421	        Dog	        2015-08-25 14:08:00	Normal	                Lucy	    Spayed Female
A400680	        Dog	        2017-06-17 13:29:00	Normal	                Lucy	    Spayed Female
A410668	        Cat	        2015-11-19 13:41:00	Normal	                Raven	    Spayed Female
```

- Raven 이름은 2번 쓰였습니다.  
- Lucy 이름은 3번 쓰였습니다.  
- Shadow 이름은 1번 쓰였습니다.  
따라서 SQL문을 실행하면 다음과 같이 나와야 합니다.   

```sql
NAME	COUNT
Lucy	3
Raven	2
```


# 정답
```sql
SELECT NAME, COUNT(NAME) FROM ANIMAL_INS GROUP BY NAME HAVING COUNT(NAME) >= 2 ORDER BY NAME
# 점점 문제가 어려워진다...
```

# 설명
문제에서 요구하는 것은 아래와 같다.  
1. 두 번 이상 쓰인 이름만 출력  
2. 이름이 쓰인 횟수를 조회  
3. 결과는 **이름**순으로 정렬
차근차근 작성해보자. 먼저 `GROUP BY`를 사용하여 `NAME`을 그룹화 시켜 각 이름이 쓰인 횟수를 구한다.  

```sql
SELECT NAME, COUNt(NAME) FROM ANIMAL_INS GROUP BY NAME
```
그 뒤 `두 번 이상 쓰인 이름`을 구하기 위하여 **그룹화 시킨 뒤 조건식을 실행할 필요가 있다.** 이에 해당하는 역할을 수행해주는 
조건문 `HAVING`을 사용하여 2번 이상 쓰인 동물만 구한다.

```sql
SELECT NAME, COUNt(NAME) FROM ANIMAL_INS GROUP BY NAME HAVING COUNT(NAME) >= 2
```
여기서 sql을 실행하면 해답이 나오지만 틀렸다고 나온다. 그 이유는 **이름순으로 정렬을 하지 않았기 때문이다.**  
`ORDER BY` 구문을 삽입해주면 정답이다.  

```sql
SELECT NAME, COUNT(NAME) FROM ANIMAL_INS GROUP BY NAME HAVING COUNT(NAME) >= 2 ORDER BY NAME
```
  

---
[문제 출처]  
[프로그래머스](https://programmers.co.kr/)의 SQL 고득점 Kit  