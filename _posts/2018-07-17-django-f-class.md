---
layout: post
title: "Django에서 F() 객체 알아보기"
categories: posts
excerpt: "F expressions in Django"
tags: [python, django, f-expressions]
author: leop0ld
comments: true
share: true
ads: true
date: 2018-07-17
---

> 이 글은 Django 2.0 기준으로 작성되었습니다.<br/>
> 참고: [Query Expressions 공식 문서](https://docs.djangoproject.com/en/2.0/ref/models/expressions/#f-expressions)

Django 공식 문서에서 **쿼리 표현식 (Query Expressions)**이란 `create`, `update`, `filter`, `order by`, `annotation`, `aggreagate`의 일부로 사용할 수 있는 값 혹은 표현식을 일컫는다.
이러한 쿼리 표현식의 핵심이 되는 클래스가 바로 `F()` 객체이다.

## F()...?

Django 공식 문서에서 `F()` 객체에 대한 정의를 보자면,

> F() 객체는 모델의 필드 혹은 어노테이트된 열의 값을 나타낸다.
> 실제로 데이터베이스에서 Python 메모리로 가져오지 않고, 모델 필드 값을 참조하고 이를 데이터베이스에서 사용하여 작업할 수 있다.

한마디로 Django에서 `F()` 객체를 사용하게 되면, 그 연산에 해당하는 쿼리를 만들어낸다는 것이다.

아래 예제를 통해 설명해보겠다.

```python
reporter = Reporters.objects.get(name='Tintin')
reporter.stories_filed += 1
reporter.save()
```

위의 예제에서는 `reporter.stories_filed` 값을 데이터베이스에서 Python 메모리로 가져와서 Python 연산자를 통해 데이터를 조작하고 그 값을 다시 데이터베이스에 저장했다.

하지만 이 작업을 다음과 같이 할 수도 있다.

```python
from django.db.models import F

reporter = Reporters.objects.get(name='Tintin')
reporter.stories_filed = F('stories_filed') + 1
reporter.save()
```

위의 예제의 `reporter.stories_filed = F('stories_filed') + 1` 이 라인에서 Python 연산자를 사용한 것처럼 보이지만 아니다.

Django에서는 `F()` 객체를 만나면, Python 연산자를 오버라이딩하여 캡슐화된 SQL문을 생성한다.

위의 작업은 전적으로 데이터베이스에서 처리하므로, Python에서는 `reporter.stories_filed`에 대해 알 수 없다.

따라서 위의 작업으로 저장된 새로운 값을 사용하려면 반드시 아래 코드처럼 다시 불러와야 한다.

```python
reporter = Reporters.objects.get(pk=reporter.pk)
# 또는 간결하게 아래처럼
reporter.refresh_from_db()
```

## F() 객체 쓰면 뭐가 좋나요?

`F()` 객체의 이점은

1. Python이 아닌 데이터베이스에서 해당 연산을 처리한다는 점과,
2. 몇몇 작업에 필요한 쿼리 수를 줄일 수 있다는 점이다.
3. 그리고 `F()` 객체의 또 다른 이점은 **경쟁 조건 (race condition)을 피할 수 있다는 점이다**.

만약 Python 스레드가 여러 개가 실행되고 있던 경우에, 2개의 스레드(1번, 2번)에서 `reporter.stories_filed` 값을 1 증가시키는 작업을 하고 있다고 가정해보자.

더해서 `reporter.stories_filed` 값이 1이었다고 가정해보자.

그러면 1번 스레드는 데이터베이스에서 Python 메모리로 값을 가져와서 Python 연산자를 통해 1 증가시킨 값인 2를 데이터베이스에 저장하려고 한다.

그리고 2번 스레드가 1번 스레드가 데이터베이스에 채 저장하기 전에 값에 접근해서 1번 스레드에서 증가시키기 전과 같은 값인 2를 계산해냈다.

그렇게 되면 2번 스레드는 1번 스레드가 했었던 작업과 정확히 똑같은 작업을 하게 되고, 1번 스레드의 작업은 손실된다.

즉, 2개의 스레드를 돌려 2를 증가하게 하고 싶었으나 1밖에 증가시키지 못한 것이다.

하지만 `F()` 객체를 사용하게 될 경우, Python에서 해당 값을 메모리에 가지고 있다가 처리하는 것이 아니라 데이터베이스에서 해당 작업을 처리하기 때문에 이러한 경쟁 조건을 피할 수 있다.

`F()` 객체는 어노테이션, 필터링, 정렬에도 굉장히 효과적으로 사용할 수 있다.

정렬에 사용되는 예시를 보자.

```python
from django.db.models import F
Company.object.order_by(F('last_contacted').desc(nulls_last=True))
```

위의 코드는 `Company` 모델 인스턴스들을 `last_contacted` 필드를 기준으로 내림차순 정렬하되, 해당 필드가 null 값을 가졌을 경우에 제일 뒤로 보낸다.

반대로 `nulls_first` 옵션도 있고, `.asc()` 메서드도 있다.

그리고 어노테이션에도 유용하게 쓰인다.

```python
company = Company.objects.annotate(
    chairs_needed=F('num_employees') - F('num_chairs'))
```

위처럼 모델 인스턴스의 필드 값을 이용해 간단한 연산을 해서 동적으로 필드를 추가해줄 수도 있다.

만약 필드가 다른 타입이라면, `F()` 객체에서는 `output_field`를 지정할 수 없으니 아래처럼 `ExpressionWrapper`로 해당 표현식을 감싸줘야 한다.

```python
from django.db.models import DateTimeField, ExpressionWrapper, F

Ticket.objects.annotate(
    expires=ExpressionWrapper(
        F('active_at') + F('duration'), output_field=DateTimeField()))
```

그리고 `F()` 객체가 `ForeignKey`를 참조하게 될 경우, `F()` 객체는 모델 인스턴스 대신 `primary key`를 반환한다.

## 주의해야할 점이 있나요?

이렇게 좋은 `F()` 객체를 사용할 때도 **주의해야할 점이 있다**.

모델 필드에 할당된 `F()` 객체는 모델 인스턴스를 저장한 후에도 유지되며 각 `save()`에 적용된다.

```python
reporter = Reporters.objects.get(name='Tintin')
reporter.stories_filed = F('stories_filed') + 1
reporter.save()

reporter.name = 'Tintin Jr.'
reporter.save()
```

처음 `reporter.stories_filed` 값이 1이었다고 가정하고, 위의 코드를 실행하게 되면 `reporter.stories_filed` 값은 3이 되어있다.

외에도 `F()` 객체의 사용법은 굉장히 다양하다. 때에 따라 적절한 사용법을 찾으면 좋을 것 같다.

읽어주셔서 고맙습니다 :D
