---
layout: post
title:  "[Programmers, SQL 고득점 Kit] GROUP BY - 입양 시각 구하기(2)"
date:   2022-03-02 13:24:00+0900
categories: jekyll update
tags: [programmers level 4]
---

<p align="center"><img src="/assets/img/blog/정보/프로그래머스.png"></p>

# 프로그래머스 SQL 고득점 Kit
## GROUP BY LEVEL 4 - 입양 시각 구하기(2)

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
  
보호소에서는 몇 시에 입양이 가장 활발하게 일어나는지 알아보려 합니다. 0시부터 23시까지, 각 시간대별로 입양이 몇 건이나 발생했는지 조회하는 SQL문을 작성해주세요. 이때 결과는 시간대 순으로 정렬해야 합니다.  
SQL문을 실행하면 다음과 같이 나와야 합니다.  

```sql
HOUR	COUNT
0	    0
1	    0
2	    0
3	    0
4	    0
5	    0
6	    0
7	    3
8	    1
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
20	    0
21	    0
22	    0
23	    0
```


# 정답
```sql
SET @hour = -1;
SELECT (@hour := @hour+1) AS HOUR,
(SELECT COUNT(DATETIME) FROM ANIMAL_OUTS WHERE HOUR(DATETIME) = @hour) AS COUNT 
FROM ANIMAL_OUTS WHERE @hour < 23
```
  
# 설명
문제에서 요구하는 조건들을 살펴보자.  
1. **각 시간대별로 입양이 몇 건이나 발생했는지 조회.** 이는 0시부터 23시까지. 즉, 모든 시간대를 조회해야한다는 조건이 붙는다.  
2. 시간대 순으로 정렬한다.  
너무 문제의 난이도가 급상승하여 많이 어려웠다. 해당 문제는 **로컬변수 `@`를 사용하는 문제이다.**  
일반적으로 COUNT함수를 이용하여 출력을 하면 당연하게 값이 존재하지 않는 필드값은 빈 칸으로 나온다.  
즉, 빈 칸으로 나와야 할 값을 0으로 출력을 시켜야하는데.. 되게 많이 시간을 허비하였다.  

차근차근 풀어보자.  
```sql
SET @hour = -1
```
**SET** 구문을 사용하며 변수와 초기값을 설정한다. 이때, `@`가 붙은 변수는 **프로시저가 종료되도 유지된다.** 해당 변수를 이용하여 문제를 풀어보자.  

```sql
SELECT (@hour := @hour+1) AS HOUR, ...
```
결과를 확인하면 HOUR 레코드에 0부터 23까지 나와있다. 즉, 0부터 23까지 값을 표현해야한다. `(@hour := @hour+1)`을 통하여 SELECT문 전체를 @hour번 실행시킨다. 이때 `:=`은 대입연산자로써 (@hour := @hour+1)은 `@hour`값을 1씩 증가시키라는 의미이다. 또한 초기값이 `-1`로 초기화 한 이유는 위 (@hour := @hour+1)식에 의하여 +1이 진행되 @hour값이 0에서 시작된다. 그 뒤 `AS` 구문을 사용하여 출력되는 레코드의 이름을 `HOUR`로 설정한다.  

```sql
... , (SELECT COUNT(DATETIME) FROM ANIMAL_OUTS WHERE HOUR(DATETIME) = @hour) AS COUNT ...
```
그 뒤 각 시간대 별 입양의 횟수를 조회하기 위한 레코드 `COUNT`가 출력된다. 해당 레코드를 출력시키기 위해서 서브쿼리를 별도로 작성한다.  
`COUNT(DATETIME)`을 통해 `DATETIME` 레코드의 횟수를 조회한다. 그 뒤 조건문 `WHERE`을 사용하여 `HOUR(DATETIME)`이 `@hour`인 경우만 카운트 시킨다. 이를 통하여 `HOUR(DATETIME)`으로 뽑아내는 날짜 데이터 타입은 **시간**만 나오며 뽑아진 시간과 `@hour`이 같다면 `COUNT(DATETIME)`을 통해 횟수가 카운트 되도록 한다. 그 뒤 `AS`구문을 사용하여 출력되는 레코드의 이름을 `COUNT`로 설정한다.  

여기서 끝나는 것이 아닌 **0시부터 23시까지만 출력되도록 설정해야한다.** 이를 생각하지 않으면 `(@hour := @hour+1)`이 `@hour`의 값을 계속해서 연산시키기 때문에 `@hour`이 0시부터 23시까지만 연산이 이루어지도록 설정해야 한다.  

```sql
... FROM ANIMAL_OUTS WHERE @hour < 23
```
조건 `WHERE`을 사용하여 `@hour`이 23보다 작거나 같을 때 까지 연산이 되도록 한다. 최종적으로는 아래와 같다.  

```sql
SET @hour = -1;
SELECT (@hour := @hour+1) AS HOUR, (SELECT COUNT(DATETIME) FROM ANIMAL_OUTS WHERE HOUR(DATETIME) = @hour) AS COUNT
FROM ANIMAL_OUTS WHERE @hour < 23
```
  
  
  

---
[문제 출처]  
[프로그래머스](https://programmers.co.kr/)의 SQL 고득점 Kit  