---
layout: post
title: "Django 2.0 Release Notes 한글버전"
date: 2017-00-00
categories: django
author: leop0ld
---

Django 2.0 버전이 드디어 릴리즈 되었습니다.

그래서 영어는 잘 못하지만 다른 한국 개발자분들이 보시기 편하게 번역을 해보았습니다.

아래부터는 내용입니다. 잘 봐주세요!

## Django 2.0 릴리즈 노트

이 릴리즈 노트는 Django 1.11 또는 이전 버전에서 업그레이드 할 때 알아 두어야 할 몇 가지 하위 호환되지 않는 변경 사항과 새로운 기능을 포함합니다.
지원 중단 주기가 끝난 기능을 일부 삭제했으며 일부 기능에 대한 지원 중단 절차가 시작되었습니다.
이 릴리즈는 Django가 느슨한 형식의 의미적 버전 관리를 사용하기 시작하지만, 2.0 릴리스에서 예상할 수 있는 주요한 호환되지 않는 변경 사항은 없습니다.
업그레이드는 이전 기능 릴리스와 비슷한 수준의 노력이어야 합니다. 기존 프로젝트를 업데이트하는 경우 Django를 [업그레이드 가이드](https://docs.djangoproject.com/en/stable/howto/upgrade-version/)를 참조하십시오.

## Python 호환성

Django 2.0은 Python 3.4, 3.5 및 3.6을 지원합니다. 각 시리즈의 최신 버전을 공식적으로 지원할 것을 적극 권장합니다. Django 1.11.x 시리즈는 Python 2.7을 지원하는 마지막 버전입니다. Django 2.0은 Python 3.4를 지원하는 마지막 릴리즈 시리즈입니다. Django 2.0 (2019 년 4 월)의 수명이 종료 될 때까지 Python 3.4를 배포하려면 Django 1.11 LTS (2020 년 4 월까지 지원)를 사용하십시오. 그러나 Python 3.4의 수명은 2019 년 3 월입니다.

## 이전 버전 Django에 대한 Third-party 라이브러리 지원

Django 2.0이 출시된 후 Third-party 앱 작성자는 Django 1.11 이전 버전의 모든 버전에 대한 지원을 중단할 것을 제안합니다. 그 때 당신은 `python -Wd` 를 사용하여 패키지의 테스트를 실행할 수 있어야 합니다. 그래서 deprecation 경고가 나타납니다. Deprecation 경고 수정을 한 후에는 앱이 Django 2.0과 호환되어야 합니다.

## What’s new in Django 2.0

### 단순화된 URL 라우팅 문법

새로운 django.urls.path() 함수는 더 간단하고 읽기 쉬운 URL 라우팅 구문을 허용합니다. 예를 들어 이전 Django 릴리즈의 예제는 다음과 같이 작성할 수 있습니다.
```
url(r'^articles/(?P<year>[0-9]{4})/$', views.year_archive),
```
다음과 같이 작성할 수 있습니다.
```
path('articles/<int:year>/', views.year_archive),
```
새로운 구문은 URL 매개 변수의 타입을 강제합니다. 이 예에서 View 는 year 인자를 문자열이 아닌 정수로 수신합니다.
이전 버전의 django.conf.urls.url() 함수는 이제 django.urls.re_path()로 사용할 수 있지만 이전 위치는 향후 제공 될 예정이며 더 이상 지원되지 않을 예정입니다. 이전 django.conf.urls.include() 함수는 이제 django.urls에서 가져올 수 있으므로 django.urls import include, path, re_path를 URLconfs 에서 사용할 수 있습니다.
URL dispatcher 문서가 다시 작성되어 새로운 구문을 적용하고 자세한 내용을 제공합니다.

### 모바일 친화적인 contrib.admin

관리자 페이지는 이제 반응형이며, 모든 주요 모바일 장치를 지원합니다. 이전 버전의 브라우저는 다양한 레벨의 우아한 성능 저하를 경험할 수 있습니다.

### 창 표현식

새로운 Window 식을 사용하면 쿼리셋에 OVER 절을 추가할 수 있습니다. 표현식에서 window 함수와 aggregate 함수를 사용할 수 있습니다.

### Minor features

## 이전 버전과 호환되지 않는 2.0 의 변경 사항

### 일부 장소에서 바이트 테스트 지원이 제거되었습니다.

네이티브 Python 2 문자열을 지원하기 위해 이전 Django 버전은 바이트 문자열과 유니 코드 문자열을 모두 받아 들여야했습니다. 이제 Python 2 지원이 중단되었으므로 입력 / 출력 경계 (예 : 2 진 필드 또는 HTTP 스트림 처리)에서만 바이트 테스트가 발생합니다. Django는 특정 코드 경로에서 바이트 체크를 더 이상 받아들이지 않으므로 바이트 코드 사용을 최소한으로 제한하도록 코드를 업데이트해야 할 수도 있습니다.

## Features deprecated in 2.0

## Features removed in 2.0
