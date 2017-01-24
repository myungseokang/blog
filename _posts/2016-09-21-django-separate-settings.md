---
layout: post
title: "Django에서 settings.py 분리하기"
date: 2016-09-21
categories: Django
author: leop0ld
---

Django에서 settings.py 분리하기
=====

## settings.py 분리의 필요성
Django 프로젝트를 진행하다보면 여러 개발자가 한 프로젝트를 작업하게 됩니다.
또한 각 개발자분들마다 사용하던(익숙한) 데이터베이스와 환경 설정 등이 다를 수도 있습니다.
그리고 제일 중요한 부분은 production에서는 settings.py의 DEBUG가 False 여야 하는데 개발 환경에서는 TRUE로 해놓고 작업하다가 다시 False로 바꿔주고 서버로 올려줘야하는 아주아주 귀찮은 일이 닥치게 됩니다.
항상 귀찮음이 사람들을 발전시키는 것 같습니다.

이러한 귀찮음을 해결하기 위해 **settings를 여러 개를 만들고 상황에 따라 다른 settings을 사용한다** 라는 것을 고안해냈습니다.
누가 처음 생각해냈는지는 모르겠으나 감사합니다.

일반적으로 3개의 settings, 많으면 4,5개 까지도 가능합니다.

## settings.py 분리하기

아래는 <project_name>/<project_name>/settings(폴더) 의 경로에 있는 파일의 구조입니다.

settings라는 폴더를 하나 만들어서 거기서 파일을 이렇게 만들어줍니다. 

```shell
.
├── __init__.py
├── base.py
├── local.py
└── production.py
```

그리고 원래 settings.py에 있던 파일의 내용을 일단 전부 다 base.py로 옮겨줍니다.
그리고 BASE_DIR 경로를 

```python
BASE_DIR = os.path.dirname(os.path.dirname(os.path.dirname(os.path.abspath(__file__))))
```

이렇게 바꿔줍니다(경로가 바뀌어서 바꿔주는 겁니다.)
base.py 는 local과 production 부분에서 공통적으로 사용하는 부분을 담아놓은 파일입니다.
local과 production 쪽에서 이렇게 사용합니다.

```python
from .base import *
```

이렇게 사용하게 되면 `*` 이 표시덕분에 base.py의 모든 것을 import 시키게 됩니다.
import * 을 사용해도 되는 유일한 곳입니다.(라이브러리 등을 import 할때는 절대 * 을 사용하지 마세요)

그 다음에 각각 설정을 해줍니다.

만약 로컬에서는 로컬의 PostgreSQL을 사용하고 서버에서는 AWS의 RDS를 사용한다면 local.py에선 DB HOST 부분을 localhost로, 그리고 production.py에서는 AWS의 RDS 주소를 사용해주면 됩니다.

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
읽어주셔서 감사합니다 :D
