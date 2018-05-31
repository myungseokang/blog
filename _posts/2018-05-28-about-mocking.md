---
layout: post
title: "Mocking 알아보기 with Python"
categories: posts
excerpt: "Mocking with Python"
tags: [python, test, mocking]
author: leop0ld
comments: true
share: true
date: 2018-05-31
---

## Mocking 이 필요하다고 느낀 순간

테스트 코드를 작성하다보면 가끔은 실제로 실행할 수 없는 코드들을 마주하게 된다.

예를 들어 SMS API를 호출하여 문자를 발송한다던지, 결제 API를 사용해서 결제를 한다던지 말이다.

이러한 외부 API를 테스트 코드에서 호출하게 되면 실제로 API가 작동해버릴 수 있으니 조심해야한다.

하지만 이러한 응답값에 대해 로직을 테스트하고 싶을 땐 어떻게 해야할까.

Mocking 이 그 해결책이다.

#### Mocking 이란?

Mock 이라는 단어의 사전적 의미는 다음과 같다.

- (특히 흉내를 내며) 놀리다
- 무시하다
- **거짓된, 가짜의**

Mocking 이라는 것은 실제로 값으로 테스트를 하기 어려우니 가짜 값을 사용할 수 있게 해주는 것이다.

#### Mokcing 이 필요할 때

Mocking 이라는 작업은 위에서 말했듯이 실제 값이 아닌 가짜 값을 만들어내는 것이다.

이러한 작업이 테스트를 작성하는 데에 있어서 어떠한 때에 필요할까.

길지 않은 개발 경험이지만 필요하다고 생각될 때를 적어보았다.

##### 1. 외부 API가 정상적으로 작동하지 않을 때

이때는 테스트의 결과가 보장받지 못한다.

즉, 테스트가 외부 API에 **굉장히 의존적이게 된다**.

무슨 소리냐하면 예를 들어, 내가 테스트 코드를 작성할 때에는 외부 API가 응답을 잘 해줬지만, CI에서 테스트할 때는 외부 API를 제공해주는 서비스가 뻗어버려서 테스트의 결과가 뒤죽박죽이 될 수도 있다.

이러한 경우는 외부 API에 너무 의존적이므로, 해당 API를 사용하는 부분에서 응답값을 Mocking 해줘야 한다.

##### 2. 외부 API를 호출할 때 비용이 발생하는 경우

외부 API를 호출할 때 그 횟수에 따라 비용이 발생하는 경우가 있다. SMS API 혹은 Email 관련 API가 그렇다.

이러한 경우에도 마찬가지로 외부 API에 너무 의존적인데다가, 비용까지 발생하게 되니 더더욱 실제로 호출해서는 안된다.

마찬가지로 Mocking 이 필요하다.

더 많은 무수한 경우들이 있겠지만 이 정도로 정리하고, 말만 했던 이 Mocking을 어떻게 하는지 알아볼까한다.

#### Mocking 해보기

아래의 Gist에 작성된 코드를 이용해서 설명을 해보려한다.

<script src="https://gist.github.com/Leop0ld/ee9d746fd1739ee9a58f5d74bb3b8674.js"></script>

먼저 `google_query` 라는 함수를 작성하고, 해당 함수는 `requests` 라는 라이브러리를 이용해 google 에 GET 요청을 보내고 해당 response 객체의 content 속성을 반환한다.

그 다음 TestCase 작성한다.

주목해야 할 부분은 `_mock_response` 함수와 `@mock.patch('requests.get')` 데코레이터 부분이다.

간단하게 설명하자면 `_mock_response` 함수는 `response` 를 `Mock` 객체를 이용해서 가짜로 만들어내는 함수다.

그리고 `@mock.patch('requests.get')` 데코레이터는 해당 함수를 **Monkey Patch** 하겠다는 것이다.

> Monkey Patch란 런타임에 메서드나 변수를 추가하거나 변경하는 것입니다.

그래서 테스트 함수의 2번째 인자 `mock_get` 이라는 이름의 인자를 넘겨주었고, 이제 해당 테스트 함수 내부에서 `requests.get` 을 호출하면 `mock_get` 이 대신 호출된다.

기존에 `request.gets` 은 response 객체를 반환했으므로 기존에 만들어둔 `_mock_response` 함수를 통해 가짜 response 객체를 생성하고, `mock_get` 함수의 `return_value` 속성에 넣어주면 `Mocking` 이 된 것이다.

그런 다음 처음에 만들어둔 우리가 작성한 로직에 해당하는 함수인 `google_query` 를 호출하면 `requests.get` 으로 가져오는 response 대신 `mock_get` 의 `return_value` 인 `_mock_response` 를 통해 `Mock` 객체를 받게 되고, 해당 `Mock` 객체에 `content` 속성을 반환해주기 때문에 테스트는 성공한다.

그리고 실패하는 케이스의 테스트 함수를 보겠다.

`_mock_response` 함수 중간에 보면 `raise_for_status` 에 `Mock` 객체를 대입하고, 인자로 넘어온 값이 있으면 `side_effect` 라는 속성에 값을 추가해주는 부분이 있다.

이 부분은 `requests.get` 도중에 실패했을 경우를 테스트하기 위해서 만들어둔 것이다.

`Mock` 객체의 `side_effect` 속성은 해당 객체가 호출됐을 때, 오류를 발생시킨다.

`test_failed_query` 함수를 보면 `self._mock_response` 함수를 이용해 가짜 객체를 생성할 때, 인자로 `raise_for_status` 라는 이름의 `HTTPError` 를 넘겨주고 있다.

이렇게 되면 `google_query` 함수 안에서 `raise_for_status` 함수를 호출했을 때 `HTTPError` 가 발생한다.

그래서 테스트 함수 내에선 `assertRaises` 함수를 통해 이를 확인하고 있다.

이런식으로 `Mocking` 작업을 할 수 있다.

필자도 많이 작성해보지는 않았으나 원리는 대충 이해할 수 있었다.

`Mock` 객체를 통해 테스트하기 껄끄러운 함수가 반환하는 객체를 만들어내고, Monkey Patch(`@mock.patch` 데코레이터) 를 통해 함수를 `return_value` 가 만들어둔 `Mock` 인 함수로 바꿔치기 해주면 끝이다.

생각해보면 그리 어렵지도 않은 것인데 처음 접하다보니 어려웠던 것 같다.

그동안 `Mocking` 에 대해 개념만 대충 알고 있었는데 코드로 정리를 하고나니 개운하다.

앞으로도 블로그 열심히 해야겠다.

읽어주셔서 감사합니다 :)

> 참고한 곳
> - [mock_requests.py](https://gist.github.com/evansde77/45467f5a7af84d2a2d34f3fcb357449c) 원본
> - Python 공식 문서 - [unittest.mock](https://docs.python.org/3/library/unittest.mock.html)
> - 테스트 관련하여 참고하면 좋을만한 글: [유닛테스트에 대한 생각](https://blog.outsider.ne.kr/1275) - Outsider
