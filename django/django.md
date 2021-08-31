# Django



## 시작하기

- 설치

```
$ pip install django
```

- 프로젝트 생성

```
$ django-admin startproject template_view
```

- app 폴더 생성

```
$ python manage.py startapp <app이름>
```

- 프로젝트 시작

```
$ python manage.py runserver
```



## 프로젝트 구조

- manage.py    : 집사. 서버를 실행함
- 마스터 앱        : 요청을 대응되는 app 폴더에 대응시킴
  - init
  - asgi
  - wsgi
  - settings   : 언어, prefix 등 프로젝트 전반에 대한 설정
  - urls          : controller. url에 요청이 들어왔을 때 처리함
- app 폴더        : 실무적인 내용을 처리함.



## 폴더 및 파일별 역할

### 마스터 앱

- urls.py

  ```python
  urlpatterns = [
      path('admin/', admin.site.urls),
      path('first_app/', include('first_app.urls')),
  ]
  ```

  - urlpatterns에다 url과 함수를 매핑시킴. 
  - 여기선 앞의 url만 확인하고 해당 url을 담당하는 app 폴더의 urls 파일에 보냄

- settings.py

  ```python
  INSTALLED_APPS = [
      ... ,
  
      'first_app',
  ]
  ```

  - 앱 폴더를 만들면 꼭 등록시켜줘야 함

### app 폴더

- 실무적인 역할을 처리하는 폴더

- urls.py

  ```python
  from . import views
  
  urlpatterns = [
      path('hello/<name>', views.hello),
      path('lunch/', views.lunch),
      path('lotto/', views.lotto)
  ]
  ```

  - 마스터에서 보낸 url 이후의 부분에 대한 매핑을 담당
  - 보통 같은 폴더의 views.py를 생성하고, 매핑되는 함수를 정의함
  - <파라미터>를 통해, url에서 변수를 받아올 수 있음

- views.py

  ```python
  # url로 들어오는 parameter을 name 변수에 담음
  def hello(request, name):
      context = {
          'name' : name
      }
      return render(request, 'first_app/hello.html', context)
  
  def lunch(request):
      menues = ['짜장면', '짜장밥', '라면']
      menu = random.choice(menues)
      
      context = {
          'menu' : menu
      }
      return render(request, 'first_app/lunch.html', context)
  ```

  - 매핑되는 함수는 html 파일을 받아와 rendering을 한 후 html 파일을 반환시킴
  - context 에는 받아오는 html 파일의 dtl 처리를 위한 변수를 담음
  - 받아오는 html 파일은 `templates` 라는 폴더 안에 담아둠

- templates

  - 처음 서버가 동작할 때, 등록된 모든 app 폴더의 templates 안에 파일들을 하나에 합침

    => 다른 app 폴더의 같은 이름의 html 파일이 있을 시 먼저 등록된 app의 html만 불러와짐

  - templates 안에 폴더를 생성하고 html 파일을 보관해야 구분가능

  - settings의 installed_apps에 등록되어 있지 않거나, templates 폴더 이외의 폴더에서 찾고 싶다면 settings -> templates -> dirs 에 등록해 놔야함



## 공동 templates

공통된 html이 존재하고 url에 따라서 그 안의 내용만 바뀌게 된다면, 공동 부분은 그대로 놔두고 요청에 따라 내용만 바꾸게 해줌

### step 1. 공동 파일 만들기

주로 root 폴더에 `templates` 폴더를 생성하고 거기에 base.html을 생성해 준다. 이때, 마스터 앱 폴더의 setting에서 templates->dir에 templates를 등록시켜준다.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <div class = "container">
        {% block content %}
        {% endblock %}
    </div>
</body>
</html>
```

html 파일에서 내용을 넣을 부분에 block을 지정한다.



### step 2. 내용 파일 만들기

해당 내용을 어떤 html 파일에 넣을 지 명시하고, 블록에 들어갈 내용을 작성해준다.

```html
{% extends 'base.html' %}
{% block content %}
    <h1>오늘의 로또 번호는!!</h1>
    {% for number in lotto %}
    <li>{{ number}}</li>
    {% endfor %}
{% endblock %}
```



## variable routing

URL 자체를 변수처럼 사용해서 동적으로 주소를 만드는 것을 의미한다. url이 하드코딩 된다면, url 이름을 변경할 때마다, 매핑된 모든 부분을 다 수정해야 하므로 변수를 설정하여 작성한다.

```python
app_name = 'first_app'

urlpatterns = [
    path('hello/<name>', views.hello, name='hello'),
    path('lunch/', views.lunch, name='lunch'),
    path('lotto/', views.lotto, name='lotto')
]
```

path() 함수에서 name을 설정하면, 해당 변수 이름에 url에 저장된다. 이때, 여러 app 폴더에서 변수명이 중첩될 경우를 방지하기 위해, app_name을 설정해 준다.



이후, html에서 해당 url에 접근할 경우가 생긴다면, 다음과 같이 {% url  '<app_name>:<url변수이름>'  %} 으로 접근할 수 있다.

```html
<a href="{% url 'first_app:lotto' %}">
```



## form 요청 처리하기

url로 form 요청이 들어오면 request에서 받을 수 있다. 예를 들어, GET 요청이 들어왔다면, request.GET에 변수와 값들이 dictionary 형태로 담겨져 있다.

```python
def pong(request):
    context = {
        'message' : request.GET.get('message'),
        'sign' : request.GET.get('sign')
    }
    return render(request, 'first_app/pong.html', context)
```

