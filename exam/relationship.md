# 1:N

```python
u1 = User.objects.create(username='Kim')
u2 = User.objects.create(username='Lee')

a1 = Article.objects.create(title='1글', user=u1)
a2 = Article.objects.create(title='2글', user=u2)
a3 = Article.objects.create(title='3글', user=u2)
a4 = Article.objects.create(title='4글', user=u2)

c1 = Comment.objects.create(content='1글1댓', article=a1, user=u2)
c2 = Comment.objects.create(content='1글2댓', article=a1, user=u2)
c3 = Comment.objects.create(content='2글1댓', article=a2, user=u1)
c4 = Comment.objects.create(content='4글1댓', article=a4, user=u1)
c5 = Comment.objects.create(content='3글1댓', article=a3, user=u2)
c6 = Comment.objects.create(content='3글2댓', article=a3, user=u1)
```

1. 모든 댓글 출력

2. 1번 사람(`u1`) 작성한 모든 게시글
3. 2번 댓글(`c2`)을 작성한 사람
4. 3번 글(`a3`)을 작성한 사람의 이름
5. 2번 글(`a2`)을 작성한 사람이 작성한 댓글들
6. 1번 글(`a1`)에 작성된 댓글 중에 첫번째를 작성한 사람의 이름
7. 1번 사람(`u1`)이 작성한 첫번째 게시글의 1, 2번째 댓글
8. 1번 사람(`u1`)이 작성한 게시글을 제목 내림차순으로 정렬





## M:N Many to many

### 1. 중개 모델

```python
class Doctor(models.Model):
    name = models.TextField()

class Patient(models.Model):
    name = models.TextField()

class Reservation(models.Model):
    doctor = models.ForeignKey(Doctor, on_delete=models.CASCADE)
    patient = models.ForeignKey(Patient, on_delete=models.CASCADE)
```

1. 예약 만들기

   ````python
   d1 = Doctor.objects.create(name='kim')
   p1 = Patient.objects.create(name='taewoo')
   Reservation.objects.create(doctor=d1, patient=p1)
   ````

2. 1번 환자의 예약 목록

   ```python
   p1.reservation_set.all()
   ```

3. 1번 의사의 예약 목록

   ```python
   d1.reservation_set.all()
   ```

4. 1번 의사의 환자 목록

   * 지금 상태에서 바로 의사가 해당하는 환자들로 접근을 할 수는 없다.

   ```python
   for r in d1.reservation_set.all():
       print(r.patient)
   ```

## 2. 중개 모델(ManyToManyField)

> 의사 -> 환자들 / 환자 -> 의사들로 접근을 하기 위해서는 `ManyToManyField`를 사용한다.
>
> Reservation 모델을 활용하려면 `through` 옵션을 사용한다. 
>
> `through` 옵션이 없으면, 기본적으로 `앱이름_patient_doctor` 라는 이름의 테이블을 생성한다.

```python
class Doctor(models.Model):
    name = models.TextField()

class Patient(models.Model):
    name = models.TextField()
    doctors = models.ManyToManyField(Doctor, through='Reservation')

class Reservation(models.Model):
    doctor = models.ForeignKey(Doctor, on_delete=models.CASCADE)
    patient = models.ForeignKey(Patient, on_delete=models.CASCADE)
```

* 마이그레이션 파일을 만들거나 마이그레이트를 할 필요가 없다!
* 즉, 데이터베이스는 전혀 변경되는 것이 없다.

1. 1번 의사의 예약 목록

   ```python
   d1.reservation_set.all()
   ```

2. 1번 의사의 환자 목록

   * `Doctor` 는 `Patient`의 역참조이므로, naming convention에 따라 아래와 같이 접근!

   ```python
   d1.patient_set.all()
   ```

3. 1번 환자의 의사 목록

   * `Patient` 는 `Doctor`는 직접 참조(`doctors`) 하므로, 아래와 같이 접근!

   ```python
   p1.doctors.all()
   ```

   

### 2.1. `related_name`

```python
class Doctor(models.Model):
    name = models.TextField()

class Patient(models.Model):
    name = models.TextField()
    doctors = models.ManyToManyField(Doctor, 
                        through='Reservation',
                        related_name='patients')

class Reservation(models.Model):
    doctor = models.ForeignKey(Doctor, on_delete=models.CASCADE)
    patient = models.ForeignKey(Patient, on_delete=models.CASCADE)
```

* 역참조시 `related_name` 옵션으로 직접 설정할 수 있다.
  * 설정하지 않으면 기본적으로 `Model명_set`으로 된다.
* 반드시 설정할 필요는 없지만, 필수적인 상황이 발생할 수 있다.
  * 예) `User` - `Article`
* 따라서, `ManyToManyField`를 쓸 때에는 항상 `related_name`을 설정하고, 모델의 복수형으로 표기하자.

1. 1번 의사의 환자 목록

   ```python
   d1.patients.all()
   ```

   

## 3. 중개모델 없이 작성

```python
class Doctor(models.Model):
    name = models.TextField()

class Patient(models.Model):
    name = models.TextField()
    doctors = models.ManyToManyField(Doctor, related_name='patients')
```

* `앱이름_patient_doctors` 로 테이블이 자동으로 생성된다.
* 별도의 컬럼이 필요 없는 경우는 위와 같이 작성한다.
* 만약, 예약시 추가정보 (예 - 시간, 담당자, ...)를 담기 위해서라면 반드시 중개 모델이 필요하다!

1. 예약 생성

   ```python
   d2 = Doctor.object.create(name='Kim')
   p2 = Patient.object.create(name='Kim')
   
   d2.patients.add(p2)
   ```

2. 예약 삭제

   ```python
   d2.patients.remove(p5)
   ```