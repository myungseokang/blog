---
layout: post
title: "Planet Hackathon 후기"
categories: posts
excerpt: "Review of Planet Hackathon"
tags: [hackathon]
author: myungseokang
comments: true
share: true
ads: true
date: 2018-09-03
---

> 이러한 행사를 준비하고 운영해주신 운영진분들께 감사를 표합니다.

<img src="/assets/img/planet-hackathon-banner.jpg" alt="Planet Hackathon">

9월 1일, 2일 나는 **Planet Hackathon**에 다녀왔다.

작년 9XD에서 주관했던 해커톤의 경우, 주의사항을 잘 숙지하지 못해 참가자 선발에서 떨어졌었다.

그 결과, 올해는 내가 놓친 주의사항이 없는지 꼼꼼하게 체크하는 계기가 되었다.

주의사항에 맞게 양식을 기입하고, 양식에 기입할 내용도 꽤 고민하여 작성했다.

다행히도 3수는 면할 수 있었고, 참가자로 선발되어 GDG Seoul과 9XD가 주관한 Planet Hackathon에 참가하게 되었다.

## Hackathon 시작

나의 팀은 3팀이었다.

처음에 행사 소개하면서 유림님이 <b>"5명이 같은 팀이 된 이유가 있을 것이다!"</b>라고 말씀해주셔서 <b>"왜 우리는 팀이 되었는가?!"</b>에 대해 팀원들과 고민했다.

그러던 도중 어김없이 15초 자기소개 시간이 돌아왔고, 정신차려보니 마이크를 받았다.

간단하게 Python과 Django를 주로 사용하고 있고, 뱅갈고양이 하나와 같이 살고 있다고 말했다.

그랬더니 팀원분들 중 한 분이 "저도 고양이 2마리와 같이 살고 있어요"라고 말씀해주셨고, 다른 한 분은 제주도에서 오셨는데 그쪽에서 회사 직원들과 같이 길냥이들을 봐주고 있다고 해주셨다.

그리고 연달아서 한분은 고양이를 키우지는 않지만 엄청 좋아하신다고 하시고, 다른 한분도 고양이를 좋아하신다고 하셨다.

그래서 추측컨대 우리 팀은 고양이를 좋아하는 사람들이 모인 팀이 아닐까? 싶었다.

> 나중에 팀을 배치해준 운영진분께 여쭤보니, 고양이 + 동물을 좋아하는 분들과 팀이 된 것이 맞았다.

그렇게 15초 자기소개가 지나가고 키워드, 주제, 팀 이름, **팀 페르소나**를 정해야할 때가 왔다.

**페르소나**라는 단어가 생소해서 찾아보니, 아마도 "성격"을 나타내는 것 같았다.

그래서 팀 페르소나는 팀원 5명의 페르소나를 각각 1개씩 적용한 것이라고 하셨고, 그 페르소나를 가진 사람들이 우리가 만들게 될 서비스의 주 타겟층이 될 것이라고 해주셨다.

그래서 우리의 페르소나가 무엇이 있을지 각각 5가지씩 정리해서 공유했고, 그렇게 해서 나오게 된 우리 팀의 페르소나는 다음과 같았다.

##### 고양이와 여행을 좋아하는 워커홀릭

그래서 팀원들과 위의 사람을 위한 서비스가 무엇이 있을까 생각해봤다.

## 팀 페르소나에 맞는 주제와 서비스

그러던 도중 떠오른 것이, <a href="https://brunch.co.kr/@cholab/13" target="_blank"><b>뽀모도로 테크닉</b></a>이었고 아무리 워커홀릭이라지만 일하는 도중 쉬는 시간이 필요하다고 생각했다.

그래서 **"그 쉬는 시간동안 고양이 사진을 볼 수 있도록 하면 어떨까?!"**라는 생각이 떠올랐고, 팀원들 모두 동의해서 이 주제로 서비스를 만들기로 했다.

팀 이름은 **하품하는 고양이**로 정했다.

