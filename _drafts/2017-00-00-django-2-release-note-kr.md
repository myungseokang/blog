---
layout: post
title: "Django 2.0 Release Notes 한글버전"
date: 2017-00-00
categories: django
author: leop0ld
---

Django 2.0 버전이 드디어 릴리즈 되었습니다.
그래서 어떤 변경사항이 있는지 볼겸해서 번역을 해보았습니다.
아래부터는 내용입니다. 잘못된 부분이 있으면 코멘트 남겨주세요.
감사합니다 :)

## Django 2.0 릴리즈 노트

이 릴리즈 노트는 Django 1.11 또는 이전 버전에서 업그레이드 할 때 알아두어야 할 하위 호환되지 않는 몇 가지 변경 사항과 새로운 기능을 포함합니다.
지원 중단 주기가 끝난 기능을 일부 삭제했으며 일부 기능에 대한 지원 중단 절차가 시작되었습니다.

이 릴리즈는 Django가 느슨한 형식의 의미적 버전 관리(loose form of semantic versioning) 를 사용하기 시작하지만, 2.0 릴리즈에서 예상할 수 있는 호환되지 않는 주된 변경 사항은 없습니다.
업그레이드는 이전 기능 릴리즈와 비슷한 수준입니다.

기존 프로젝트를 업데이트하는 경우 Django를 [업그레이드 가이드](https://docs.djangoproject.com/en/stable/howto/upgrade-version/) 를 참조하세요.

## Python 호환성

Django 2.0은 Python 3.4, 3.5 및 3.6을 지원합니다. Python 각 시리즈의 최신 버전을 공식적으로 지원할 것을 **적극 권장**합니다.

Django 1.11.x 시리즈는 Python 2.7을 지원하는 마지막 버전입니다. Django 2.0 은 Python 3.4를 지원하는 마지막 릴리즈입니다.

Django 2.0 (~2019년 4월) 의 수명이 종료될 때까지 Python 3.4를 배포하려면 Django 1.11 LTS (~2020년 4월)를 사용하세요. 그러나 Python 3.4의 수명은 2019년 3월입니다.

## 이전 버전의 Django에 대한 Third-party 라이브러리 지원

Django 2.0이 출시된 후 Third-party 앱 작성자는 Django 1.11 이전의 모든 버전에 대한 지원을 중단할 것을 제안합니다. 그 때 `python -Wd` 를 사용하여 패키지 테스트를 실행하면 deprecation 경고가 나타납니다. Deprecation 경고 수정을 한 후에는 앱이 Django 2.0과 호환되어야 합니다.

## What’s new in Django 2.0

### 단순화된 URL 라우팅 문법

새로운 django.urls.path() 함수는 더 간단하고 읽기 쉬운 URL 라우팅 문법을 허용합니다. 예를 들어 이전 Django 릴리즈의 예제는 다음과 같이 작성할 수 있습니다.

```
url(r'^articles/(?P<year>[0-9]{4})/$', views.year_archive),
```

다음과 같이 작성할 수 있습니다.

```
path('articles/<int:year>/', views.year_archive),
```

새로운 문법은 URL 매개 변수의 타입을 강제합니다. 이 예제에서 View 는 year 인자를 문자열이 아닌 정수로 받습니다.
또한 아래 예제에서는 일치시킬 URL의 제약이 약간 적습니다. 예를 들어 연도 10000 은 이제 정규 표현식에 있는 그대로 정수가 정확히 4자리 길이로 제한되지 않기 때문에 일치합니다.

이전 버전의 django.conf.urls.url() 함수는 이제 django.urls.re\_path()로 사용할 수 있습니다. 이전 위치는 하위 호환성을 위해 남아 있으며 지원 중단이 급하지 않습니다. 이전 django.conf.urls.include() 함수는 이제 django.urls에서 가져올 수 있으므로 URLconfs 에서 `from django.urls import include, path, re_path` 이렇게 사용할 수 있습니다.

URL dispatcher 문서가 새로운 문법에 맞게 다시 작성되어 자세한 내용을 제공합니다.

### 모바일 친화적인 contrib.admin

관리자 페이지는 이제 반응형이며, 모든 주요 모바일 장치를 지원합니다. 이전 버전의 브라우저는 다양한 수준 저하를 경험할 수 있습니다.

### Window 표현식

새로운 Window 표현식을 사용하면 쿼리셋에 OVER 절을 추가할 수 있습니다. 표현식에서 window 함수와 aggregate 함수를 사용할 수 있습니다.

### **Minor features**

### django.contrib.admin

- 새로운 ModelAdmin.autocomplete\_fields 속성과 ModelAdmin.get\_autocomplete\_fields() 메소드를 사용하면 ForeignKey 및 ManyToManyField에 대해 Select2 검색 위젯을 사용할 수 있습니다.

### django.contrib.auth

- PBKDF2 password hasher 의 기본 반복 횟수가 36,000에서 100,000으로 증가합니다.

### django.contrib.gis

### django.contrib.postgres

### django.contrib.sitemaps

### Cache

### File Storage

### Forms

### Generic Views

### Management Commands

### Migrations

### Models

### Pagination

### Requests and Responses

### Templates

### Tests

### Validators

## 이전 버전과 호환되지 않는 2.0 의 변경 사항

### 일부 장소에서 bytestring 지원이 제거되었습니다.

Python 2 문자열을 지원하기 위해 이전 Django 버전은 바이트 문자열과 유니코드 문자열을 모두 받아들여야 했습니다.
이제 Python 2 지원이 중단되었으므로 입력/출력 한계(Ex. 바이너리 필드 핸들링 또는 HTTP 스트림 처리)에서만 bytestrings 이 발생합니다.
Django는 특정 코드에서 bytestrings 를 더 이상 받아들이지 않으므로 bytestrings 의 사용을 최소한으로 제한하도록 코드를 업데이트해야 할 수도 있습니다.

### Database backend API

이 섹션에서는 Third-Party 데이터베이스 백엔드에서 필요할 수 있는 변경 사항에 대해 설명합니다.

### Dropped support for Oracle 11.2

Oracle 11.2의 업스트림 지원 종료일은 2020년 12월입니다. Django 1.11은 2020년 4월까지 지원될 예정이며 거의 이 날짜에 도달합니다. Django 2.0은 공식적으로 Oracle 12.1+ 를 지원합니다.

### Default MySQL isolation level is read committed

### AbstractUser.last\_name max\_length 150자로 증가

`django.contrib.auth.models.User.last_name` 에 대한 마이그레이션이 포함되어 있습니다. AbstractUser 에서 상속한 커스텀 사용자 모델이 있는 경우, 사용자 모델에 대한 데이터베이스 마이그레이션을 생성하고 적용해야 합니다.

last\_name 에 30자 제한을 유지하려면 커스텀 폼을 사용하세요.

```
from django.contrib.auth.forms import UserChangeForm

class MyUserChangeForm(UserChangeForm):
    last_name = forms.CharField(max_length=30, required=False)
```

사용자를 수정할 때 관리자가 이 제한 사항을 유지하려면 `UserAdmin.form` 이 해당 양식을 사용하도록 설정하세요.

```
from django.contrib.auth.admin import UserAdmin
from django.contrib.auth.models import User

class MyUserAdmin(UserAdmin):
    form = MyUserChangeForm

admin.site.unregister(User)
admin.site.register(User, MyUserAdmin)
```

### 슬라이싱 후 QuerySet.reverse() and last() 가 금지되었습니다

슬라이스 된 쿼리셋에서 QuerySet.reverse() 또는 last() 를 호출하면 재정렬 후 슬라이스가 적용되므로 예기치 않은 결과가 발생합니다.
지금은 금지되었습니다.

```
>>> Model.objects.all()[:2].reverse()
Traceback (most recent call last):
...
TypeError: Cannot reverse a query once a slice has been taken.
```

### Form fields 는 더 이상 optional 인수를 positional 인수로 사용할 수 없습니다

Form fields 인수의 잘못된 순서로 인한 런타임 오류를 방지하기 위해 내장 Form fields 의 optional 인수를 더 이상 positional 인수로 사용할 수 없습니다.

```
forms.IntegerField(25, 10)
```

다음으로 대체해야합니다.

```
forms.IntegerField(max_value=25, min_value=10)
```

### call_command() 는 받은 옵션의 유효성을 검사합니다

`call_command()` 는 이제 호출중인 명령의 인수 파서가 `call_command()` 에 전달된 모든 옵션을 정의하는지 확인합니다.

`parser.add_argument()` 를 사용하여 생성되지 않은 옵션을 사용하는 custom management commands 의 경우, stealth\_options 속성을 추가하세요.

```
class MyCommand(BaseCommand):
    stealth_options = ('option_name', ...)
```

### Indexes no longer accept positional arguments

### Foreign key constraints are now enabled on SQLite

### Miscellaneous

## Features deprecated in 2.0

### context argument of Field.from_db_value() and Expression.convert_value()

### Miscellaneous

## Features removed in 2.0

다음 기능들은 지원 중단 주기가 끝났으며 Django 2.0에서 제거되었습니다. 이러한 기능 사용을 제거하는 방법을 비롯하여 이러한 변경 사항에 대한 자세한 내용은 [1.9에서 더 이상 사용되지 않는 기능](https://docs.djangoproject.com/en/2.0/releases/1.9/#deprecated-features-1-9)을 참조하세요.
