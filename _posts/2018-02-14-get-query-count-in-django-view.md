---
layout: post
title: "Django view 안에서 쿼리 개수 확인하기"
categories: posts
excerpt: "Get query count in Django view"
tags: [django]
author: leop0ld
comments: true
share: true
date: 2018-02-14
---

> 이 글은 Django 2.0 기준으로 작성되었습니다.

가끔 개발하다가 오른쪽 옆에 떠있는 <a href="https://github.com/jazzband/django-debug-toolbar" target="_blank">django debug toolbar</a> 를 보고 쿼리 개수를 확인하는 경우가 많다.

하지만 json response 를 return 하는 View 경우에는 debug toolbar 로 확인하는 것도 한계가 있다.

그럴 떄 쉽게 View 단에서 쿼리 개수를 console 에 나타낼 수 있는 방법을 소개해볼까 한다.

```python
from django.db import connection, reset_queries
print(len(connection.queries))
reset_queries()
```

이렇게 View 코드에 추가해주면 해당 View 에서 몇개의 쿼리가 발생했는지 알 수 있다.

그리고 해당 connection.queries는 `sql`과 `time`이라는 속성을 가진 Dictionary의 List를 반환하고 있다.

따라서 해당 속성을 통해 특정 View 에서 걸리는 쿼리의 시간을 확인할 수도 있다.

```
from django.db import connection
print(sum(float(query['time']) for query in connection.queries))
```

이런 식으로 View 내부에서 확인할 수 있다.

하지만 어디까지나 django debug toolbar 를 못쓸 때의 이야기이다.

쓸 수 있다면 무조건 debug toolbar 로 확인하는 게 좋은 것 같다.

<a href="https://docs.djangoproject.com/ko/2.0/faq/models/" target="_blank">해당 공식문서 바로가기</a>

감사합니다 :D
