---
layout: post
title: "select_related, prefetch_related 그리고 debug toolbar"
date: 2017-06-27
categories: django
author: leop0ld
---

> 이 글은 django 1.11 LTS 버전 기준으로 작성되었습니다.

![Optimize all the things!](/assets/img/optimize-all-the-things.jpg)

회사에 다니기 시작하면서 성능에 예민해지기 시작했고, Caching(캐싱), Query Optimization(쿼리 최적화) 등등 어찌보면 혼자서는 잘 접근할 수 없는 부분에 관심이 생겼다.

그렇게 그냥저냥 회사코드를 만지다가 **select_related**, **prefetch_related** 를 만났다.

처음에 **select_related**, **prefetch_related** 를 몰랐을 때는 로직을 잘 작성했는데 왜 이렇게 느리지 라고 생각했었다.

알고보니 3 depths 였는데 아무런 행동도 취해주지 않아서 날렸던 쿼리를 또 날리고, 또 날리고 해서 느렸던 것이었다. ㅠㅠ

django debug toolbar 라는 게 있는데 그것을 살펴보면 현재 페이지가 로딩될 때 몇개의 쿼리가 얼마나 걸렸는지 보여주는데, 보고 충격먹어서 쿼리 최적화를 잘 해야겠다는 생각으로 이번 글을 쓰게 되었다.

처음에는 ~~헐 저게 뭐야~~ 라는 심정으로 봤었는데 보다보니까 쪼금 봐줄만한 정도는 된 것 같다.

이제부터 **쿼리 최적화** 에 대한 이야기를 해볼까 한다.

## select\_related? prefetch\_related? 그게 뭔데?

<a href="https://docs.djangoproject.com/en/1.11/ref/models/querysets/#select-related" target="_blank">select_related 공식문서</a>, <a href="https://docs.djangoproject.com/en/1.11/ref/models/querysets/#prefetch-related" target="_blank">prefetch_related 공식문서</a>

select\_related 와 prefetch\_related 는 하나의 QuerySet 객체를 가져올 때 **연관있는 Model 들을 같이 가져오게** 해주는 메서드이다.

다만 두 메서드를 사용할 타이밍은 **각각 다르다**. 조금 이따 다뤄보도록 하겠다.

SQL에서 JOIN 구문은 하나의 쿼리에서 여러개의 테이블에 접근해서 다양한 값들을 얻는다.

select\_related 와 prefetch\_related 도 마찬가지다. (내부적으로 JOIN 구문을 사용한다)

다만 표현방식이 좀 더 ORM 스럽게 바뀐 것뿐이다. (ORM 에 관한 개인적인 견해는 따로 글을 작성해볼까 한다)

이렇게 두 메서드 모두 DB에 접근하는 횟수, 즉 쿼리의 개수를 줄임으로써 성능향상을 꾀할 수 있다.

다만 위에서 말했듯이 두 메서드를 사용할 타이밍이 각각 다르다.

이제 이것에 대해 알아보도록 하겠다.

## 설명 시작하기 앞서

아래와 같은 모델 구조에서 간단하게 select\_related, prefetch\_related 를 이해해보려고 한다.

```python
from django.db import models


class Person(models.Model):
    name = models.CharField(max_length=16)

    def __str__(self):
        return self.name


class Car(models.Model):
    name = models.CharField(max_length=32)
    owner = models.ForeignKey(Person)

    def __str__(self):
        return self.name
```

이 경우는 대강 아래 그림과 같은 구조를 이루고 있을 것 같다.

![Model 구조](/assets/img/select_related-example.jpg)

이제 알아보도록 하겠다.

## select_related

**select_related** 는 1:1의 관계에서 사용할 수 있고, 혹은 1:N 관계에서 N이 사용할 수 있다.

처음 들으면 무슨 소린지 이해가 안된다. (필자도 똑같았다)

필자가 예시로 만들어뒀던 코드에서는 `Car` 기준으로 사용할 수 있다.

예시 코드를 봐보도록 하겠다.

```python
# 1번 코드
car = Car.objects.get(id=1)
owner = car.owner
```

```python
# 2번 코드
car = Car.objects.select_related('owner').get(id=1)
owner = car.owner
```