그러면서 자연스레 서비스 이름은 **HAGO**라고 정해졌고, 구현할 기능들을 정하기 시작했다.

일단 처음에는 **고양이 사진들을 크롤링하여 서버에 저장하고 뿌려주자!**라는 아이디어가 나왔는데, **진짜 고양이 사진인가?** 검증하는 부분이 이번 해커톤 내에 구현하기 어려울 것 같아 패스했다.

그러던 도중 좋은 API를 하나 찾았고, 이름하여 <a href="https://thecatapi.com/" target="_blank"><b>The Cat API</b></a>였다.

그래서 백엔드 기능이 얼마 안나올 것 같아서, 나 혼자 백엔드 개발을 맡았고, 시니어 백엔드 개발자분은 데스크탑용 앱을 만들기로 하셨다.

그리고 나머지분들은 각각 웹 프론트는 주니어 프론트엔드 개발자분이, 안드로이드 모바일은 시니어 프론트엔드 개발자분이 개발해주셔서 iOS 제외 모든 플랫폼을 커버했다.

나머지 한 분은 디자인과 iOS 겸하시려했지만 시간이 부족해 디자인만 하셨다.

그래서 이제 본격적으로 개발을 시작하기 위해 어떤 기능이 필요할지 논의했다.

시니어 백엔드 개발자분이 대충 만들어야할 API 목록을 정해주셨는데, 다음과 같다.

- 고양이 사진을 수집하는 배치 
- 고양이 사진을 저장하는 API 
- 고양이 사진을 조회하는 API
- 팁을 저장하는 API
- 팁을 조회하는 API

그래서 대충 저런 게 필요하겠구나 싶어 내가 백엔드 개발을 맡아야하니 Python/Django를 섞어 내 식대로 재정의해봤다.

그리고 중간에 모바일에 좋아요 버튼이 들어가면서, 좋아요 하는 기능도 생겼다.

혹시 몰라서 1가지 사진에 대한 정보를 주는 기능도 만들어둬야겠다고 생각했다.

그렇게 재정의한 백엔드 기능들은 다음과 같다.

- 고양이 사진을 수집하여 모델에 저장하는 Custom Management Command
- 랜덤하게 고양이 사진을 캐싱하는 Custom Management Command
- 랜덤하게 고양이 사진을 뿌려주는 API
- 1개의 고양이 사진에 대한 정보를 뿌려주는 API
- 고양이 사진에 대해 좋아요 처리하는 API
- 랜덤하게 팁을 뿌려주는 API
- \+ Django Admin

"팁"이란 건 고양이들 혹은 길고양이들에게 유용한 정보들이다.

그래서 팁을 API를 통해 사용하기 보다는 Django Admin에서 관리자가 직접 만들어두는 게 낫겠다고 생각해서 그 부분에는 Django Admin을 사용했다.

이런 식으로 기능을 정의하고 작업을 했다.

## 본격 개발 시작

하기 전에 점심으로는 피자도 먹었다.

맛있게 먹고, 개발하면서도 간식이 많아서 뭘 먹으면서 개발하는 나로써는 너무나 좋았다.

간식이 남아서 문제일 정도로 많았다.

그렇게 개발을 했고, 백엔드 작업은 간단해서 얼마 안걸렸다.

그렇게 개발을 하던 도중, 2018 자카르타-팔렘방 아시안게임 축구 결승전(한일전)이 있었고 치킨을 먹으며 축구를 봤다.

다같이 아쉬워하고, 기뻐하고 응원하는 게 너무 재밌었다.

그렇게 쭉 개발을 했고, Heroku에 배포했다.

HAGO의 백엔드 결과물은 <a href="https://github.com/planet-hackathon-3/hago_backend" target="_blank">GitHub</a>에서 확인할 수 있다.

## 이제 뭐하지?

해커톤 성격 자체가 그런진 몰라도 내가 할 게 없었다.

백엔드 개발을 하고 배포해놓고나니 너무나 할 게 없었다.

