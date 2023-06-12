---
title: "[Django] dj-rest-auth를 활용한 JWT 회원가입/로그인 구현"
author: JIHWAN PARK
categories: [Backend, Django]
tag: [Backend, Django]
math: true
date: 2023-06-12 14:50:52 +0900
last_modified_at: 2023-06-12 14:50:52 +0900
---
> Django `dj-rest-auth`를 활용한 JWT 회원가입/로그인 구현
{: .prompt-info}

## 😂 서론
직접 `views.py`와 `serializers.py`를 작성하면서 `simplejwt`를 사용해서 회원가입/로그인/로그아웃 등을 구현해봤었다. 검색하면서 구현해보다가 관련 기능을 제공하는 것들이 있는 것 같은데, 그 때는 애초에 로그인 API 구현을 처음해보고 jwt token을 처음 접해서 그런지 이해가 안갔다. 계속해서 해보다보디 조금씩 이해가 되면서 `dj-rest-auth`라는 아주 간편한 도구가 있어서 사용했다.

## ✅ 구현해봅시다
Django에서는 로그인 할 때 기본적으로 `username`을 사용하는데 나는 `email`을 사용할 것이고 추가적인 필드가 필요했다. 그래서 `BaseUserManager`와 `AbstractUser`를 사용해서 `user` 테이블을 커스텀 했다.

먼저 회원관련 API를 제공할 `accounts`라는 앱을 생성하고 `settings.py`에 추가해준다.
> `settings.py`에 아래 코드를 추가해야한다. 추가하지 않으면 인식 못함
> `AUTH_USER_MODEL = '{앱이름}.{클래스명}'` => `AUTH_USER_MODEL = 'accounts.User'`
{: .prompt-tip}

### ✔️ `accounts/managers.py`
`accounts` 폴더 안에 `managers.py`를 생성하고 아래와 같이 작성한다.

```python
from django.contrib.auth.base_user import BaseUserManager
from django.utils.translation import gettext_lazy as _

class UserManager(BaseUserManager):
    def create_user(self, email, password, **extra_fields):
        if not email:
            raise ValueError(_('Users must have an email address'))
        email = self.normalize_email(email)
        user = self.model(email=email, **extra_fields)
        user.set_password(password)
        user.save()
        return user

    def create_superuser(self, email, password, **extra_fields):
        superuser = self.create_user(
            email=email,
            password=password,
        )
        superuser.is_staff = True
        superuser.is_superuser = True
        superuser.is_active = True
        superuser.is_admin = True
        superuser.save(using=self._db)
        return superuser
```

### ✔️ `accounts/models.py`
`accounts/models.py`로 와서 아래와 같이 작성한다.

```python
from django.db import models
from django.contrib.auth.models import AbstractUser
from django.utils.translation import gettext_lazy as _

from .managers import UserManager

class User(AbstractUser):
    username = None
    email = models.EmailField(_('email address'), unique=True, null=False, blank=False)
    # 추가적으로 넣을 field들 작성
    hospitalID = models.ForeignKey("Hospital", on_delete=models.CASCADE, blank=True, null=True, db_column="hospitalID")
    
    USERNAME_FIELD = 'email' # username field를 email로 사용
    REQUIRED_FIELDS = []

    # 헬퍼 클래스
    objects = UserManager()
    
    @property
    def name(self):
        return f"{self.first_name} {self.last_name}"
    
    def __str__(self):
        return self.email
    class Meta:
        db_table = 'user' # 테이블 이름 설정
```

### ✔️ `accounts/urls.py`
메인 `urls.py`에서 연결시키는건 잊지 않았쥬? 
```python
urlpatterns = [
    ...
    path('accounts/',include('accounts.urls')),
    ...
]
```

**`accounts/urls.py`**

```python
from django.urls import path, include

urlpatterns = [
    ...
    # 일반 회원 회원가입/로그인
    path('', include('dj_rest_auth.urls')),
    path('registration/', include('dj_rest_auth.registration.urls')),
    ...
]
```

### ✔️ 라이브러리
```
# 라이브러리 설치
pip install djangorestframework djangorestframework-simplejwt dj-rest-auth django-allauth 
```
- `djangorestframework` : Django를 REST API 형태로 사용할 수 있도록 함
- `djangorestframework-simplejwt` : Django에서 JWT Token을 사용하도록 함
    - 근데 `dj-rest-auth`에서 제공을 하는건지 안써도 되는 것 같기도하고..
- `dj-rest-auth` : 로그인, 로그아웃 등의 기능을 REST API 형태로 제공
    - 얘가 계속 업데이트 되는지, 기존 다른 분들의 블로그 글에서 작성한 파라미터 값으로는 에러가 났었음. 공식 문서를 참고해야 함
- `allauth` : 회원가입 기능을 제공

**settings.py**에 새로 설치한 앱 추가 및 변수 세팅
- 여기서 부터는 공식문서를 참고하는게 좋다.
- `simplejwt`는 안써도 되는 것 같기도 한데 잘 모르겠다..

