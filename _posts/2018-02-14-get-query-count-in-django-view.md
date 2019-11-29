---
layout: post
title: "Django view 안에서 쿼리 개수 확인하기"
categories: posts
excerpt: "Get query count in Django view"
tags: [django]
author: myungseokang
comments: true
share: true
ads: true
date: 2018-02-14
---

> 이 글은 Django 2.0 기준으로 작성되었습니다.

가끔 개발하다가 오른쪽 옆에 떠있는 <a href="https://github.com/jazzband/django-debug-toolbar" target="_blank">django debug toolbar</a> 의 SQL 패널을 보고 쿼리 개수를 확인하는 경우가 많다.

하지만 json response 를 return 하는 View 경우에는 debug toolbar 로 확인하는 것도 한계가 있다.

그럴 떄 쉽게 View 단에서 쿼리 개수를 console 에 나타낼 수 있는 방법을 소개해볼까 한다.

<script src="https://gist.github.com/myungseokang/ecd3993980d2204c317390da8b1668ad.js"></script>

필자가 작성한 `connection.queries`는 `sql`과 `time`이라는 속성을 가진 Dictionary의 List를 반환하고 있다.

띠리서 해당 List에 대해 `len()`을 실행하면 쿼리의 개수를 알 수 있다.

게다가 아래 Snippet 에선 Dictionary 안에 있는 `time`값을 통해 특정 View 에서 걸리는 쿼리의 시간을 확인할 수도 있다.

하지만 어디까지나 django debug toolbar 를 못쓸 때의 이야기이다.

쓸 수 있다면 무조건 debug toolbar 로 확인하는 게 다방면에서 좋은 것 같다.

<a href="https://docs.djangoproject.com/ko/2.0/faq/models/" target="_blank">해당 공식문서 바로가기</a>

감사합니다 :D
