---
layout: post
title: "django 프로젝트 배포하기 - AWS Elastic Beanstalk"
date: 2017-06-26
categories: django
author: leop0ld
---

> 이 글은 macOS 기준으로 설명되었습니다.

꽤나 오랜만에 블로그 포스팅입니다.

최근 장고걸스 활동을 하면서 느낀 건 "**역시 배포를 해야 뭔가 완성되는 느낌을 받는구나**" 라는 것이었습니다.

그래서 django 를 포하는 방법에 대해서 조금 알아볼까 합니다.

제가 좋아하는 AWS의 PaaS(Platform as a Service)인 **Elastic Beanstalk**(이하 EB) 라는 서비스를 이용해서 배포하는 방법을 알아보고자 합니다.

![AWS Elastic Beanstalk](/assets/img/aws-elastic-beanstalk.jpg)

AWS 에서 제공하는 PaaS 서비스인 EB 는 자체만으로는 과금이 되지 않습니다.

다만 RDS, EC2, Elastic Load Balancer, EBS 등등을 사용하게 되면 과금이 될 수 있으니 **주의**하세요!

<script async src="//pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-format="fluid"
     data-ad-layout="in-article"
     data-ad-client="ca-pub-1864899826477546"
     data-ad-slot="2703362319"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

### 0. virtualenv 만들기

저는 pyenv, virtualenv 를 시용하기 때문에 아래와 같이 가상환경을 만들고 활성화합니다.

![pyenv, virtualenv](/assets/img/pyenv-virtualenv.png)

pyenv, virtualenv, autoenv 에 대한 글은 [여기](https://leop0ld.github.io/python/2016/12/08/using-pyenv.html)를 참고해주시면 됩니다.

그리고 django 를 설치해줍니다.

![Install django](/assets/img/install-django.png)


### 1. django 프로젝트 만들기

먼저 django 프로젝트를 만들어보도록 하겠습니다.

```shell
$ django-admin startproject leopold
```

위의 명령어를 통해서 django 프로젝트를 시작할 수 있습니다.

그리고 프로젝트 폴더로 이동한 뒤, Migration 작업을 해줍니다.

```shell
$ python manage.py migrate
```

아래의 명령어를 통해 잘 만들어졌는지 확인할 수 있습니다.

![runserver](/assets/img/runserver.png)

이렇게 서버를 시작시킨 뒤, 브라우저를 열고 localhost:8000 에 들어가서 `It worked!` 페이지가 정상적으로 뜨면 성공입니다.


### 2. Elastic Beanstalk 시작하기

저는 `awsebcli` 라는 cli tool 을 통해서 EB 를 시작해보려고 합니다.

아래와 같이 pip 를 이용해서 설치할 수 있습니다.

![Install awsebcli](/assets/img/install-awsebcli.png)

설치전에

```shell
$ brew update
```

도 잊지 말아주세요!

<!-- TODO: EB 부분 추가해야함 -->
