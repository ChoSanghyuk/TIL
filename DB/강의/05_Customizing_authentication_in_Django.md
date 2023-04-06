# Customizing authentication in Django



## Substituting a custom User model



### User 모델 대체하기

- 일부 프로젝트에서는 Django 내장 User 모델이 제공하는 인증 요구사항이 적절하지 않음
- Django는 User를 참조하는데 사용하는 **AUTH_USER_MODEL** 값을 제공하여, default user model을 `재정의` 할 수 있도록 함
- 단, 프로젝트의 모든 migrations 혹은 `첫 migrate를 실행하기 전`에 이 작업을 마쳐야 함
- 새 프로젝트를 시작하는 경우, 커스텀 유저 모델을 설정하는 것을 강력 권장 



### AUTH_USER_MODEL

- User를 나타내는데 사용하는 모델

- 프로젝트가 진행되는 동안 변경할 수 없음

- 기본 값 : `auth.User`

- [참고] 프로젝트 중간에 AUTH_USER_MODEL 변경

  - 모델 관계에 영향을 미치기 때문에 훨씬 더 어려운 작업

    => 중간 변경 권장 x

### *Custom User 모델 정의하기 

1. 관리자 권한과 함계 완전한 기능을 갖춘 User 모델을 구현하는 기본 클래스인 AbstractUser를 상속받아 새로운 User 모델 작성

```python
# accounts/models.py

from django.contrib.auth.models import AbstractUser

# Create your models here.
class User(AbstractUser):
    pass    
```

2. 기존에 django가 사용하는 User 모델이었던 auth 앱의 User 모델을 accounts 앱의 User 모델을 사용하도록 변경

```python
# settings.py
AUTH_USER_MODEL = 'accounts.User'
```

3. admin site에 Custom User 모델 등록

```python
# accounts/admin.py

from django.contrib import admin
from django.contrib.auth.admin import UserAdmin
from .models import User
# Register your models here.

admin.site.register(User, UserAdmin)
```

- 중간 변경 방법 : db 초기화 한 후에 마이그레이션 진행
  - db.sqlite3 파일 삭제
  - migrations 파일 모두 삭제 (파일명에 숫자가 붙은 파일만 삭제)



## Customer user & Built-in auth forms



### Customer User 설정 회원 가입 에러

- UserCreationForm과 UserChangeForm은 기존 내장 User 모델을 사용한 ModelForm 이기 깨문에 커스텀 User 모델로 대체 필요



### Custom Built-in Auth Forms

- 커스텀 User 모델로 다시 작성하거나 확장해야 하는 forms
  - UserCreationForm
  - UserChangeForm

```python
# accounts/forms.py

from django.contrib.auth.forms import UserChangeForm, UserCreationForm
from django.contrib.auth import get_user_model


class CustomUserChangeForm(UserChangeForm):

    class Meta:
        model = get_user_model()
        fields = ('email', 'first_name', 'last_name',)


class CustomUserCreationForm(UserCreationForm):

    class Meta(UserCreationForm.Meta):
        model = get_user_model()
        fields = UserCreationForm.Meta.fields + ('email',)
```

- UserChangeForm은 이미 이전에 customize 함.

  - 이때, get_user_model() 함수를 통해서 현재 활성화된 User 클래스를 가져옴

    => django는 User 클래스를 직접 참조 지양

