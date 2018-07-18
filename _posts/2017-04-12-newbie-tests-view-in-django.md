---
layout: post
title: "테스트 초보의 테스트 삽질기 with Django"
categories: posts
excerpt: "Newbie's test shovel with Django"
tags: [test, python, django]
author: leop0ld
comments: true
share: true
ads: true
date: 2017-04-12
---

Test all the things!
---

<img src="/assets/img/testallthethings.png" style="width: 100%;" alt="Test all the things!" />

2017.05.02 UPDATE
---

`django.test.Client` 의 `login()` 메서드는 email 로그인이 가능했다...

```python
self.client.login(email='l3opold7@gmail.com', password='password')
```

위와 같이 작성할 경우 로그인 처리가 된다. ( ~~역시 Django가 이런 처리를 안해놨을 리가 없어!~~ )

그리고 또한 csrf token을 쓸 일이 꽤 있어서 TestCase의 메서드로 빼기로 했다.

```python
class BlahTest(TestCase):
    def get_csrftoken(self):
        """
        Get csrf token
        """
        response = self.client.get('/')
        return response.cookies.get('csrftoken')
```

이런 식으로 빼서 `self.get_csrftoken()` 식으로 썼다.

<br>

최근에 POST 메서드를 처리하는 View 를 다시 볼 일이 있어서 **아 이제는 진짜 제대로 된 테스트 코드를 짜보자!** 하는 마음으로 테스트 코드를 짜보기로 했다.

