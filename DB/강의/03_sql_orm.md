# SQL with django ORM



## 기본 세팅

### db 프로그램 실행

```
$ python manage.py dbshell
```

### ORM 실행

```
$ python manage.py shell_plus --print-sql
```

​	: ORM을 실행하고, 대응되는 sql 프린트 해줌



## ORM 활용

### 전체 수

```shell
User.objects.count()
```



### 특정 열 READ & WHERE 절

```shell
User.objects.filter(age=30).values('first_name')
```



### WHERE 절 AND 체인

```shell
User.objects.filter(age__gte=30, last_name="김").count()
```



### WHERE 절 OR 체인

```shell
User.objects.filter( Q(age__gte=30) | Q(last_name="김")).count()
```



### LIKE operator

```shell
User.objects.filter(phone__startswith="02-")
```



### ORDER BY

```SHELL
User.objects.order_by('balance', '-age')[:10]
```



### Aggregate function

```shell
User.objects.aggregate(Avg('age'))
```



### GROUP BY

- Annotate()
  - '주석을 달다'라는 사전적 의미로, 특정 조건으로 계산된 값을 가진 컬럼을 하나 만들고 추가하는 개념

```shell
User.objects.values('country').annotate(Count('country'))
```

