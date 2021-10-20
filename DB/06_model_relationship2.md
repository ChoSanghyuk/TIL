# Model Relationship2



## M:N 관계



## 중개 테이블 작성

- 다대다 관계의 테이블을 연결하는 중개 테이블 생성

```python
from django.db import models

class Doctor(models.Model):
    name = models.TextField()

    def __str__(self):
        return f'{self.pk}번 의사 {self.name}'


class Patient(models.Model):
    name = models.TextField()
    # 외래키 삭제

    def __str__(self):
        return f'{self.pk}번 환자 {self.name}'

# 중개모델 작성
class Reservation(models.Model):
    doctor = models.ForeignKey(Doctor, on_delete=models.CASCADE)
    patient = models.ForeignKey(Patient, on_delete=models.CASCADE)

    def __str__(self):
        return f'{self.doctor_id}번 의사의 {self.patient_id}번 환자'
```

- 의사와 환자의 다대다 관계 생성시 Reservation 객체를 생성.

  => 조회 또한 1:N 관계처럼 접근
  
- 중개 테이블에 추가 데이터를 사용해 다대다 관계로 연결하려는 경우에 사용

- trough 옵션을 사용하여, 중개 테이블을 나타내는 Django 모델을 지정할 수 있음



### ManyToManyField

- 다대다 관계 설정 시 사용하는 모델 필드
- 하나의 필수 위치인자(M:N 관계로 설정할 모델 클래스)가 필요

```python
from django.db import models

class Doctor(models.Model):
    name = models.TextField()

    def __str__(self):
        return f'{self.pk}번 의사 {self.name}'


class Patient(models.Model):
    name = models.TextField()
    # ManyToManyField 작성
    doctors = models.ManyToManyField(Doctor, through='Reservation')

    def __str__(self):
        return f'{self.pk}번 환자 {self.name}'
```

- django에서 자동으로 중개 테이블 생성
- 테이블 :  `앱이름_소스테이블_타켓테이블` (hospitals_patient_doctors)
- 필드 이름 :` id` , `< containing_model >_id`, `< other_model >_id`
- 다대다 관계 
  - 생성 : `patient1.doctors.add(doctor1)`
  - 조회 : `patient1.doctors.all()`
  - 제거 : `patient1.doctors.remove(doctor1)`
  - 역참조 생성 : `doctor1.patient_set.add(patient1)`
  - 역참조 조회 : `doctor1.patient_set.all()`
  - 역참조 제거 : `doctor1.patient_set.remove(patient1)` 



#### related_name 필드

- target model (관계 필드를 가지지 x 모델)이 source model(관계 필드를 가진 모델)을 참조할 때 사용할 manager의 이름을 설정
- 즉, 역참조 시에 사용하는 manager의 이름 설정 ( ForeignKey의 related_name과 동일)



#### through

- 중개 테이블을 직접 작성하는 경우, through 옵션을 사용하여 중개 테이블을 나타내는 Django 모델을 지정

```python
from django.db import models

class Doctor(models.Model):
    name = models.TextField()

    def __str__(self):
        return f'{self.pk}번 의사 {self.name}'

class Patient(models.Model):
    doctors = models.ManyToManyField(Doctor, through='Reservation')
    name = models.TextField()

    def __str__(self):
        return f'{self.pk}번 환자 {self.name}'

class Reservation(models.Model):
    doctor = models.ForeignKey(Doctor, on_delete=models.CASCADE)
    patient = models.ForeignKey(Patient, on_delete=models.CASCADE)
    symptom = models.TextField()
    reserved_at = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return f'{self.doctor.pk}번 의사의 {self.patient.pk}번 환자'
```

- 환자와 의사는  M:N때 처럼 참조, 역참조 가능

- through_default를 사용해 add(), create() 또는 set()을 사용하여 관계 생성

  `patient1.doctors.add(doctors1, through_default={'symtom' : 'flu'})`

#### symmetrical

- ManyToManyField가 동일한 모델(on self)을 가르키는 정의에서만 사용
- symmetrical= True(기본값)일 경우, django는 person_set 매니저를 추가하지 않음
  - source 모델의 인스턴스가 target 모델의 인스턴스를 참조하는 경우, target 모델의 인스턴스도 해당 source 모델의 인스턴스를 자동으로 참조하게 됨



#### 중개 테이블의 필드 생성 규칙

- source model 및 target model이 다른 경우
  - id
  - < containing model>_id
  - < other model >_id
- ManyToManyField가 동일한 모델을 가르키는 경우
  - id
  - from_ < model >_ id
  - to_ < model >_ id





## Like



### Like 구현하기

```python
# Create your models here.
class Article(models.Model):
    user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    like_users = models.ManyToManyField(settings.AUTH_USER_MODEL, related_name='like_articles')
    title = models.CharField(max_length=10)
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

    def __str__(self):
        return self.title
```

- User을 참조할 때에는 직접 참조하지 않고, **settings.AUTH_USER_MODEL**을 통해서 현재 프로젝트의 User 클래스를 참조
- 위의 경우 1:N 관계와 M:N 관계에서의 model manager 이름이 겹치기 때문에 `related_name`필수



### QuerySet API - 'exists()'

- QuerySet에 결과가 포함되어 있으면 True를 반환하고 그렇지 않으면 False 반환
- 규모가 큰 QuerySet에 컨텍스트에서 특정 개체 존재 여부와 관련된 검색에 유용
- 고유한 필드가 있는 모델이 QuerySet의 구성원인지 여부를 찾는 가장 효율적인 방법
- `article.like_users.filter(pk=request.user.pk).exists()`



## Follow

```python
# accounts/models.py
class User(AbstractUser):
    followings = models.ManyToManyField('self', symmetrical=False, related_name='followers')
```















