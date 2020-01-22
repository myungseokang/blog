---
layout: post
title: "Polymorphic? Generic Relations with Django"
categories: posts
excerpt: "Polymorphic? Generic Relations! with Django"
tags: [django, polymorphic]
author: myungseokang
comments: true
share: true
ads: true
date: 2020-01-22
---

> 이 글은 <a href="https://docs.djangoproject.com/en/3.0/ref/contrib/contenttypes/#generic-relations" target="_blank">이 주제</a> 와 관련된 이야기를 다룹니다.

처음에 회사에서 어떠한 모델에 여러 모델이 연결될 수 있지만, 실제 연결될 수 있는 건 많은 모델들 중 하나만 `ForeignKey` 로 연결해야하는 경우가 생겨서, 이것저것 찾아보게 되었습니다.

처음에는 회의 도중 "polymorphic (다형성)" 과 관련된 얘기가 나왔고, 다른 데에서는 어떻게 하고 있을까 궁금하여 찾아보게 되었습니다.

Ruby on Rails 에서는 <a href="https://guides.rubyonrails.org/association_basics.html#polymorphic-associations" target="_blank">polymorphic associations</a> 으로 처리하고 있더라구요.

그래서 polymorphic 이라는 키워드로 django 에서는 어떻게 하고 있나 궁금해서 검색해보니, `django-polymorphic` (<a href="https://django-polymorphic.readthedocs.io/en/stable/" target="_blank">Docs</a>, <a href="https://github.com/django-polymorphic/django-polymorphic" target="_blank">GitHub</a>) 이라는 Third-Party 라이브러리가 나왔습니다.

Docs 와 GitHub 을 살펴보니 좋아보였지만, 제가 원하는 기능은 아니었습니다.

여기서 **"제가 원했던"** 의 조건을 짚어보자면...

예를 들어, 카카오톡 메시지에서 사진을 첨부할 수도 있고 연락처를 보낼수도 있는데, 이때 메시지와 사진, 연락처의 관계를 생각해보면 될 것 같습니다. (비유를 위한 표현이니 실제 사용하기에 적합하지 않아도 너그러운 양해 부탁드립니다.)

```py
class Photo(models.Model):
    image = models.ImageField()


class Contact(models.Model):
    phone_number = models.CharField()


class Message(models.Model):
    photo = models.ForeignKey(
        verbose_name="사진 정보",
        to=Photo,
        on_delete=models.SET_NULL,
        null=True,
    )

    contact = models.ForeignKey(
        verbose_name="연락처 정보",
        to=Contact,
        on_delete=models.SET_NULL,
        null=True,
    )

```

코드는 아니고, 개념적으로 이런 식이 되길 원했습니다.

`Message` 에서 `Photo` 와 `Contact` 라는 모델을 `ForeignKey` 로 참조하고 싶은데, 그렇게 될 경우 참조할 모델 개수만큼 컬럼을 생성해야 합니다.

하지만 실제로 메시지에서는 사진이랑 연락처는 같이 보낼 수 없기 때문에 2개 컬럼이 동시에 있을 필요는 없어서 고민을 많이 하던 도중, Django 에서 지원하는 **`GenericForeignKey` (Generic Relations)** 를 알게 되었습니다.

`GenericForeignKey` 를 사용하면 아래 코드처럼 해결할 수 있습니다.

```py
from django.contrib.contenttypes.models import ContentType


class Photo(models.Model):
    image = models.ImageField()


class Contact(models.Model):
    phone_number = models.CharField()


class Message(models.Model):
    embed_reference_type = models.ForeignKey(
        verbose_name="임베드 정보의 타입",
        to=ContentType,
        on_delete=models.SET_NULL,
        null=True,
    )
    embed_reference_id = models.PositiveIntegerField(
        verbose_name="임베드 정보의 PK",
        null=True,
    )
    embed_reference = GenericForeignKey("embed_reference_type", "embed_reference_id")

```

여기서 `ContentType` 모델이 등장합니다.

`ContentType` 모델은 Django 내장되어있는 기본 모델인데, 처음 Django 프로젝트를 만들고 마이그레이션 작업을 하게 되면 생성되는 `django_content_type` 테이블이 이 모델에 해당됩니다.

`ContentType` 모델에는 2가지 필드가 있는데, `app_label` (앱 이름) 과 `model` (모델 클래스) 를 저장하고 있습니다.

따라서 `ContentType` 을 `ForeignKey` 로 참조한다는 것은 **어떤 모델을 참조하는지를 알 수 있다** 는 것과 같습니다. 

위처럼 만들어둔 구조는 아래와 같이 사용할 수 있습니다.

```py
first_contact = Contact.objects.first()
message = Message(embed_reference=first_contact)
message.save()
```

이런 식으로 처음에 원했던 구조로 만들 수 있습니다.

예시로 든 `message` 인스턴스는 `embed_reference` 로 `first_contact` 를 가지게 되고, 실제 컬럼 구조를 확인하면 `embed_reference_type_id` 라는 컬럼에 `contact` 모델을 `ForeignKey` 로 참조하고 있을 것이고,
`embed_reference_id` 에는 `first_contact` 의 PK 값이 들어가게 됩니다.

이런 식으로 사용할 수 있고, 추가적으로 Django REST Framework 에서는 Django 공식 문서에서 설명하는 예시에서 어떻게 Serializer 에서 잘 사용할 수 있는지를 설명해줍니다. Django REST Framework 를 사용하신다면, 참고해봐도 좋을 것 같네요. <a href="https://www.django-rest-framework.org/api-guide/relations/#generic-relationships" target="_blank">DRF Generic Relationships 문서 링크</a>

여기까지입니다. 읽어주셔서 감사합니다 :)
