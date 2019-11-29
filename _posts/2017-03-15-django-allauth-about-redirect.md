---
layout: post
title: "django-allauth 소셜로그인 후의 redirect에 대해"
categories: posts
excerpt: "About redirect after social auth at django-allauth"
tags: [python, django, django-allauth]
author: myungseokang
comments: true
share: true
ads: true
date: 2017-03-15
---

이번 포스팅은 django-allauth를 사용했을 때 소셜로그인 한 뒤에 redirect 하는 것에 관련된 자료가 스택오버플로우나 다른 곳에 정리된 것이 없어서 정리해보려고 한다.

## 원래는 작동했었는데...

나는 회사에서 `django-allauth` 라는 라이브러리를 사용한다.

근데 오늘 관련해서 처리를 해야될 일이 있어서 처리를 하려고 했다.

회사의 로그인 / 회원가입 방식이 다른 웹사이트와는 다르게 팝업창을 하나 띄우고 그 안에서 비동기로 처리한다음 성공하면 메인페이지의 userBox만 JS쪽에서 업데이트 해준다.

response가 정상적으로 오지 않을 경우에만 `window.location.reload()` 를 이용해서 reload를 한다.

그리고 이것들은 정상적으로 작동하는 로직**이었다**.

이러한 전제를 깔고 가보도록 하겠다.

## 1차 멘붕의 시작

Social App의 Provider를 Facebook으로 해서 하나를 추가해봤다.

테스트용 앱의 ID와 SECRET KEY를 넣고 등록도 했다.

그리고 settings 쪽에 관련 설정 부분도 넣어주고, version 명시까지 해줬다.

이때까지만 해도 **완벽히 작동**할 줄 알았다.

그리고 JS쪽은 기존 코드가 있어서 크게 수정할 부분이 없다고 판단해서 잘 되겠지 하고 테스트를 해봤는데 userBox도 변하지 않을 뿐더러 reload도 되어버리는 일이 발생했다. (여기서 1차 멘붕...)

도저히 원인을 그냥 찾기는 힘들 것 같아서 JS 디버깅을 시도헀다. ( ~~Chrome 진짜 잘만들었다. 사랑합니다~~ )

jQuery로 Ajax 날리는 부분에서 URL 부분이 이상했다.

그래서 디버깅을 해봤더니 혹시나가 역시나. 웬 이상한 곳의 url을 return 해서 수정하고 다시 디버깅.

그런데 또 이상한 값이 들어왔다. 어떤 값인지 자세히 이야기는 못하지만 뭔가 바뀌긴 했었다.

그래서 JS 쪽을 계속 뒤져봤는데 이거 아무래도 이상하다.

JS 쪽에는 잘못이 없다고 판단.

하지만 break point는 계속 걸어놓고, 이번엔 python 파일을 뒤적거렸다.

JS와 Python을 같이 보던 도중 개발자 도구의 `Network` 탭을 봤는데 Ajax에 대한 HTTP Status code가 302였다. (사실 이것도 Preserve log 옵션을 켜서 확인했다 ㅠㅠ...)

302는 Redirect 를 나타내는 HTTP Status code이다.

여기서부터 오기가 돋았다.

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

## 2차 멘붕의 시작

Python 에서 `HttpResponseRedirect` 를 return 했다고 판단. 그래서 Python도 Pycharm을 이용해서 break point를 찍고 디버깅 모드로 실행했다. ( ~~JetBrain 사랑합니다. 디버깅 짱편함~~ )

break point를 찍고 한 줄씩 실행시켜 가면서 하나하나 확인해봤다.

문제는 django-allauth 쪽에서 정의되어 있는 `login_by_token` 이라는 함수였다. (<a href="https://github.com/pennersr/django-allauth/blob/master/allauth/socialaccount/providers/facebook/views.py#L73" target="_blank">Github Link</a>)

여기서 보면 `ret` 이라는 놈을 return 한다.

이 놈은 <a href="https://github.com/pennersr/django-allauth/blob/master/allauth/socialaccount/providers/facebook/views.py#L114" target="_blank">여기</a> 서 값을 return 받는다.

Pycharm 디버깅 모드에서는 한 depth를 파고 들어갈 수 있는 버튼이 있다.

그걸 이용해서 파고 들어가봤다.

그렇게 파고 들어가다보니 <a href="https://github.com/pennersr/django-allauth/blob/master/allauth/socialaccount/helpers.py#L56" target="_blank">이것</a> 과 마주쳤다.

이 `perform_login` 함수가 거의 문제의 근원이었다.

이걸 다시 파고 들어가보면 <a href="https://github.com/pennersr/django-allauth/blob/master/allauth/account/utils.py#L146" target="_blank">이것</a> 과 마주친다.

이 떄쯤 2차 멘붕...

결국 response는 `HttpResponseRedirect` 로 생성되는 것이었다.

## 임시 해결방법

처음에는 이 함수를 오버라이딩 해버릴까 생각해봤지만 별로 좋지 못한 생각이라는 것을 5초만에 깨닫고, 다른 방법을 생각하다가 결국 사수님이 추천해주신 방법으로 해결했다.

사수님이 추천해주신 방법은 지정해준 URL로 Redirect를 시켜주니 하나의 JSON을 리턴해주는 View 함수를 만들어서 그 View 함수의 URL을 넘겨주면 request의 양은 1개 늘지만 처리가 가능하다고 말씀해주셨다.

그래서 그 방법으로 시도해보니 로컬에서는 아주아주아주 잘되었다.

너무나 행복했다.

하지만 근본적으로 해결한 것이 아니고 이 방법을 사용하게 되면 request가 하나 더 늘게 되니 좀 꺼려지는 부분이 있다.

이 부분에 대해서 솔루션을 거의 오후 내내 찾아봤는데 찾지 못했다. (아시는 분 계시면 코멘트로 말씀 좀 남겨주세요...)

결국 깔끔하진 않지만 처리를 해놨고, 당연히 배포 시에도 잘되는지 테스트를 해봐야했기에 테스트 서버에 배포를 해봤다.

근데 배포 시에 Facebook App의 문제인지 잘 작동하지 않았다...

이성의 끊을 놓을 뻔 했으나 로직 상의 문제는 아닌 거 같은데 아직 해결 못했다.

곧 해결할 수 있지 싶긴하다.

## 결론

회원가입 / 로그인 을 Ajax로 처리하는 건 좋지 못한 것 같다.

django-allauth 도 페이지 전환하는 방식을 두고 만들어진 것 같다.

다만 Ajax로 처리할 수 있게끔 솔루션이 존재하지 않는 것은 조금 아쉽다.

그리고 지금 해결한 솔루션이 최선의 방법인 것 같지도 않다.

좀 더 뒤적여봐야 알겠지만 아마도 더 깔끔하게 처리할 수 있는 방법이 있을텐데 내 머리가 안좋아서 아직은 잘 모르겠다.

좀 더 고민해봐야겠다.
