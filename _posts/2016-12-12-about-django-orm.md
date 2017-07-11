---
layout: post
title: "django ORM에 대해"
categories: posts
excerpt: "About django ORM"
tags: [django]
author: leop0ld
comments: true
share: true
date: 2016-12-12
---

django ORM에 대해
====================
> 부족한 부분은 댓글로 알려주시면 감사하겠습니다.

요즘 django의 조금은 아랫단에 대해서 점차 생각이 들기 시작했다.

최근에 Flask도 접하고 있기 때문에 그 중에 특히 관심이 생기는 부분은 아무래도 **SQLAlchemy** 와 **django ORM** 그리고 **Jinja2** 와 **django Template** 인 것 같다.

물론 템플릿 엔진 부분은 따로 다루는 포스팅이 있으니 [여기](https://leop0ld.github.io/about-template-engine/)서 확인을 바랍니다.

이번 포스팅에서는 django ORM이 뭔지, 어떤 일을 해주는지, 간단한 사용법과 그것에 대한 예제(django 문서가 더 잘나와있을 것 같기는 함) 등 내가 아는만큼만 써보려고 한다.

사실 나는 django 접한 것 자체는 그리 오래되지 않았다.

django는 작년 겨울방학에 학교에서 진행하는 **소수전공** 이라는 프로그램에서 처음 접했는데, 그마저도 원래는 Flask로 진행되는 것이었지만 강사님이 django로 진행을 해주셔서 처음 접했다.

그 강사님께 감사를 표한다.

django를 처음 접했을때는 정말 신세계였다.

그래서 django를 이용해서 이것저것 했던 기억이 있다.

그렇게 재밌을 수 있었던 이유 중에 하나가 **django ORM** 이었다고 생각한다.

**그래. django가 뭔지는 알겠는데... ORM은 뭐야?**

`ORM`은 "**Object-relational mapping**" 의 약자이다.

ORM은 객체지향적인 언어와 RDBMS 시스템을 매핑해서, 객체를 이용해서 데이터를 다룰 수 있게끔 해주는 기술이다.

객체지향적 언어에 존재하는 Class의 개념은 RDBMS의 Table과 유사하다.
이 특징을 이용해서 객체지향적 언어 쪽에서 편하고 쉽게 데이터까지 다룰 수 있게해준 나한테는 아주 감사한 기술이다.

다만 표현이 아주 다르기 때문에 이렇게 사용하기 위해서는 매핑을 통해서 변환을 시켜줘야 합니다.

이렇게 객체지향적 언어 쪽에만 신경을 쓴다면 SQL쪽도 매핑을 통해 처리해주는 ORM 이 있으니 개발자의 입장에서는 신경 쓸 부분을 하나 덜었다고 할 수 있다.

이 부분이 ORM을 사용하는 아주 큰 메리트라고 생각한다.

ORM 자체는 사실 그렇게 효율이 좋지 않다. 효율을 생각하면 raw 쿼리를 작성하는 게 더 도움이 된다.

그렇지만 나는 raw 쿼리를 헤비하게 짤 실력 좋은 개발자도 아니고 일개 주니어 개발자다.

SQL 쿼리문까지 신경 쓰기에는 django 하나의 개념을 전부 알기도 바쁘다.

**ORM을 사용하면 내가 SQL에는 신경을 덜 쓸 수 있게 해주고, 다른 곳에 더 시간을 투자할 수 있다.**

심지어 RDBMS를 전혀 모르고 객체지향적인 사고로 접근해도 원하는 결과를 꽤나 쉽게 얻을 수 있기때문에 좋은 기술이라고 생각한다.(그러니 사용되고 있겠지만)

변환되는 부분은 내가 자세히 살펴보지 않았고, 살펴봤다고 해도 raw 쿼리를 잘 몰라서 설명하는 데에 지장이 있을 것 같으니 패스 하겠다.

그래서 django를 이용해 ORM의 간단한 사용법을 위해 예를 들어보겠다.

{% highlight python %}
class Car(models.Model):
    name = models.CharField(max_length=32)
    price = models.PossitiveIntegerField()
{% endhighlight %}

이러한 모델이 있다고 가정하겠다.

요롷게 사용할 수 있다.

{% highlight python %}
Car.objects.all() # 전체 목록을 QuerySet 리스트로 반환
Car.objects.get(조건) # Car들 중에 조건에 맞는 단 1개의 객체만 반환
Car.objects.filter(조건) # Car들 중에 조건에 맞는 객체들의 리스트를 반환
Car.objects.create(인자=값) # Car 객체를 인자로 넘겨준 값으로 생성
{% endhighlight %}

등등... 더 많은 정보를 알고 싶다면 [django 공식문서](https://docs.djangoproject.com/en/1.10/ref/models/querysets/)로 가면 많은 것들을 확인할 수 있다.

<del>django 처럼 문서화 잘되어있는 오픈소스 프로젝트도 드묾 django 하세요 django</del>

이상 django ORM 자랑을 마치고... 원래는 SQLAlchemy와 비교 도 해보고 싶었으나 SQLAlchemy에 대한 숙련도도 부족하고(사실 django ORM도 잘 쓰지는 않는거같고...) 해서 비교는 어려울 것 같고, 다음에 SQLAlchemy와 django ORM을 좀 더 잘 다루게 되었을 때를 기약하겠습니다.

읽어주셔서 감사합니다 :D