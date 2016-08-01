---
layout: post
title:  "Django REST Framework 알아보기"
date:   2016-07-06
excerpt: "Keep Calm and Code Python."
tag:
- Python
- Django Rest Framework
comments: true
---

### 1. DRF(Django REST Framework) 설치

```shell
pyenv virtualenv drf_test
pyenv activate drf_test
pip install django
pip install djangorestframework
pip install django-rest-swagger
```

새로운 virtualenv를 생성한 다음 activate로 활성화 시켜줍니다.
그 다음 DRF를 사용하기 위한 모듈을 pip install로 설치해줍니다.

### 2. Django settings.py 수정

INSTALLED_APPS에 rest_framework, rest_framework_swagger를 추가해줍니다.

### 3. Django REST Swagger View 추가

[Django REST Swagger Document 바로가기](http://marcgibbons.github.io/django-rest-swagger/)

1. Swagger를 위한 View를 생성해줍니다.
2. SwaggerURIRenderer, OpenAPIRenderer 2개의 렌더러(Renderer)를 데코레이터(Decorator 혹은 장식자)로 꾸며줍니다.
3. 해당 View 를 urls.py에 등록 후, 등록한 URL로 접근합니다.

### 4. urls.py에 추가

urls.py에 view 이름을 추가해줍니다.

### 5. 실행 후 확인

python manage.py runserver 를 해주고 확인해줍니다.
[http://127.0.0.1:8000/](http://127.0.0.1:8000)
