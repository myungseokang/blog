---
layout: post
title: "django 1.6에서 1.10(1.11)로 마이그레이션 할 때 막혔던 부분 정리"
categories: posts
excerpt: "Cleanup when I migrated from django 1.6 to 1.10 (1.11)"
tags: [django]
author: leop0ld
comments: true
share: true
ads: true
date: 2016-09-28
---

기존에 Django 1.6 버전으로 돌아가던 프로젝트를 보안 문제도 있고, 라이브러리의 호환성 및 이러저러한 이유로 1.10로 넘어간 뒤에 1.11로 마이그레이션을 하는 작업을 생각중에만 있다가 오늘 실행에 옮겼습니다.
생각보다 크나큰 작업이 될 것 같아서 마음을 편하게 먹고 작업에 임했습니다.
작업은 생각보다 오래 걸리지 않았습니다.(물론 전부 다 제대로 돌아가려면 시간이 조금 필요할 것 같습니다만, 실행하는데에는 시간이 그리 크게 들지 않았습니다.)
일단 문제가 총 3개 정도 있었습니다.

## 1. South to Migration

Django 1.6에서는 syncdb 라는 명령어로 South 라이브러리를 사용하고 있었습니다.
하지만 Django가 1.7 버전으로 올라가면서 이것을 내장하고 migration 파일을 사용하도록 바뀌었습니다.

따라서 이 변화를 따라가야했습니다.
자세한 사항은 [링크](https://leop0ld.github.io/posts/south-to-migration)을 참고해주시면 감사하겠습니다.

## 2. Settings

settings 부분도 많이 바뀌었습니다.
Django 1.6에서는 ```TEMPLATE_DEBUG = DEBUG``` 이런 식으로 관리했으나 
Django 1.10에서는 ```TEMPLATE = {...}``` 이런 식으로 Dict 형태로 관리해서 이 부분또한 변경해주어야 했습니다.

<script async src="//pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-format="fluid"
     data-ad-layout="in-article"
     data-ad-client="ca-pub-1864899826477546"
     data-ad-slot="2703362319"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

## 3. 그 밖의 외부 라이브러리

그 밖에 Django에 붙여서 사용하던 외부 라이브러리들 중에도 에러를 뱉는 부분이 있어서 그 부분또한 봐줬어야 했습니다.
대표적으로 django-allauth 라는 소셜 로그인과 관련된 라이브러리 였습니다.
이 부분은 allauth의 doc을 보고 했습니다. [링크](http://django-allauth.readthedocs.io/en/latest/installation.html)

Django 1.8+ 일때 하는 부분으로 설정해주었습니다.

이 부분은 settings 부분이랑 맞물리는 부분이다보니 두 개를 잘 섞어서 해주어야 했습니다.
제 프로젝트 같은 경우 다음과 같이 설정을 해주었습니다.

settings/base.py의 일부

```python
INSTALLED_APPS = [
	...
	'allauth',
    'allauth.account',
    'allauth.socialaccount',
    'allauth.socialaccount.providers.facebook',
]

from django.conf.global_settings import AUTHENTICATION_BACKENDS as AB
AUTHENTICATION_BACKENDS = AB + [
    # Needed to login by username in Django admin, regardless of `allauth`
    'django.contrib.auth.backends.ModelBackend',

    # `allauth` specific authentication methods, such as login by e-mail
    'allauth.account.auth_backends.AuthenticationBackend',
]

TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                # `allauth` needs this from django
                'django.template.context_processors.request',
                'django.template.context_processors.debug',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
```

이렇게 써주었습니다.(django-allauth의 Document에 있는 내용 거의 그대로 사용했습니다.)
제 프로젝트에서는 facebook 로그인만 사용하기에 providers.facebook 만 추가를 했지만, 문서를 보니 굉장히 많은 사이트를 추가할 수 있게 되어있었습니다.
문서를 잘보고 원하시는 사이트를 INSTALLED_APPS에 추가해주시면 될 것 같습니다.
