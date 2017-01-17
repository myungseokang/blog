---
layout: post
title: "Two scoops of Django 김승진 역자님과 미팅 with Django Girls"
date:   2016-09-23
excerpt: "Two scoops of Django meeting with Django Girls"
categories:
- Django
- Django Girls
- Two Scoops of Django
comments: true
---

김승진 역자님과 미팅 with Django Girls
=====


간단하게나마 오늘 들었던 것을 정리해보려 합니다.

## CBV vs FBV

two scoops of Django 의 원저자는 CBV를 굉장히 선호한다고 합니다(그래서 책에서도 많이 추천함)
하지만 김승진 역자님은 FBV를 선호하신다고 하셨습니다.

## setting 부분, architecture 부분, test 부분

이렇게 3가지 부분이 다른 책과는 비교되는 부분이라고 말씀해주셨고, 꼭 Django뿐만이 아니더라도 쓸모가 많은 책이지만 너무 얕게 다룬다고 하셨습니다.
3가지 중에서 제일 기억나는 부분은 역시 setting 부분 이었습니다.
이야기 나온 것은 아래와 같습니다.

Djanog 1.6 -> 설정 파일 패턴 EX) secret.json

Django 1.8 -> 환경 변수 패턴 EX) export SECRET_KEY="my-secret-key"
Git, 코드 자체의 보안 키가 없다는 장점이 있는 반면, linux 지식이나 환경 변수에 대한 지식이 필요하다고 하셨습니다.

Apache에서는 환경 변수가 지원이 안됨
Nginx에서는 환경 변수 지원이 됨

회사의 웹서버는 Apache이기 때문에 설정 파일 패턴을 사용할 듯 합니다(물론 개인 프로젝트 새로이 시작하면 바로 환경 변수 패턴으로 갑니다)

그리고 역시 제일 많은 질문!

## App design 
> 정답없음 / 개인적인 생각은 1개의 기능만 하는 앱을 만드는 것이 좋아보임

정답이 없다고 하십니다.

김승진 역자님은 양방향 으로 오고가는 걸 선호하셔서 따로 pt같은 건 준비 안하시고 이런저런 정보를 공유했습니다.
사후 pt라는 걸 만들어주셔서 나눠주신다고 하셨습니다(미팅이 끝난뒤 이야기한것을 바탕으로 pt를 제작해서 나눠주신다고 하셨습니다)

Django 공부를 더 해야겠다는 걸 많이 느꼈고, Django Girls에서 강의 요청(장난식으로 ㅋㅋ) 왔습니다만 강의할 수 있다면 해보고 싶습니다.

이상 오늘 미팅 간단 정리였습니다. 감사합니다(추후 pt 받으면 링크는 올려드리겠습니다.)


