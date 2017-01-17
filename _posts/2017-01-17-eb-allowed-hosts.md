---
layout: post
title: "AWS Elastic Beanstalk Health Check 관련 문제"
date:   2017-01-17
excerpt: "Problem about Health Check on AWS Elastic Beanstalk"
tag:
- Django
- Elastic Beanstalk
comments: true
---

이 부분은 월요일 장고 스터디에서 주워들은 내용이다. 일단은 직접 겪어보지는 못했고, **겪을법한 일** 이라서 정리를 해두려고 한다.

AWS에는 **Elastic Beanstalk** 라는 서비스가 있다. 이 서비스는 따로 서비스로 존재하지만 사실상 AWS의 EC2 와 S3, RDS 를 자동으로 관리해주는 서비스이다.

AWS Elastic Beanstalk 는 웹 어플리케이션을 쉽고 빠르게 배포할 수 있게 해주기 때문에 웹 개발자인 나로서는 관심이 안 갈래야 안 갈 수가 없었다.

더군다나 이번에 회사에서 Django 버전을 업그레이드 함과 동시에 Elastic Beanstalk 를 사용하자! 라고 결정이 나서 더욱 관심있게 보고 있는 서비스 중에 하나이다.

그런데 관련해서 월요일 장고 스터디 모임에서 한 분이 이런 말씀을 해주셨다.

**"Elastic Beanstalk 에서 Health Check 라는 걸 해주는데 그 부분에 있어서 Django 의 ALLOWED_HOSTS 쪽에 따로 처리가 필요합니다."**

이야기를 듣고 아직 Elastic Beanstalk 를 써보지 않았던 나는 꽤나 흥미로웠던( ~~잘하면 회사가서 자랑할 수 있을지도...?~~ ) 주제여서 관련 링크를 찾게되면 보내달라고 말씀을 드렸다.

그 링크는 <a href="https://dryan.com/articles/elb-django-allowed-hosts/" target="_blank">이 곳</a> 이다.

그리고 Elastic Beanstalk 와 Django 사이의 문제점을 정리해놓은 좋은 링크가 있어서(사실 저도 사수님께 받음) 적어놓으려고 한다.

이 링크는 <a href="https://blog.hashedin.com/2017/01/06/5-gotchas-with-elastic-beanstalk-and-django/" target="_blank">이 곳</a> 이다.

위에 링크의 해결법인즉슨, EC2 머신의 Private IP를 Django 세팅 쪽에서 잡아줘서 ALLOWED_HOSTS 쪽에 추가해주는 식이다.

그렇게 깔끔하지는 않은 방법일 수는 있지만 그나마 제일 간단하게 해결한 게 아닐까 싶다.

다르게 해결하는 방법도 분명 존재하겠지만, 아직 문제가 눈 앞에 닥친 게 아니기 때문에 이 정도만 언급하겠다.

관련해서 아직 직접적으로 문제를 겪어보지는 못했으나, 혹시나 도움될 수 있는 사람들을 위해 혹은 미래의 나를 위해 적어놓으려고 한다.

읽어주셔서 감사합니다 :)