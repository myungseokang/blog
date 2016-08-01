---
layout: post
title:  "Python 기초 문법 알아보기"
date:   2016-07-03
excerpt: "Keep Calm and Code Python."
tag:
- Python
- Django
comments: true
---

## Django 기초

### 1. MVC, MTV

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


### 2. Django 개념
- Django: Python 기반의 오픈소스 웹 어플리케이션 프레임워크


### 3. settings.py, manage.py
- settings.py: Database, Static files, app 등등 Django에 관한 여러가지 설정들을 다루는 파일입니다.

- manage.py:
