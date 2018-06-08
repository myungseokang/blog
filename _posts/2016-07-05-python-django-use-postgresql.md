---
layout: post
title: "django 에서 PostgreSQL 사용하기"
categories: posts
excerpt: "Using PostgreSQL in django"
tags: [django, postgresql]
author: leop0ld
comments: true
share: true
ads: true
date: 2016-07-05
---

# django 에서 PostgreSQL 사용하기

## 1. PostgreSQL 설정

Django와 PostgreSQL을 연동하기 위해서는 psycopg2 라는 패키지가 필요합니다.

그럼 설치하기 전에

```shell
$ sudo apt-get update
```

는 꼭 하고 갑시다.

그 다음 psycopg2를 설치해줍니다.

```shell
$ pip install psycopg2
```

를 해주고 PostgreSQL에서 Django 프로젝트를 위한 user와 DB를 생성해줘야 합니다.

먼저 Django 프로젝트를 위한 DB를 하나 만들어줍니다.

```shell
$ sudo -u postgres createdb 데이터베이스_이름
```

그리고 위에서 만들어준 DB로 접속해줍니다.

```shell
$ sudo -u postgres psql 데이터베이스_이름
```

접속을 했습니다.

이제 Django 프로젝트를 위한 유저를 생성해보겠습니다.

데이터베이스_이름=# create user 유저_이름 with password '패스워드';
요런식으로 생성이 가능합니다. 그리고 간단한 encoding 설정과 timezone 설정을 해보겠습니다.

```sql
데이터베이스_이름=# alter role 유저_이름 set client_encoding to 'utf-8';
ALTER ROLE
데이터베이스_이름=# alter role 유저_이름 set timezone to 'Asia/Seoul';
ALTER ROLE
```

이런식으로 timezone과 encoding 설정을 해주었습니다.

그리고 만든 유저에게 grant 구문으로 데이터베이스에 대한 권한을 부여해줄 차례입니다.

```sql
데이터베이스_이름=# grant all privileges on database 데이터베이스_이름 to 유저_이름;
```

와 같이 설정해줄 수 있습니다.

이제 데이터베이스쪽 설정은 끝났습니다.

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

## 2. Django 설정

Django 프로젝트를 열고 프로젝트_이름/settings.py 로 가줍니다.

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql_psycopg2',
        'NAME': '데이터베이스_이름',
        'USER': '유저_이름',
        'PASSWORD': '패스워드',
        'HOST': 'localhost',
        'PORT': '',
    }
}
```

DATABASE 부분을 위와 같이 설정해주고 makemigration을 진행한 뒤 migrate 를 진행해주게 되면 잘 실행됩니다!

이상 읽어주셔서 감사합니다 :)
