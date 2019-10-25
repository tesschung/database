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
    sqlite > .mode csv
    sqlite > .import users.csv users_user
    sqlite > SELECT COUNT(*) FROM users_user;
    ```



## 문제

> 아래의 문제들을 sql문과 대응되는 orm을 작성 하세요.

##### 기본 CRUD 로직

1. 모든 user 레코드 조회

   ```python
   # orm
   User.objects.all()
   ```

      ```sql
   -- sql
   SELECT * FROM users_user;
      ```

2. user 레코드 생성

   ```python
   # orm
   User.objects.create(first_name='길동', 
                       last_name='홍', 
                       age=30, 
                       country='제주특별자치도', 
                       phone='010-1234-1234',
                       balance=300)
   
   # 2. 
   user = User()
   user.first_name = '길동'
   ..
   user.save()
   ```

   ```sql
   -- sql
   INSERT INTO users_user
   VALUES ('길동', '홍', 30, '제주', '010-1234-1234', 300);
   ```

3. 해당 user 레코드 조회

   ```python
   # orm
   User.objects.get(pk=101)
   ```

      ```sql
   -- sql
   SELECT * FROM users_user
   WHERE id=101;
      ```

4. 해당 user 레코드 수정

   ```python
   # orm
   user = User.objects.get(pk=101)
   user.last_name = '김'
   user.save()
   ```

      ```sql
   -- sql
   UPDATE users_user
   SET last_name='김'
   WHERE id=101;
      ```

5. 해당 user 레코드 삭제

      ```python
   # orm
   User.objects.get(pk=101).delete()
   
   # user = User.objects.get(pk=101)
   # user.delete()
   ```
   
   ```sql
   -- sql
   DELETE FROM users_user
   WHERE id=101;
   ```

### 조건에 따른 쿼리문

1. 전체 인원수 

   ```python
   # orm
   User.objects.all().count()
   ```

      ```sql
   -- sql
   SELECT COUNT(*) FROM users_user;
      ```

2. 나이가 30인 사람의 이름

   ```python
   # orm
   User.objects.filter(age=30).values('first_name')
   # <QuerySet [{'first_name': '영환'}, {'first_name': '보람'}, {'first_name': '은영'}]>
   print(User.objects.filter(age=30).values('first_name').query)
   # SELECT "users_user"."first_name" FROM "users_user" WHERE "users_user"."age" = 30
   ```

      ```sql
   -- sql
   SELECT first_name FROM users_user
   WHERE age=30;
      ```

3. 나이가 30이상인 사람의 인원 수

   > __gte : 이상
   >
   > __gt : 초과
   >
   > __lte : 이하
   >
   > __lt : 미만

   ```python
   # orm
   User.objects.filter(age__gte=30).count()
   print(User.objects.filter(age__gte=30).query)
   # query는 queryset의 인스턴스 변수로 존재함.
   # SELECT "users_user"."id", "users_user"."first_name", "users_user"."last_name", "users_user"."age", "users_user"."country", "users_user"."phone", "users_user"."balance" FROM "users_user" WHERE "users_user"."age" >= 30
   ```

      ```sql
   -- sql
   SELECT COUNT(*) FROM users_user
   WHERE age >= 30;
      ```

4. 나이가 30이면서 성이 김씨인 사람의 인원 수

   ```python
   # orm
   User.objects.filter(age=30, last_name='김').count()
   # User.objects.filter(age=30).filter(last_name='김')
   print(User.objects.filter(age=30).filter(last_name='김').query)
   SELECT "users_user"."id", "users_user"."first_name", "users_user"."last_name", "users_user"."age", "users_user"."country", "users_user"."phone", "users_user"."balance" FROM "users_user" WHERE ("users_user"."age" = 30 AND "users_user"."last_name" = 김)
   ```

   ```sql
   -- sql
   SELECT COUNT(*) FROM users_user
   WHERE age=30 AND last_name='김';
   ```

5. 지역번호가 02인 사람의 인원 수

   > LIKE
   >
   > exact, contains, startswith, endswith
   >
   > iexact, icontains, istartswith, iendswith
   >
   > i -> case insensitive (대소문자 무시)

   ```python
   # orm
   User.objects.filter(phone__startswith='02-').count()
   ```

      ```sql
   -- sql
   SELECT COUNT(*) FROM users_user
   WHERE phone LIKE '02-%';
      ```

6. 거주 지역이 강원도이면서 성이 황씨인 사람의 이름

   ```python
   # orm
   User.objects.filter(country='강원도', last_name='황').values('first_name')
   ```

      ```sql
   -- sql
   SELECT first_name FROM users_user
   WHERE last_name='황' AND country='강원도';
      ```



### 정렬 및 LIMIT, OFFSET

1. 나이가 많은 사람 10명 

   ```python
   # orm
   # age를 내림차순으로 정렬해서 10개
   User.objects.order_by('-age')[:10]
   print(User.objects.order_by('-age')[:10].query)
   # SELECT "users_user"."id", "users_user"."first_name", "users_user"."last_name", "users_user"."age", "users_user"."country", "users_user"."phone", "users_user"."balance" FROM "users_user" ORDER BY "users_user"."age" DESC  LIMIT 10
   ```

      ```sql
   -- sql
   SELECT * FROM users_user
   ORDER BY age DESC LIMIT 10;
      ```

2. 잔액이 적은 사람 10명

   ```python
   # orm
   # balance를 오름차순
   User.objects.order_by('balance')[:10]
   ```

      ```sql
   -- sql
   SELECT * FROM users_user
   ORDER BY balance ASC LIMIT 10;
      ```

3. 성, 이름 내림차순 순으로 5번째 있는 사람

      ```python
   # orm
   User.objects.order_by('-last_name', '-first_name')[4]
   ```

   
   ```sql
   -- sql
   SELECT * FROM users_user
   ORDER BY last_name DESC, first_name DESC LIMIT 1 OFFSET 4;
   ```

### 표현식

* [관련 문서](https://docs.djangoproject.com/en/2.2/topics/db/aggregation/)

1. 전체 평균 나이

   ```python
   # orm
   from django.db.models import Avg
   User.objects.aggregate(Avg('age'))
   # {'age__avg': 28.23}
   ```

      ```sql
   -- sql
   SELECT AVG(age) FROM users_user;
      ```

2. 김씨의 평균 나이

   ```python
   # orm
   from django.db.models import Avg
   User.objects.filter(last_name='김').aggregate(Avg('age'))
   ```

      ```sql
   -- sql
   SELECT AVG(age) FROM users_user
   WHERE last_name='김';
      ```

3. 계좌 잔액이 가장 높은 값

   ```python
   # orm
   from django.db.models import Max
   User.objects.aggregate(Max('balance'))
   ```

      ```sql
   -- sql
   SELECT MAX(balance) FROM users_user;
      ```

4. 계좌 잔액 총액

   ```python
   # orm
   from django.db.models import Sum
   User.objects.aggregate(Sum('balance'))
   ```
   
   ```sql
   -- sql
   SELECT SUM(balance) FROM users_user;
   ```