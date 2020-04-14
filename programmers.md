https://programmers.co.kr/learn/challenges



## SELECT

`ANIMAL_INS` 테이블은 동물 보호소에 들어온 동물의 정보를 담은 테이블입니다. `ANIMAL_INS` 테이블 구조는 다음과 같으며, 

`ANIMAL_ID`, `ANIMAL_TYPE`, `DATETIME`, `INTAKE_CONDITION`, `NAME`, `SEX_UPON_INTAKE`는 

각각 동물의 아이디, 생물 종, 보호 시작일, 보호 시작 시 상태, 이름, 성별 및 중성화 여부를 나타냅니다.



> 모든 레코드 조회하기



모든 동물의 정보를 ANIMAL_ID 순으로 조회하기 

```SQL
SELECT * FROM ANIMAL_INS ORDER BY ANIMAL_ID ASC;
```



- SELECT * FROM ANIMAL_INS ORDER BY ANIMAL_ID ASC; 오름차순
- SELECT * FROM ANIMAL_INS ORDER BY ANIMAL_ID DESC; 내림차순



> 역순 정렬하기



동물 보호소에 들어온 모든 동물의 이름과 보호 시작일을 조회하는 SQL문을 작성해주세요. 이때 결과는 ANIMAL_ID 역순으로 보여주세요. 

```SQL
SELECT NAME, DATETIME FROM ANIMAL_INS ORDER BY ANIMAL_ID DESC;
```



> 아픈 동물 찾기



동물 보호소에 들어온 동물 중 아픈 동물의 아이디와 이름을 조회하는 SQL 문을 작성해주세요. 이때 결과는 아이디 순으로 조회해주세요.

```SQL
SELECT ANIMAL_ID, NAME FROM ANIMAL_INS WHERE INTAKE_CONDITION LIKE "sick" ORDER BY ANIMAL_ID ASC;
```



> 어린 동물 찾기



동물 보호소에 들어온 동물 중 젊은 동물[1](https://programmers.co.kr/learn/courses/30/lessons/59037?language=mysql#fn1)의 아이디와 이름을 조회하는 SQL 문을 작성해주세요. 이때 결과는 아이디 순으로 조회해주세요.

```SQL
SELECT ANIMAL_ID, NAME FROM ANIMAL_INS WHERE INTAKE_CONDITION NOT LIKE "Aged";
```



> 동물의 아이디와 이름



아이디와 이름 모두 오름차순으로 조회해주세요.

```SQL
SELECT ANIMAL_ID, NAME FROM ANIMAL_INS ORDER BY ANIMAL_ID, NAME ASC;
```



> 여러 기준으로 정렬하기



동물 보호소에 들어온 모든 동물의 아이디와 이름, 보호 시작일을 이름 순으로 조회하는 SQL문을 작성해주세요.

 단, 이름이 같은 동물 중에서는 보호를 나중에 시작한 동물을 먼저 보여줘야 합니다.



```SQL
SELECT ANIMAL_ID, NAME, DATETIME FROM ANIMAL_INS ORDER BY NAME ASC, DATETIME DESC;
```



- ORDER BY <필드명> ASC, <필드명> DESC 이런식으로 여러 기준으로 정렬이 가능하다.



> 상위 n개 레코드



동물 보호소에 가장 먼저 들어온 동물의 이름을 조회하는 SQL 문

```SQL
SELECT NAME FROM ANIMAL_INS ORDER BY DATETIME ASC LIMIT 1;
```



## SUM, MAX, MIN

> 최대값 구하기



가장 최근에 들어온 동물은 언제 들어왔는지 조회하는 SQL 문

```SQL
SELECT MAX(DATETIME) FROM ANIMAL_INS;
```



가장 늦게 들어온 동물은 언제 들어왔는지 조회하는 SQL 문

```SQL
SELECT MIN(DATETIME) FROM ANIMAL_INS;
```



> 최솟값 구하기











## GROUP BY





## IS NULL



## JOIN





## String, Date