지금 현재 프로젝트에는 [`circleci`](circleci.com) 가 붙어있고, [`pytest-django`](https://pytest-django.readthedocs.io/en/latest/) 와 [`flake8`](http://flake8.pycqa.org/en/latest/) 를 사용하고 있다.

(Tip! pytest 세팅은 [여기](https://gist.github.com/perhapsspy/fc5521376df87036be2d034d1beaf69b) 를 참고해봤다. 존경하는 분의 글이라 그런지 더 좋게 느껴졌다.)

사실 **테스트를 짜자!** 라는 동기를 부여하기 위해 무리해서 전부 붙여봤다. ( ~~감당 안되면 전부 떼내야지...~~ )

여튼 그동안은 기능 구현하는 데에 급급해서 테스트 코드 없이 코드를 짜기 일쑤였지만 최근 들어 여러가지 글을 읽고서는 진짜 이대로 가다가는 테스트 코드를 안짤 것 같아서 **바로 실행에 옮기기**로 했다.

요번에 Django 버전 업그레이드 할 때도 몸소 체험했기도 하고.

그래서 마음을 다잡고 **내가 어떤 로직에 대해 먼저 테스트를 해보는 게 좋을까** 생각해봤다.

호기롭게 도전
---

역시 사용자에게 보이는 부분이 중요할 것 같아서 user와 관련된 `views.py` 에 대한 테스트 코드르 작성하기로 했다.

이름은 그냥 파일 이름 뒤에 `_test` 를 붙여서 만들었다. (Test 파일 이름을 생성하는 부분은 Golang에서 모티브를 얻었다. pytest-django 에서 지원을 해주기도 했고.)

Two scoops of Django 책에서 봤는데 Django 에서 기본적으로 생기는 `tests.py` 지우고 각각 파일에 대한 test 파일을 만들라고 봤던 기억이 있어서 위와 같이 했다.

그래서 막상 뭘 해야될 지 감을 못 잡겠었는데 일단 무작정 `django.test.TestCase` 를 상속받는 클래스를 작성해봤다.

처음에는 간단한 뷰의 status_code를 비교하는 테스트 메서드를 작성해보고, "이제 POST 메서드를 처리는 View 함수도 테스트할 수 있겠군!" 싶어 도전해봤다.

테스트 코드가 익숙치 않아서 좀 많이 헤맸다.

처음 내가 시도한 방법은 대충 아래와 같다.

```python
from django.test import Client, TestCase
from model_mommy.recipe import Recipe

from blahblah.models import User
from blahblah.views import blah_view


class UserViewTest(TestCase):
    def setUp(self):
        self.client = Client()
        self.user_recipe = Recipe(User)

    def test_view_using_post_method_blah_blah(self):
        user = self.user_recipe.make()

        self.client.login(user)

        post_data = {
            'name': 'leop0ld',
        }

        request = self.client.post(reverse('blah:blah'), data=post_data, follow=True)
        request.user = user

        response = blah_view(request)

        self.assertRedirects(response, [(reverse('blah:blah2'), 302)])
        self.assertEqual(user.name, 'leop0ld')

```

이렇게 시도했던 것 같은데 에러가 났다.

어떤 에러인지 보아하니 `HttpResponseForbidden` 은 redirect 뭐시기를 가지고 있지 않는다는 에러였다.

그렇다는 건 위 코드의 `response` 가 `HttpResponseForbidden` 라는 건데, 원래는 `HttpResponseRedirect` 여야 했다.

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

실패를 겪고 다시 도전
---

대충 짐작컨대 `CSRF token` 문제라고 생각했다.

근데 이 문제를 해결할 수 있는 방법이 떠오르지 않았다... ㅠㅠ

그래서 이런저런 곳들([9XD Slack](https://festi.kr/zlack/team/T0ZAS4K7Z/29/), [Django Korea Slack](https://festi.kr/zlack/team/T0E4AM2AF/5/) 등등) 에 질문을 해봤다.

9XD Slack의 #python 채널쪽에서 관련되어서 답변을 잘해주셔서 여러가지 방법으로 시도를 해볼 수 있었다.

그 중에서 heechan(@yangroro) 이라는 닉네임을 쓰시는 분이 추천해주신 방법을 써봤다.

추천해주신 방법은 **GET request를 날린 뒤 csrf token을 얻어서 POST request 를 날려보는 것** 이었다.

꽤나 사용자 시나리오? 와 비슷하게 테스트를 하는 것 같아서 작성해보고 있었는데 외부 라이브러리를 사용하지 않으면 꽤나 복잡하게 작성해야했다.(물론 제가 매우 생각이 짧고 지식이 얉아서 그런 것일수도 있을 것 같습니다.)

아래는 내가 매우 노력한 코드다.

```python
from django.test import TestCase, RequestFactory, Client
from django.urls import reverse
from model_mommy.recipe import Recipe

from blahblah.views import blah_view
from blahblah.models import User


class UserViewTest(TestCase):
    def setUp(self):
        self.client = Client(enforce_csrf_checks=True)
        self.factory = RequestFactory()

        self.user_recipe = Recipe(User)

    def test_post_blahblah(self):
        user = self.user_recipe.make()

        # GET request 보내기 위한 세팅
        get_request = self.factory.get(reverse('blah:blahblah'))
        get_request.user = user
        get_request.session = self.client.session

        get_response = blah_view(get_request)

        # get_response 에서 csrftoken 가져옴
        csrf_token = get_response.cookies['csrftoken']

        # POST를 보내기 위한 Form 데이터
        post_data = {
            'name': 'leop0ld',
            'csrfmiddlewaretoken': csrf_token,
        }

        # POST 보내기 위한 세팅
        post_request = self.factory.post(reverse('blah:blahblah'), data=post_data)
        post_request.user = user
        post_request.session = self.client.session
        post_request.COOKIES['csrftoken'] = csrf_token

        response = blahblah(post_request)

        # POST 처리된 뒤 결과 비교
        self.assertEqual(response.status_code, 302)
        self.assertEqual(user.name, 'leop0ld')

```

현재 테스트를 하려는 View 함수에서 `@ensure_csrf_cookie` 라는 데코레이터를 사용하는데 이 데코레이터는 CSRF token 을 Cookie 에 보내도록 강제하는 데코레이터이다.(공식 문서 [링크](https://docs.djangoproject.com/en/1.10/ref/csrf/#django.views.decorators.csrf.ensure_csrf_cookie))

따라서 Cookie 에서 가져오는 선택을 했고, `Client` 를 이용해서 POST request 를 만들 경우에는 이상하게 에러가 났다.

그래서 그냥 `RequestFactory` 를 이용해서 날코딩을 하게 됐다. (몰랐던 사실인데 `Client` 클래스는 `RequestFactory` 클래스를 상속받아 만들어진 클래스다.)

이 테스트가 통과하고 너무 기쁜 나머지 9XD Slack에서 미쳐날뛰었는데( ~~하라는 일은 안하고~~ ), @jelly 라는 닉네임을 사용하시는 분이 더 개선된 코드를 보여주셨다.

jelly(@kyunooh) 님이 추천해주신(사용하신 이라고 하는 게 더 맞을지도) 방법은 [`django-test-plus`](https://github.com/revsys/django-test-plus) 라는 외부 라이브러리를 사용하는 방법이었다.

그 코드는 바로 [9XD repository 의 이 곳](https://github.com/9XD/9XD/blob/master/posts/tests.py) 에 있다.

보니까 허무할만큼 간단한 코드였다.

당시에 조금 현자타임이 왔었지만 정신을 다잡고, `django-test-plus` 를 설치해서 사용해봤는데, 이런저런 문제가 있어서 아쉽게 사용하지 못했다.

이유는 한 가지다.

**`test_plus.TestCase.login()` 메서드의 email login 미지원**

말 그대로 `test_plus.TestCase.login()` 메서드가 있는데 이 메서드가 **email login을 지원하지 않는다**.

코드를 까보면 `USERNAME_FIELD` 로 받긴 하는데 내가 시도해본 결과 로그인 처리가 안된다. (코드만 놓고보면 될 것 같긴 한데...)

이슈를 남기든 뭘하든 해봐야겠다.

게다가 현재 프로젝트에서는 `UserManager` 클래스를 개조해놓은 상태라서 `make_user()` 메서드도 사용하지 못한다.

괴랄한 `Uesr` 모델과 `UserManager` 클래스 덕분에 꿀 같은 라이브러리를 쓰지 못한 것은 좀 아쉽지만 그래도 날코딩 하는 게 재미없는 것은 아니니 됐다.

성능 문제는 ......애도를 표한다. 좀 더 개선해볼 방법을 생각해봐야겠다.

결론
---

결론은 **테스트는 연습이다**. 팩트다.

다양한 경우에 대해서 많이 테스트를 작성해본 사람이 그것에 대해 솔루션을 잘 알고 있고, 좀 더 제대로 된 방법으로 테스트할 수 있다고 생각한다.

그래도 테스트에 대한 주워들은 이야기는 많아서 그런 게 도움이 됐던 것 같다.

2016년에 생각했던 2017년 새해 목표가 **테스트 코드와 친해지기** 였던만큼 좀 더 노력할 필요가 있다는 걸 4월이나 되어서야 깨달았다.

읽어주셔서 감사합니다 :D