```python
INSTALLED_APPS = [
    ...
    # 설치한 라이브러리
    'rest_framework',
    # 'rest_framework_simplejwt',
    'rest_framework.authtoken',
    'dj_rest_auth',
    'dj_rest_auth.registration',
    'allauth',
    'allauth.account',
    
    # Locals Apps: 생성한 app
    "accounts",
]

# default AUTH_USER 
AUTH_USER_MODEL = 'accounts.User'

# dj-rest-auth
REST_AUTH = {
    'USER_DETAILS_SERIALIZER': 'dj_rest_auth.serializers.UserDetailsSerializer',
    'USE_JWT': True,
    'JWT_AUTH_COOKIE': 'my-app-auth',
    'JWT_AUTH_REFRESH_COOKIE': 'my-refresh-token',
    'JWT_AUTH_HTTPONLY' : False,
}

# allauth
SITE_ID = 1
ACCOUNT_USER_MODEL_USERNAME_FIELD = None # username 필드 사용 x
ACCOUNT_EMAIL_REQUIRED = True            # email 필드 사용 o
ACCOUNT_USERNAME_REQUIRED = False        # username 필드 사용 x
ACCOUNT_AUTHENTICATION_METHOD = 'email'

ACCOUNT_EMAIL_VERIFICATION = 'none' # 회원가입 과정에서 이메일 인증 사용 X
OLD_PASSWORD_FIELD_ENABLED = True # 비밀번호 변경 시 예전 비밀번호 입력


# REST_FRAMEWORK settings:
REST_FRAMEWORK = {
    'DEFAULT_PERMISSION_CLASSES': [
        'rest_framework.permissions.IsAuthenticated',
        'rest_framework.permissions.IsAdminUser',
    ],
     'DEFAULT_AUTHENTICATION_CLASSES': [
        'dj_rest_auth.jwt_auth.JWTCookieAuthentication',
    ]
}

# SIMPLE_JWT = {
#     'ACCESS_TOKEN_LIFETIME': timedelta(minutes=30), 
#     'REFRESH_TOKEN_LIFETIME': timedelta(days=7),
#     'ROTATE_REFRESH_TOKENS': False, # True일 경우: TokenRefeshView 에 retresh token을 보내면 새로운 access/refresh token 발급
#     'BLACKLIST_AFTER_ROTATION': False, # True 일 시: 새로 고침 토큰 이 블랙리스트에 추가
#     'UPDATE_LAST_LOGIN': False, # True 일 시: User 테이블의 last_login 필드가 로그인 시 업데이트

#     'ALGORITHM': 'HS256',
#     'SIGNING_KEY': SECRET_KEY,
#     'VERIFYING_KEY': None,
#     'AUDIENCE': None,
#     'ISSUER': None, # host site의 도메인
#     'JWK_URL': None,
#     'LEEWAY': 0,

#     'AUTH_HEADER_TYPES': ('Bearer',),
#     'AUTH_HEADER_NAME': 'HTTP_AUTHORIZATION', # HTTP_  뒤 부분이 token 헤더 이름이 됨
#     'USER_ID_FIELD': 'id',
#     'USER_ID_CLAIM': 'user_id',
#     'USER_AUTHENTICATION_RULE': 'rest_framework_simplejwt.authentication.default_user_authentication_rule',

#     'AUTH_TOKEN_CLASSES': ('rest_framework_simplejwt.tokens.AccessToken',),
#     'TOKEN_TYPE_CLAIM': 'token_type',
#     'TOKEN_USER_CLASS': 'rest_framework_simplejwt.models.TokenUser',

#     'JTI_CLAIM': 'jti',

#     'SLIDING_TOKEN_REFRESH_EXP_CLAIM': 'refresh_exp',
#     'SLIDING_TOKEN_LIFETIME': timedelta(minutes=5),
#     'SLIDING_TOKEN_REFRESH_LIFETIME': timedelta(days=1),
# }
```

## ✨ API 테스트
POSTMAN으로 테스트 해보면 잘 되는걸 확인할 수 있다.

### ✔️ 회원가입
![회원가입](https://github.com/Jihwan98/Jihwan98.github.io/assets/76936390/c8df5c3b-9763-45f7-8752-683fd86e86a2)

### ✔️ 로그인
![로그인](https://github.com/Jihwan98/Jihwan98.github.io/assets/76936390/6e1a5a4c-df3c-4d70-be8a-4a5f774346bd)

### ✔️ 회원정보 가져오기
![회원정보 가져오기](https://github.com/Jihwan98/Jihwan98.github.io/assets/76936390/a68ca7a8-cf5d-4b0e-8417-27400cc4afe2)

### ✔️ 비밀번호 변경
![비밀번호 변경](https://github.com/Jihwan98/Jihwan98.github.io/assets/76936390/d05510aa-f322-4d76-ae92-674d265a57bf)

### ✔️ 로그아웃
![로그아웃](https://github.com/Jihwan98/Jihwan98.github.io/assets/76936390/3c959516-67fb-49eb-96d8-fffee480a2cb)

### ✔️ AccessToken 재발급
![AccessToken 재발급](https://github.com/Jihwan98/Jihwan98.github.io/assets/76936390/f9c0be21-1317-4225-819d-c4e44b2677d2)

