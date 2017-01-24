---
layout: post
title: "Django ALLOWED_HOSTS 관련 문제"
date: 2017-01-17
categories: Django
author: leop0ld
---

최근에 회사에서 Django 프로젝트(아주 간단한 It worked! 프로젝트) 를 Python 3.4.5 + django 1.10.5 + Nginx + uWSGI 를 사용하여 배포를 해보려고 하다가 난관에 봉착했다.

Nginx 설정도 분명히 잘 되어있고, uWSGI 설정도 문제 없어 보였다.

물론 테스트로 업로드 해보는 것이라서 `DEBUG=True` 로 하고 배포를 하려고 했었다.

배포 쪽 에러는 익숙하지 않다보니 한참 고민하던 도중에 사수님께 여쭤보았다.

아래는 생각나는대로 적은 그 떄 대화이다.

나: "사수님 이거 Nginx + uWSGI 써서 요롷게 해봤는데 안되는데 왜 안되는 지 아시나여...?!"

사수님: "엇 이거 django 버전이 몇인가요?"

나: "아마 최신버전일테니 1.10.5(최근에 릴리즈됨)일걸...요?!"

사수님: "아 그거 그럼 그 문제겠네요. 잠시만요."

곧 슬랙 DM으로 링크들이 왔다.

링크는 바로 이것들.

<a href="https://docs.djangoproject.com/en/1.10/ref/settings/#allowed-hosts" target="_blank">https://docs.djangoproject.com/en/1.10/ref/settings/#allowed-hosts</a>

<a href="https://docs.djangoproject.com/en/1.10/releases/1.10.3/#dns-rebinding-vulnerability-when-debug-true" target="_blank">https://docs.djangoproject.com/en/1.10/releases/1.10.3/#dns-rebinding-vulnerability-when-debug-true</a>

위 링크를 보면 Django 1.10.3 버전에서 바뀐 게 있다는 것이다.

그래서 무엇인지 자세히 살펴보니...

```
Changed in Django 1.10.3:
In older versions, ALLOWED_HOSTS wasn’t checked if DEBUG=True.
This was also changed in Django 1.9.11 and 1.8.16 to prevent a DNS rebinding attack.
```

내 멋대로 해석: 옛날 버전에서는 `ALLOWED_HOSTS` 가 `DEBUG=True` 일 때 체크되지 않았음. 근데 이거 **DNS rebinding** 공격 막기 위해서 바뀌었는데, Django 1.9.11 버전이랑 1.8.16 에서도 바뀜 ㅇㅇ.

Django 쪽에서도 꽤나 급하게 바꿨는 지 마이너한 버전 업그레이드에서 전부 올려버렸다.

이것이 무슨 소리냐하면 `DEBUG=True` 일 때도 `ALLOWED_HOSTS` 는 설정되어 있어야 한다.

나는 테스트 가 목적인 배포였기에 '**\***'(모두 허용) 로 설정해줬다.

실제 배포 시에는 이렇게 하는 게 매우 치명적이다. 조심하시길.

이 사항은 배포 시에 적용된다. 로컬에서 돌릴 때는 아무 문제 없을듯.

이것 때문에 Nginx + uWSGI 로 테스트 배포하는 데에 어려움을 겪어서 간단하게나마 포스팅을 해보았다.

이제 회사에서 django 관련해서 삽질을 할 일이 좀 많이 생길 것 같은데 전부 포스팅하지는 못 하더라도 중요한 것들은 기억해놨다가 이렇게 포스팅 해보려고 한다.

그게 나한테도 다른 분들에게도 도움이 많이 될테니...!

읽어주셔서 감사합니다 :)