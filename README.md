:star: 해당 README는 키워드만 작성, 자세한 사항은 PDF를 참고하여 공부하시면 됩니다. :star:

[SQL grammar](https://www.sqlite.org/lang.html)

**데이터베이스(DB)**

- 체계화된 데이터의 모임
- 통합관리되는 집합의 모음
- 자료 파일을 조직적으로 통합, 자료 중복을 없앤다.



**데이터베이스 장점**

- 중복 최소화
- 무결성
  - 정확한 정보 보장
- 일관성
- 독립성
  - 물리적과 논리적 독립성
- 표준화
- 보안유지



**RDBMS(관계형 데이터베이스 관리 시스템)**

- 식별자
- 어트리뷰트
- 튜플
- head
- body
- 어트리뷰트값



**schema**

- 데이터베이스에서의 자료의 구조, 표현방법, 관계들을 정의한 구조

  

[**DATATYPE**](https://www.guru99.com/sqlite-data-types.html)



**SQL(structured query language) 개념**

- DDL data definition language

: **CREATE, DROP, ALTER**

- DML data manipulation language

: **INSERT, UPDATE, DELETE, SELECT**

- DCL data create language

: **GRANT, REVOKE, COMMIT, ROLLBACK**



:one: DDL

path 설정 후

bash에서

시작: sqlite3

종료: ^Z

```bash
# 시작
$ sqlite3 tutorial.databases
SQLite version 3.29.0 2019-07-10 17:32:03
Enter ".help" for usage hints.
sqlite> .databases
main: C:\Users\student\development\정승원_database_git\database\tutorial.databases

# .mode csv
# .import <파일명.csv> <파일명 임의지정>
sqlite> .mode csv
sqlite> .import hellodb.csv examples

# 테이블 생성
# 키워드는 항상 대문자로 작성
sqlite> CREATE TABLE classmates (
   ...> id INTEGER PRIMARY KEY,
   ...> name TEXT
   ...> );

# .tables 불러오기
sqlite> .tables
classmates  examples

# .schema <테이블명> 불러오기
sqlite> .schema classmates
CREATE TABLE classmates (
id INTEGER PRIMARY KEY,
name TEXT
);
sqlite> .schema examples
CREATE TABLE examples(
  "id" TEXT,
  "first_name" TEXT,
  "last_name" TEXT,
  "age" TEXT,
  "country" TEXT,
  "phone" TEXT
);

# 테이블 삭제
sqlite> DROP TABLE classmates;
sqlite> .tables
examples
```

:star: AUTOINCREAMENT

```bash
#AUTOINCREMENT 을 지정함으로써 이전에 삭제된 데이터와 id 값이 구별되도록 저장할 수 있다.
sqlite> CREATE TABLE tests (
   ...> id INTEGER PRIMARY KEY AUTOINCREMENT,
   ...> name TEXT
   ...> );
sqlite> .tables
classmates  examples    tests
sqlite> INSERT INTO tests VALUES (1, "홍길동"), (2, "김철수");
sqlite> SELECT * FROM tests;
id          name
----------  ----------
1           홍길동
2           김철수
sqlite> DELETE FROM tests WHERE id=2;
sqlite> SELECT * FROM tests;
id          name
----------  ----------
1           홍길동
sqlite> INSERT INTO tests (name) VALUES ("최철순");
sqlite> SELECT * FROM tests;
id          name
----------  ----------
1           홍길동
3           최철순 # PK가 3번으로 저장된 것을 확인할 수 있다.

# 2번이 공석이 되면서 메모리가 사용되는 것
# 그러므로 특정한 상황이 아닌경우 AUTOINCREMENT을 만들지 않아야한다.
# 그렇지 않은 상황에서 사용하게 되면 불필요하게 메모리를 사용하게 된다.
```

:star: ALTER

```bash
sqlite> CREATE TABLE articles(
   ...>     title TEXT NOT NULL,
   ...>     content TEXT NOT NULL
   ...> );
sqlite> .schema
CREATE TABLE examples(
  "id" TEXT,
  "first_name" TEXT,
  "last_name" TEXT,
  "age" TEXT,
  "country" TEXT,
  "phone" TEXT
);
CREATE TABLE classmates (
name TEXT NOT NULL,
age INT NOT NULL,
address TEXT NOT NULL
);
CREATE TABLE tests (
id INTEGER PRIMARY KEY AUTOINCREMENT,
name TEXT
);
CREATE TABLE sqlite_sequence(name,seq);
CREATE TABLE ssafy (
id INTEGER PRIMARY KEY,
location TEXT,
class INTEGER
);
CREATE TABLE users(
  "id" TEXT,
  "first_name" TEXT,
  "last_name" TEXT,
  "age" TEXT,
  "country" TEXT,
  "phone" TEXT,
  "balance" TEXT
);
CREATE TABLE articles(
    title TEXT NOT NULL,
    content TEXT NOT NULL
);
sqlite>
sqlite> INSERT INTO articles VALUES ("1번째 글", "hello database");
# 테이블명 변경
sqlite> ALTER TABLE articles RENAME TO news;

# 컬럼추가 방법

# 1.
sqlite> ALTER TABLE news ADD COLUMN published_date INTEGER;
sqlite> SELECT * FROM news;
title       content         published_date
----------  --------------  --------------
1번째 글       hello database

# 2.
sqlite> INSERT INTO news VALUES ("title", "content", datetime("now", "localtime"));
sqlite> SELECT * FROM news;
title       content         published_date
----------  --------------  --------------
1번째 글       hello database
title       content         2019-09-18 16:

# 3.
sqlite> ALTER TABLE news ADD COLUMN subtitle TEXT NOT NULL DEFAULT 1;
sqlite> SELECT * FROM news;
title       content         published_date  subtitle
----------  --------------  --------------  ----------
1번째 글       hello database                  1
title       content         2019-09-18 16:  1

```



:two:DML

```bash
# DATA 추가 
# *순서 및 개수를 똑같이 넣어야 한다.
# INSERT INTO table (column1, column2...)
sqlite> INSERT INTO classmates (name, age)
   ...> VALUES("홍길동", 23);

# 키워드 모든정보 키워드 파일명
# SELECT * FROM Table; 
sqlite> SELECT * FROM examples;
1,"길동","홍",600,"충청도",010-2424-1232

sqlite> SELECT * FROM classmates;
id          name        age         address
----------  ----------  ----------  ----------
1           홍길동         23

# 모든 열에 데이터를 넣을때는 따로 지정할 필요없다.
sqlite> INSERT INTO classmates
   ...> VALUES(3, "홍길동", 30, "서울");
sqlite> SELECT * FROM classmates;
id          name        age         address
----------  ----------  ----------  ----------
1           홍길동         23
2           홍길동         23          서울
3           홍길동         30          서울

# .headers on
# .mode column
sqlite> .headers on
sqlite> .mode column
sqlite> SELECT * FROM examples;
id          first_name  last_name   age         country     phone
----------  ----------  ----------  ----------  ----------  -------------
1           길동          홍           600         충청도         010-2424-1232

sqlite> SELECT name FROM classmates;
name
----------
홍길동

# INTEGER PRIMARY KEY 타입으로 컬럼 생성시 rowid를 대체할 수 있다.
# 따로 id를 지정하지 않으면 SQLite는 PK를 지정하지 않으면 자동으로 정의된다.
# 지정 한 경우 자동으로 정의되지 않기때문에 INSERT 시 지정해줘야 하는 불편함이 있다.
sqlite> SELECT rowid, * FROM classmates;
id          id          name        age         address
----------  ----------  ----------  ----------  ----------
1           1           홍길동         23
2           2           홍길동         23          서울
3           3           홍길동         30          서울


# id를 따로 지정하지 않고 자동 생성하기 위해서 id는 따로 지정하지 않는다.
sqlite> DROP TABLE classmates;
sqlite> CREATE TABLE classmates (
   ...> name TEXT NOT NULL,
   ...> age INT NOT NULL,
   ...> address TEXT NOT NULL
   ...> );
sqlite> INSERT INTO classmates
   ...> VALUES("홍길동", 30, "서울");
sqlite> SELECT * FROM classmates;
name        age         address
----------  ----------  ----------
홍길동         30          서울
sqlite> SELECT rowid, * FROM classmates;
rowid       name        age         address
----------  ----------  ----------  ----------
1           홍길동         30          서울
# 여러개의 데이터를 한 번에 저장 가능하다.
sqlite> INSERT INTO classmates
   ...> VALUES("홍길동", 30, "서울"), ("박나래", 24, "서울");
sqlite> SELECT rowid, * FROM classmates;
rowid       name        age         address
----------  ----------  ----------  ----------
1           홍길동         30          서울
2           홍길동         30          서울
3           홍길동         30          서울
4           박나래         24          서울

sqlite> SELECT name, rowid FROM classmates;
name        rowid
----------  ----------
홍길동         1
홍길동         2
홍길동         3
박나래         4


# LIMIT num 
# 몇 개만 가져오기
sqlite> SELECT name, rowid FROM classmates LIMIT 1;
name        rowid
----------  ----------
홍길동         1

# LIMIT num OFFSET num
# 3번째에 있는 값 하나만 가져오기
# 두번을 뛰어넘어야 세번째것을 가지고 올 수 있다.
sqlite> SELECT name, rowid FROM classmates LIMIT 1 OFFSET 2;
name        rowid
----------  ----------
홍길동         3

# 세번을 뛰어넘어서 네번째것을 가져온다.
sqlite> SELECT name, rowid FROM classmates LIMIT 1 OFFSET 3;
name        rowid
----------  ----------
박나래         4

# WHERE column=value;
# if문과 동일하게 특정한 값만 가져온다.
sqlite> SELECT name FROM classmates
   ...> WHERE age=30;
name
----------
홍길동
홍길동
홍길동

sqlite> SELECT name FROM classmates
   ...> WHERE address="서울";
name
----------
홍길동
홍길동
홍길동
박나래


# AND, OR, NOT, IN
sqlite> SELECT name FROM classmates
   ...> WHERE age=30 and address="서울";
name
----------
홍길동
홍길동
홍길동

# 중복값을 정제하여 조회
sqlite> SELECT DISTINCT name FROM classmates;
name
----------
홍길동
박나래

sqlite> SELECT DISTINCT age FROM classmates;
age
----------
30
24

# 테이블 내의 특정 데이터 삭제
# DELETE FROM <테이블명> WHERE <어트리뷰트명>=<어트리뷰트값>;
# 데이터 삭제시 PK로 삭제하는 것이 일반적이다.
sqlite> DELETE FROM classmates WHERE rowid=3;
sqlite> SELECT * FROM classmates;
name        age         address
----------  ----------  ----------
홍길동         30          서울
홍길동         30          서울
박나래         24          서울
```



:star:UPDATE

```bash
# 테이블내의 특정 어트리뷰트값 갱신
# UPDATE classmates SET name="정승원" WHERE rowid=1;
sqlite> SELECT * FROM classmates;
홍길동|30|서울
홍길동|30|서울
박나래|24|서울
홍길동|30|서울
박나래|24|서울
sqlite> UPDATE classmates SET name="정승원" WHERE rowid=1;
sqlite> SELECT * FROM classmates;
정승원|30|서울
홍길동|30|서울
박나래|24|서울
홍길동|30|서울
박나래|24|서울
```

:star: SELECT + AND/OR/NOT/IN

```bash
# import해야 작업 가능
sqlite> .mode csv
sqlite> .import users.csv users

# age가 30이상인 users 데이터만 조회
sqlite> SELECT * FROM users WHERE age >= 30;
# 
sqlite> SELECT first_name, last_name FROM users WHERE age >= 30;
sqlite> SELECT first_name, last_name, age FROM users WHERE age >= 30 AND last_name="김";
```

:star: Experssions(표현식)

```bash
# users 테이블의 레코드 총 개수
# SELECT COUNT(column) FROM users;
sqlite> SELECT COUNT(*) FROM users;
COUNT(*)
----------
1000
sqlite> SELECT COUNT(*) FROM users WHERE last_name="김";
COUNT(*)
----------
268

# 30살 이상 사람들의 평균나이
# AVG(), SUM(), MIN(), MAX()
# SELECT AVG(column) FROM users; 
sqlite> SELECT AVG(age) FROM users WHERE age >= 30;
AVG(age)
----------------
35.1763285024155

sqlite> SELECT MAX(balance), first_name FROM users;
MAX(balance)  first_name
------------  ----------
990000        선영

sqlite> SELECT AVG(balance) FROM users WHERE age >=30;
AVG(balance)
----------------
153541.425120773

# LIKE (wild cards)
# SELECT * FROM users WHERE age LIKE "";
sqlite> SELECT * FROM users WHERE age LIKE "2%";
sqlite> SELECT * FROM users WHERE phone LIKE "02-%";


# ORDER BY
SELECT * FROM users ORDER BY first_name ASC;
SELECT * FROM users ORDER BY first_name DESC;

SELECT * FROM users ORDER BY age ASC LIMIT 10;

SELECT * FROM users ORDER BY age, last_name ASC LIMIT 10;

sqlite> SELECT last_name, first_name FROM users ORDER BY balance DESC LIMIT 10;
last_name   first_name
----------  ----------
김           선영
나           상현
이           정호
이           상철
최           지아
박           준서
문           미영
고           하윤
유           은정
안           서윤
```



:three: DCL 

: **GRANT, REVOKE, COMMIT, ROLLBACK**