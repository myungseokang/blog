---
layout: post
title: "Django에서 settings.py 여러 개로 분리하기"
categories: posts
excerpt: "Splitting settings.py into mutiple pieces in Django"
tags: [python, django]
author: myungseokang
comments: true
share: true
ads: true
date: 2016-09-21
---

## settings.py 분리의 필요성

Django 프로젝트를 진행하다보면 여러 개발자가 한 프로젝트를 작업하게 됩니다.

또한 각 개발자분들마다 사용하던(혹은 익숙한) 데이터베이스와 환경 설정 등이 다를 수도 있습니다.

그리고 제일 중요한 부분은 실제 배포 환경에서는 settings.py의 `DEBUG`가 `False`여야 하는데 개발 환경에서는 `True`로 해놓고 작업하다가 다시 `False`로 바꿔주고 서버로 올려줘야 하는 아주 귀찮은 일이 닥치게 됩니다.

**항상 귀찮음이 사람들을 발전시키는 것 같습니다**.

이러한 귀찮음을 해결하기 위해 **settings를 여러 개를 만들고 상황에 따라 다른 settings을 사용한다** 라는 것을 고안해냈습니다.
누가 처음 생각해냈는지는 모르겠으나 감사합니다.

일반적으로 3개의 settings, 많으면 4,5개 까지도 가능합니다.

## settings.py 여러 개로 분리하기

아래는 `<project_name>/<project_name>/settings` 의 경로에 있는 파일 구조입니다.

settings라는 폴더를 하나 만들어서 거기서 파일을 이렇게 만들어줍니다.

```shell
settings/
├── __init__.py
├── base.py
├── local.py
└── production.py
```

그리고 원래 settings.py에 있던 파일의 내용을 일단 전부 다 base.py로 옮겨줍니다.

그리고 settings.py 의 경로가 바뀌었으니 `BASE_DIR` 경로를 아래와 같이 바꿔줍니다.

```python
BASE_DIR = os.path.dirname(os.path.dirname(os.path.dirname(os.path.abspath(__file__))))
```

base.py 는 로컬과 프로덕션 부분에서 공통적으로 사용하는 설정을 담아놓은 파일입니다.

로컬과 프로덕션 쪽에서 이렇게 사용합니다.

```python
from .base import *
```

이렇게 사용하게 되면 `*` 이 표시덕분에 base.py의 모든 것을 `import` 하게 됩니다.
`import *` 을 사용해도 되는 유일한 곳입니다. (라이브러리 등을 import 할때는 절대 * 을 사용하지 마세요.)

> Tip! flake8 을 사용할 떄, `import *` 을 하게 되면 잘못 되었다고 알려줍니다. 이럴 땐 "**No Quality Assurance**"의 약자인 `NOQA` 를 해당 줄에 주석으로 달아주면 flake8 검사 시 무시가 됩니다.


그 다음에 각각 설정을 해줍니다.

만약 로컬에서는 로컬의 PostgreSQL을 사용하고 프로덕션에서는 AWS의 RDS를 사용한다면 local.py에선 `DB HOST` 부분을 localhost로, 그리고 production.py에서는 AWS의 RDS 주소를 사용해주면 됩니다.

이런 식으로 각각 설정을 해주시면 됩니다.

이제 manage.py로 가보겠습니다.

manage.py는 주로 개발할 때 사용하기 때문에 아래와 같이 settings.local 로 사용하도록 설정을 해주고 wsgi.py는 production을 사용하게 해줍니다.

manage.py

```python
#!/usr/bin/env python
import os
import sys

if __name__ == "__main__":
    os.environ.setdefault("DJANGO_SETTINGS_MODULE", "<PROJECT_NAME>.settings.local")

    from django.core.management import execute_from_command_line

    execute_from_command_line(sys.argv)
```

wsgi.py

```python
import os
os.environ.setdefault("DJANGO_SETTINGS_MODULE", "<PROJECT_NAME>.settings.production")

from django.core.wsgi import get_wsgi_application
application = get_wsgi_application()
```

이런 식으로 해줬습니다.

읽어주셔서 고맙습니다 :D
