---
layout: post
title:  "Django 기초 알아보기"
date:   2016-07-03
excerpt: "Learning Django basic"
tag:
- Python
- Django
comments: true
---


# Django 기초


## 1. MVC, MTV

- MVC(Model, Controller, View)
 - 어플리케이션을 세가지 역할로 구분한 개발 방법론!
 - Model: Data를 안전하게 저장하는 역할
 - Controller: Model을 통해 Data를 가져오고 그 Data를 View에게 전달해주는 역할
              \+ View에서 넘어온 Data를 잘 가공해서 Model에 저장하는 역할
 - View: Controller에서 넘어온 Data를 표현해주는 역할


- MTV(Model, Template, View)
 - Model: Data를 안전하게 저장, MVC의 Model과 똑같은 역할
 - Template: 표현 관련된 것들을 다루는 MVC에서 View의 역할
 - View: Model과 Template의 징검다리 역할을 하는 MVC의 Controller 역할

MTV 코딩 순서
 - 보통은 Model을 먼저 코딩하고, 그 다음 View와 Template을 코딩하는 것이 일반적
 - CBV의 경우에는 Model을 코딩하고 View가 매우 간단하기 때문에 View를 코딩한 뒤 Template을 코딩하는 방식


## 2. Django 개념

- Django: Python 기반의 오픈소스 웹 어플리케이션 프레임워크


## 3. settings.py

- Database, Static files, app 등등 Django에 관한 여러가지 설정들을 다루는 파일입니다.
- 필수 사항: 데이터베이스, 템플릿, 정적 파일, 애플리케이션, 타임존
- settings.py를 알아두면 Django에 대한 전반적 이해가 수월!


## 4. models.py

- 테이블을 정의하는 부분
- ORM(Object Realation Mapping)을 이용해서 클래스와 데이터베이스의 테이블을 매핑해줌
- 데이터베이스에서 하나의 테이블은 하나의 클래스로, 데이터베이스의 컬럼은 클래스의 속성으로 매핑시킴

- **syncdb** vs **makemigrations**, **migrate**
 - syncdb: Django 1.7 미만 버전 이전에 쓰이던 방식(나는 안써봐서 모름)
 - makemigrations, migrate: Django 1.7 이상 버전에서 쓰이는 방식
  - 데이터베이스에 대한 변경사항을 파일로 저장해주고(makemigrations)
  - 변경사항을 직접 데이터베이스에 적용하는 방식(migrate)


## 5. views.py

- 로직을 코딩하는 부분! 중요함
- 가독성과 유지보수를 생각하면서 코딩해야함
- 함수형 뷰(Function-Based View)와 클래스형 뷰(Class-Based View)로 나뉨
- 잘 쓰면 CBV가 훨씬 간편하고 좋은듯(주관적인 의견)


## 6. urls.py

- URL을 정의할 때는 urls.py를 앱 안에 만듦으로서 여러개로 나눌 수 있음
- 보통 위의 방식을 쓰지만 간단할 경우에는 프로젝트의 urls.py 한개에 몰아넣을 수도 있음
- 여러 개의 urls.py로 분리하면 재사용 가능성 증가
- URL 패턴별로 이름을 부여할 수 있고, 패턴그룹에 대해 namespace도 지정이 가능함
- reverse나 url 템플릿 태그에서 매우 중요한 부분


## 7. templates

- templates(\*.html)파일들을 모아두기 위한 디렉토리


진행중!