다른 클라이언트쪽도 같이 볼까 했는데, 데스크탑 앱은 electron, 웹은 Angular, 모바일은 Kotlin 이라 내가 아는 게 하나도 없었다.

괜히 방해만 될 듯 싶어 잿밥에 더 관심을 가지기로 했다.

Planet Hackathon은 경쟁이 없는 해커톤이지만 최고상은 있다.

그 상은 바로 가장 많이 다른 사람을 도와준 사람에게 수여되는 <b>"베스트 헬퍼상(King Helper)"</b>이었다.

베스트 헬퍼상은 개인전으로 2명을 뽑는데, 선정 기준은 구글 시트에 올라오는 다양한 질문들에 가장 많이 답변자로 채택되는 것이다.

초반에는 잿밥(상품)에 관심이 많지 않았는데, 개발을 다 하고 시간이 남다보니 잿밥이 탐났다.

그래서 엄청 간단한 질문부터 기술적인 질문까지 최대한 답변하려고 노력했다.

그러다가 다른 분들과 기술적인 얘기(<a href="https://github.com/denysdovhan/wtfjs" target="_blank">wtfjs</a>라는 게 있는데 wtfphp를 만들어볼까 라던가)를 한다던가 이것저것 하면서 시간을 때웠다.

## 마무리 하기 전에

새벽에는 요가 타임이 있었고, 아침 즈음에는 댄스 타임이 있었다.

요가 타임에는 가볍게 스트레칭을 해주었고, 댄스 타임에는 저스트댄스에 있는 Daddy Cool을 따라해보는 시간도 있었다.

색다른 시도가 많았고, 그게 눈에 보여 좋았다.

## 마무리

그렇게 마무리를 하고 시상식이 이뤄졌다.

우리 팀은 <b>"고양이와 즐거운 상상"</b>을 받았고, 인프런에서 후원해주신 쿠폰 및 후디를 받았다.

그리고 개인적으로는 <b>베스트 헬퍼상을 받았다</b>.

<img src="/assets/img/king-helper.jpg" alt="베스트 헬퍼상(King Helper)"/>

<img src="/assets/img/leopold-750r.jpg" alt="레오폴드 750R PD 갈축"/>

개인적으로 레오폴드라는 회사의 키보드를 좋아해서 닉네임으로 <b>"myungseokang"</b>를 사용하는데 이런 상품을 받게되어 너무 기쁘다.

## 느낀 점

#### 개인적인 측면으로는,

되게 오랜만에 하는 해커톤이었는데, 예전에는 무언가 만들 때 "이런 건 어떻게 만들어야하지?", "이 부분은 조금 어려울 것 같은데..." 싶은 부분이 있었다.

물론 이번에 개발하게 된 주제에 백엔드쪽이 기능이 많지 않아서일수도 있지만, 이번에는 그런 게 없었다.

Heroku에 배포하는 부분에서 삽질을 조금 했지만 무사히 끝났다.

시간이 남아서 <a href="http://flake8.pycqa.org/en/latest/" target="_blank">flake8</a>과 <a href="https://travis-ci.org/" target="_blank">Travis CI</a>까지 붙여봤다. 재밌었다.

다만 한가지 아쉬웠던 점은 시니어 백엔드 개발자분과 페어 프로그래밍을 하고 싶었는데, 우리 팀은 개발자 각각 한명이 다른 플랫폼을 맡아서 그걸 못 해봤다는 것이다.

다음엔 꼭 페어 프로그래밍을 해보고 싶다.

#### 행사적인 측면으로는,

일단 먹을 수 있는 것이 많았다.

다른 해커톤들에서는 부족한 경우도 있는데 일단 풍족했다.

더해서 신기했다.

개발하다가 요가도 하고, 춤도 추고, 같이 축구도 보고, 경쟁에 찌든 해커톤이 아니어서 너무 좋았다.

다음에도 한다면 또 참여하고 싶다. 너무 재밌었다 :D