위 두개의 코드는 그렇게 많은 차이는 없다.

하지만 1번 코드의 쿼리 개수는 총 2개고, 2번 코드의 쿼리 개수는 총 1개다.

1번 코드는 Car 모델에서 car 를 id 로 get 하기 위한 쿼리 1개 + 그 car 의 owner 를 찾기 위한 쿼리 1개 해서 총 2개다.

그에 비해 2번 코드는 Car 모델에서 car 를 id 로 get 함과 동시에 Person 모델에서 car 의 owner 를 찾아둔다.

물론 이렇게 간단한 코드에서는 1개 차이정도밖에 안나겠지만 로직이 조금만 복잡해져도 큰 차이를 가져올 수 있다.

N(여기서는 Car 모델)의 입장에서 바라볼 때 1(Person)은 1개밖에 없다.

이런 경우에 select_related 를 사용한다.

## prefetch_related

**prefetch_related** 는 반대로 M:N 에 관계에서 사용할 수 있고, 1:N 의 관계에서 1이 사용할 수 있다.

위와 동일한 모델 구조가 있다고 가정할 때 반대의 입장에서 생각해보겠다.

한 Person 인스턴스가 가진 car 들을 가져오고 싶을 때는 아래와 같이 하면 된다.

```python
owner = Person.objects.prefetch_related('car_set').get(id=1)

for car in owner.car_set.all():
    print(car.name)
```

위의 코드는 owner 라는 Person 인스턴스 하나가 가진 car 들의 이름을 전부 출력해주는 간단한 코드다.

prefetch_related 메서드는 위처럼 1:N 의 관계의 1(예시에서는 Person)의 입장에서 쓰기 수월하다.

prefetch_related 메서드의 경우 좀 더 복잡하게 들어가게 될 수도 있다.

이런 경우에는 <a href="https://docs.djangoproject.com/en/1.11/ref/models/querysets/#prefetch-objects" target="_blank">Prefetch class</a> 를 사용할 수도 있다.

프로덕션 레벨의 프로젝트에서는 모델 구조가 복잡한 경우가 많은데 이럴 경우에 Prefetch class 를 쓰는 경우도 많다.

직접 예제로 설명하기는 어려울 것 같고, 간단하게 설명하자면 복잡한 모델 구조 속에서 string 값을 이용한 prefetch 로는 버거울 경우 사용할 수 있다.

특정하게 filtering 된 QuerySet 을 지정해서 prefetch 를 할 수도 있고 굉장히 다양하게 시도해볼 수 있다.

이 경우는 수가 너무 많아서 예시를 들어 설명하기가 어려울 것 같아 생략하겠다. (나중에 시간나면 예제를 추가해보도록 하겠다)

### django debug toolbar

<a href="https://django-debug-toolbar.readthedocs.io/en/stable/" target="_blank">공식문서</a>, <a href="https://github.com/jazzband/django-debug-toolbar" target="_blank">Github 주소</a>

django debug toolbar는 개발 환경에서 사용되는 아주 유용한 도구다.

쿼리의 개수나 속도는 물론이고 템플릿에 사용된 템플릿 파일이나 현재 django의 버전 등등 개발하는 데 필요한 많은 정보를 얻을 수 있다.

매우 유용하니 개발 환경에 설치해보고 사용하시는 걸 추천한다.

필자는 쿼리 최적화 작업을 하면서 아주 유용하게 사용했던 경험이 있다.

아래는 debug toolbar 의 사용할 때 모습이다.

<img src="/assets/img/django-debug-toolbar-1.png" alt="django debug toolbar" style="max-height: 700px;" />

이렇게 오른쪽 옆에 패널처럼 보이게 된다.

![django debug toolbar](/assets/img/django-debug-toolbar-2.png)

그리고 각 정보에 대해 세부적으로도 확인할 수 있고, 위처럼 중복된 쿼리는 **Duplicated** 라고 나타내준다. (정말 편리하지 않은가... 꼭 쓰자!)

진짜 좋다. ~~debug toolbar 쓰세요. 두번 쓰세요.~~

읽어주셔서 감사합니다 :D