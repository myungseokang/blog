---
layout: post
title: "Django에서 DB 액세스 최적화하기"
categories: posts
excerpt: "Optimize DB access in Django"
tags: [python, django, database, optimization]
author: leop0ld
comments: true
share: true
ads: true
date: 2018-08-20
---

> 이 글은 Django 2.1 기준으로 작성되었습니다.<br/>
> 참고: [DB 액세스 최적화 공식 문서](https://docs.djangoproject.com/ko/2.1/topics/db/optimization/)


## 0. 먼저 프로파일링부터

DB 액세스나 쿼리를 최적화하기 전에 <a href="https://docs.djangoproject.com/ko/2.1/faq/models/#faq-see-raw-sql-queries" target="_blank">해당 쿼리와 그에 대한 비용에 대해 알아보는 게 중요합니다</a>.

QuerySet이 데이터베이스에 의해서 어떻게 실행되는지 이해하려면 <a href="https://docs.djangoproject.com/ko/2.1/ref/models/querysets/#django.db.models.query.QuerySet.explain" target="_blank">QuerySet.explain()</a>을 사용하면 됩니다.

<a href="https://github.com/jazzband/django-debug-toolbar/" target="_blank">django-debug-toolbar</a>와 같은 외부 프로젝트나 데이터베이스를 직접 모니터링하는 도구를 사용할 수도 있습니다.

## 1. 보편적인 DB 최적화 기법 사용하기

- <a href="https://ko.wikipedia.org/wiki/%EC%9D%B8%EB%8D%B1%EC%8A%A4_(%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4)" target="_blank">인덱스</a>: Django에서는 <a href="https://docs.djangoproject.com/ko/2.1/ref/models/fields/#django.db.models.Field.db_index" target="_blank">Field.db_index</a> 또는 <a href="https://docs.djangoproject.com/ko/2.1/ref/models/options/#django.db.models.Options.index_together" target="_blank">Meta.index_together</a>를 사용하여 추가할 수 있습니다. `filter()`, `exclude()`, `order_by()` 등을 사용하여 자주 쿼리하는 필드에 인덱스를 추가하는 것이 좋습니다. 인덱스를 사용하면 조회 속도를 높이는 데 도움이 됩니다. 하지만 인덱스를 너무 남발하게 될 경우, 인덱스를 유지하는 비용이 인덱스를 사용해 속도를 높인 쿼리의 성능보다 클 수 있기 때문에 신중하게 결정해야 합니다.

- 적절한 필드 타입 사용


## 2. QuerySet 이해하기

<a href="https://docs.djangoproject.com/ko/2.1/ref/models/querysets/" target="_blank">QuerySet</a>을 이해하는 것은 간단한 코드로 좋은 성능을 얻는 데 특히 중요합니다.

#### 2-1. QuerySet 계산 이해하기

성능 문제를 피하려면 다음을 이해하는 게 중요합니다.

- <a href="https://docs.djangoproject.com/ko/2.1/topics/db/queries/#querysets-are-lazy" target="_blank"><b>QuerySet은 게으르다(lazy)</b></a>
    - QuerySet을 생성하는 행위는 데이터베이스 활동을 포함하지 않습니다. Django는 QuerySet이 계산될 때까지 실제로 쿼리를 실행하지 않습니다. 실제로 계산되는 시점은 아래에서 더 자세하게 다룹니다.
- <a href="https://docs.djangoproject.com/ko/2.1/ref/models/querysets/#when-querysets-are-evaluated" target="_blank"><b>QuerySet이 계산되는 시점</b></a>
    - QuerySet은 다음과 같은 시점에 계산됩니다: **반복 (Iteration)**, **슬라이싱 (Slicing)**, **피클링 / 캐싱 (Pickling/Caching)**, **repr()**, **len()**, **list()**, **bool()**
- <a href="https://docs.djangoproject.com/ko/2.1/topics/db/queries/#caching-and-querysets" target="_blank"><b>데이터가 메모리에 저장되는 방식</b></a>
    - 각 QuerySet에는 데이터베이스 액세스를 최소화하기 위한 캐시가 포함되어 있습니다. 이것이 어떻게 작동하는지 이해하면 효율적인 코드를 작성하는 데 도움이 됩니다.

#### 2-2. 캐시된 속성 이해하기

전체 QuerySet 캐싱 뿐만 아니라 ORM 객체에 대한 속성 결과 캐싱도 있습니다. 일반적으로 호출할 수 없는 속성은 캐싱됩니다. 예를 들어 Weblog 모델 예제를 보면 다음과 같습니다:

```python
>>> entry = Entry.objects.get(id=1)
>>> entry.blog   # Blog 객체가 여기서 검색된다
>>> entry.blog   # 캐시되었기 때문에 여기서는 DB 액세스가 일어나지 않는다
```

그러나 일반적으로 호출 가능한 속성은 매번 DB 조회를 발생시킵니다.

```python
>>> entry = Entry.objects.get(id=1)
>>> entry.authors.all()   # 쿼리 실행
>>> entry.authors.all()   # 또다시 쿼리 실행
```

#### 2-3. `with` 템플릿 태그 사용하기

QuerySet의 캐싱 동작을 사용하려면 <a href="https://docs.djangoproject.com/ko/2.1/ref/templates/builtins/#std:templatetag-with" target="_blank">with</a> 템플릿 태그를 사용해야 할 수도 있습니다.

<a href="https://docs.djangoproject.com/ko/2.1/ref/templates/builtins/#std:templatetag-with" target="_blank">with</a> 템플릿 태그는 복잡한 변수를 더 간단한 이름으로 저장합니다.

이는 비용이 많이 드는 방법 (EX. 데이터베이스를 조회하는 방법)에 여러 번 액세스 할 때 유용합니다.

#### 2-4. `iterator()` 사용하기

객체가 많은 경우 `QuerySet`의 캐싱 동작으로 인해 많은 양의 메모리가 사용될 수 있습니다. 이 경우 <a href="https://docs.djangoproject.com/ko/2.1/ref/models/querysets/#django.db.models.query.QuerySet.iterator" target="_blank">iterator()</a>가 도움이 될 수 있습니다.
한 번 액세스 해야하는 많은 수의 객체를 반환하는 `QuerySet`의 경우, 성능이 향상되고 메모리가 크게 감소할 수 있습니다.

#### 2-5. `explain()` 사용하기

맨 처음 프로파일링 부분에서도 언급했지만, <a href="https://docs.djangoproject.com/ko/2.1/ref/models/querysets/#django.db.models.query.QuerySet.explain" target="_blank">QuerySet.explain()</a>은 사용된 인덱스 및 조인을 포함하여 데이터베이스에서 쿼리를 실행하는 방법에 대한 자세한 정보를 제공합니다.
이러한 세부 정보는 보다 효율적으로 다시 작성할 수 있는 쿼리를 찾거나 성능을 향상시키기 위해 추가할 수있는 인덱스를 식별하는 데 도움이 될 수 있습니다.

## 3. Python이 아닌 데이터베이스에서 작동하는 경우

예를 들어,

- `filter()` 및 `exclude()`를 사용하여 데이터베이스에서 필터링하는 경우
- <a href="https://docs.djangoproject.com/ko/2.1/ref/models/expressions/#django.db.models.F" target="_blank">F 표현식</a>을 사용하여 같은 모델의 다른 필드를 기반으로 필터링하는 경우
- 데이터베이스에서 `aggregation`을 하기 위해 어노테이션하는 경우

다음과 같이 필요한 SQL을 생성하기에 충분하지 않은 경우에는 다음과 같은 방법이 있습니다.

- <a href="https://docs.djangoproject.com/ko/2.1/ref/models/expressions/#django.db.models.expressions.RawSQL" target="_blank">RawSQL</a> 사용하기
    - 일부 SQL을 쿼리에 명시적으로 추가할 수 있습니다.
- <a href="https://docs.djangoproject.com/ko/2.1/topics/db/sql/" target="_blank">raw SQL</a> 사용하기
    - 그래도 부족할 경우, 완전하게 raw SQL를 사용할 수도 있습니다.

## 4. 고유하거나 인덱스된 열을 사용한 개별 객체 검색하는 경우

`get()`을 사용하여 개별 객체를 검색할 때 `unique` 또는 `db_index` 열을 사용하는 데는 두 가지 이유가 있습니다.

첫 번째는 데이터베이스 인덱스로 인해 쿼리 속도가 빨라집니다.

또한 여러 객체가 조건과 일치하면 쿼리가 훨씬 느리게 실행될 수 있습니다.

열에 고유한 제한 조건이 있으면 이것이 결코 발생하지 않을 것이기 때문입니다.

## 5. 필요한 항목은 즉시 검색하세요

일반적으로 모든 부분을 필요로 하는 한 데이터 집합의 여러 부분에 대해 데이터베이스를 여러 번 히트하는 것은 하나의 쿼리에서 모두 검색하는 것보다 효율적이지 않습니다.
이 사항은 루프에서 실행되는 쿼리가 있는 경우 특히 중요하므로 하나만 필요할 때 많은 데이터베이스 쿼리를 수행할 수 있습니다.

#### `QuerySet.select_related()` 및 `prefetch_related()`를 사용하기

<a href="https://docs.djangoproject.com/ko/2.1/ref/models/querysets/#django.db.models.query.QuerySet.select_related" target="_blank">select_related()</a> 및 <a href="https://docs.djangoproject.com/ko/2.1/ref/models/querysets/#django.db.models.query.QuerySet.prefetch_related" target="_blank">prefetch_related()</a>를 철저히 이해하고 사용하세요.

- Manager가 사용 중이거나 사용되지 않을 떄를 숙지해야 합니다.
- view 코드 혹은 다른 레이어에서 필요에 따라 <a href="https://docs.djangoproject.com/ko/2.1/ref/models/querysets/#django.db.models.prefetch_related_objects" target="_blank">prefetch_related_objects()</a>를 사용합니다.

## 6. 필요없는 항목은 검색하지 마세요

#### `QuerySet.values()` 및 `values_list()`를 사용하기

`dict` 또는 `list` 값을 원할 때, ORM 모델 객체가 필요하지 않은 경우에는 <a href="https://docs.djangoproject.com/ko/2.1/ref/models/querysets/#django.db.models.query.QuerySet.values" target="_blank">values​​()</a>를 적절하게 사용하세요. 템플릿 코드에서 모델 객체를 대체하는 데 유용할 수 있습니다.

#### `QuerySet.defer()` 및 `only()`를 사용하기

데이터베이스 열(column)이 필요없다는 것을 알고 있거나(대부분의 경우 필요하지 않을 것이다) 데이터베이스 열(column)이 로드되지 않도록 하려면 <a href="https://docs.djangoproject.com/ko/2.1/ref/models/querysets/#django.db.models.query.QuerySet.defer" target="_blank">defer()</a>와 <a href="https://docs.djangoproject.com/ko/2.1/ref/models/querysets/#django.db.models.query.QuerySet.only" target="_blank">only()</a>를 사용하세요. 하지만 부적절하게 사용하는 경우 ORM이 별도의 쿼리로 가져와야 하므로 더 나빠질 수도 있음을 알아두세요.

또한 지연된 필드(deferred fields)가 있는 모델을 생성할 때, Django 내부에서 발생하는 약간의 오버 헤드가 있음에 유의하세요. `defer()` 및 `only()` 메서드는 많은 텍스트 데이터를 로드하지 않거나 Python으로 다시 변환하기 위해 많은 처리가 필요한 필드의 경우 가장 유용합니다. 항상 그렇듯이 먼저 프로파일링을 한 다음 최적화하세요.

#### `QuerySet.count()` 사용하기

개수만 원하는 경우에 `len(queryset)`보다 적절합니다.

#### `QuerySet.exists()` 사용하기

적어도 하나의 결과가 존재하는지 알아내고 싶은 경우에 `if queryset`보다 적절합니다.

**하지만**

#### `count()` 및 `exists()`를 과도하게 사용하지 마세요

QuerySet에서 다른 데이터가 필요하다면 그냥 계산하세요.

예를 들어, User 모델과 many-to-many 관계가 있고, `body` 속성을 가진 Email 모델이 있다고 가정하면 다음 템플릿 코드가 최적입니다.

```django{% raw %}
{% if display_inbox %}
  {% with emails=user.emails.all %}
    {% if emails %}
      <p>You have {{ emails|length }} email(s)</p>
      {% for email in emails %}
        <p>{{ email.body }}</p>
      {% endfor %}
    {% else %}
      <p>No messages today.</p>
    {% endif %}
  {% endwith %}
{% endif %}
{% endraw %}
```

다음과 같은 이유로 최적입니다:

1. `display_inbox`가 `False`일 경우 데이터베이스 쿼리가 발생하지 않아서 QuerySet이 게으르기(lazy) 때문입니다.
2. <a href="https://docs.djangoproject.com/ko/2.1/ref/templates/builtins/#std:templatetag-with" target="_blank">with</a>를 사용하면 나중에 사용할 수 있도록 `user.emails.all`을 변수에 저장하여 다시 사용할 수 있습니다.
3. `{% raw %}{% if emails %}{% endraw %}` 행은 `QuerySet.__bool__()`이 호출되도록 합니다. 이 경우 `user.emails.all()` 쿼리가 데이터베이스에서 실행되고 최소한 첫 번째 행은 ORM 객체로 변환됩니다. 결과가 없으면 `False`를 반환하고 그렇지 않으면 `True`를 반환합니다.
4. `{{ emails|length }}`를 사용하면 `QuerySet.__len__()`을 호출하여 다른 쿼리를 수행하지 않고 나머지 캐시를 채웁니다.
5. `for` 루프는 이미 채워진 캐시를 반복합니다.

전체적으로 이 코드는 하나 또는 0개의 데이터베이스 쿼리를 수행합니다. 수행된 유일한 의도적 최적화는 `with` 태그의 사용입니다.
언제든지 `QuerySet.exists()` 또는 `QuerySet.count()`를 사용하면 추가 쿼리가 발생합니다.

#### `QuerySet.update()` 및 `delete()`를 사용하기

객체의 로드를 검색하고, 일부 값을 설정하고, 개별적으로 저장하는 대신, <a href="https://docs.djangoproject.com/ko/2.1/topics/db/queries/#topics-db-queries-update" target="_blank">QuerySet.update()</a>를 통해 대량 SQL UPDATE 문을 사용하세요.
마찬가지로 가능한 경우에 대량 삭제(<a href="https://docs.djangoproject.com/ko/2.1/topics/db/queries/#topics-db-queries-delete" target="_blank">bulk deletes</a>)를 수행하세요.
그러나 이러한 일괄 업데이트 메서드는 개별 인스턴스의 `save()` 또는 `delete()` 메서드를 호출할 수 없으므로 이러한 메서드에 대해 추가한 사용자 지정 동작은 일반 데이터베이스 객체 시그널(<a href="https://docs.djangoproject.com/ko/2.1/ref/signals/" target="_blank">signals</a>)에서 비롯된 모든 작업을 포함하여 실행되지 않습니다.

#### 외래 키 값 직접 사용하기

외래 키 값만 필요하면 관련 객체 전체를 가져오는 대신 이미 가지고있는 객체에 있는 외래 키 값을 사용하세요.

즉,

```python
# 이렇게 말고
entry.blod.id
```

```python
# 이렇게 사용하세요
entry.blod_id
```

#### 상관없다면 결과를 정렬하지 마세요

정렬은 결코 가벼운 작업이 아닙니다. 각 필드별로 데이터베이스가 반드시 수행해야하는 작업입니다. 모델에 기본 순서(<a href="https://docs.djangoproject.com/ko/2.1/ref/models/options/#django.db.models.Options.ordering" target="_blank">Meta.ordering</a>)가 있고, 필요하지 않은 경우 매개 변수 없이 <a href="https://docs.djangoproject.com/ko/2.1/ref/models/querysets/#django.db.models.query.QuerySet.order_by" target="_blank">order_by()</a>를 호출하여 QuerySet에서 모델을 제거하세요.

데이터베이스에 인덱스를 추가하면 정렬 성능을 향상시키는 데 도움이 될 수 있습니다.

## 7. 일괄 삽입

가능한 경우, 개체를 만들 때 <a href="https://docs.djangoproject.com/ko/2.1/ref/models/querysets/#django.db.models.query.QuerySet.bulk_create" target="_blank">bulk_create()</a> 메서드를 사용하여 SQL 쿼리 수를 줄입니다.

예를 들어,

```python
# 이 코드보다
Entry.objects.create(headline='This is a test')
Entry.objects.create(headline='This is only a test')
```

```python
# 이 코드가 더 바람직하다
Entry.objects.bulk_create([
    Entry(headline='This is a test'),
    Entry(headline='This is only a test'),
])
```

이 방법에는 <a href="https://docs.djangoproject.com/ko/2.1/ref/models/querysets/#django.db.models.query.QuerySet.bulk_create" target="_blank">여러 가지 주의 사항</a>이 있으므로 사용 사례에 적절한 지 확인하세요.

이것은 ManyToManyFields에도 적용됩니다.

```python
# 이 코드보다
my_band.members.add(me)
my_band.members.add(my_friend)
```

```python
# 이 코드가 더 바람직하다
my_band.members.add(me, my_friend)
```

읽어주셔서 감사합니다 :)
