# SQL과 django ORM

## 기본 준비 사항

* https://bit.do/djangoorm에서 csv 파일 다운로드

* django app

  * `django_extensions` 설치

  * `users` app 생성

  * csv 파일에 맞춰 `models.py` 작성 및 migrate

    아래의 명령어를 통해서 실제 쿼리문 확인

    ```bash
    $ python manage.py sqlmigrate users 0001
    ```

* `db.sqlite3` 활용

  * `sqlite3`  실행

    ```bash
    $ ls
    db.sqlite3 manage.py ...
    $ sqlite3 db.sqlite3
    ```

  * csv 파일 data 로드

    ```sqlite
    sqlite > .tables
    auth_group                  django_admin_log
    auth_group_permissions      django_content_type
    auth_permission             django_migrations
    auth_user                   django_session
    auth_user_groups            auth_user_user_permissions  
    users_user
    sqlite > .mode csv
    sqlite > .import users.csv users_user
    sqlite > SELECT COUNT(*) FROM user_users;
    100
    ```

* 확인

  * sqlite3에서 스키마 확인

    ```sqlite
    sqlite > .schema users_user
    CREATE TABLE IF NOT EXISTS "users_user" ("id" integer NOT NULL PRIMARY KEY AUTOINCREMENT, "first_name" varchar(10) NOT NULL, "last_name" varchar(10) NOT NULL, "age" integer NOT NULL, "country" varchar(10) NOT NULL, "phone" varchar(15) NOT NULL, "balance" integer NOT NULL);
    ```

    

## 문제

> 아래의 문제들을 sql문과 대응되는 orm을 작성 하세요.

### 기본 CRUD 로직

1. 모든 user 레코드 조회

   ```python
   # orm
   ```

      ```sql
   -- sql
      ```

2. user 레코드 생성

   ```python
   # orm
   ```

   ```sql
   -- sql
   ```

   * 하나의 레코드를 빼고 작성 후 `NOT NULL` constraint 오류를 orm과 sql에서 모두 확인 해보세요.

3. 해당 user 레코드 조회

   ```python
   # orm
   ```

      ```sql
   -- sql
      ```

4. 해당 user 레코드 수정

   ```python
   # orm
   ```

      ```sql
   -- sql
      ```

5. 해당 user 레코드 삭제

   ```python
   # orm
   ```

      ```sql
   -- sql
      ```

### 조건에 따른 쿼리문

1. 전체 인원 수 

      ```python
   # orm
   ```

      ```sql
   -- sql
      ```

2. 나이가 30인 사람의 이름

      ```python
   # orm
   ```

      ```sql
   -- sql
      ```

3. 나이가 30살 이상인 사람의 인원 수

      ```python
   # orm
   ```

      ```sql
   -- sql
      ```

4. 나이가 30이면서 성이 김씨인 사람의 인원 수

   ```python
   # orm
   ```

      ```sql
   -- sql
      ```

5. 지역번호가 02인 사람의 인원 수

   ```python
   # orm
   ```

      ```sql
   -- sql
      ```

6. 거주 지역이 강원도이면서 성이 황씨인 사람의 이름

   ```python
   # orm
   ```

      ```sql
   -- sql
      ```



### 정렬 및 LIMIT, OFFSET

1. 나이가 많은 사람 10명

      ```python
   # orm
   ```

      ```sql
   -- sql
      ```

2. 잔액이 적은 사람 10명

      ```python
   # orm
   ```

      ```sql
   -- sql
      ```

3. 성, 이름 내림차순 순으로 5번째 있는 사람

      ```python
   # orm
   ```

      ```sql
   -- sql
      ```



### 표현식

1. 전체 평균 나이

      ```python
   # orm
   ```

      ```sql
   -- sql
      ```

2. 김씨의 평균 나이

      ```python
   # orm
   ```

      ```sql
   -- sql
      ```

3. 계좌 잔액 중 가장 높은 값

      ```python
   # orm
   ```

      ```sql
   -- sql
      ```

4. 계좌 잔액 총액

      ```python
   # orm
   ```

      ```sql
   -- sql
      ```