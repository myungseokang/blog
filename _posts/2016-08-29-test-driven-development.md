---
layout: post
title: "TDD(Test-Driven Development) 알아보기"
categories: posts
excerpt: "Studying TDD(Test-Driven Development)"
tags: [tdd, test, study]
author: myungseokang
comments: true
share: true
ads: true
date: 2016-08-29
---

# TDD 알아보기

## What is Test Code?!

Test Code란 코드를 테스트하는 코드를 의미합니다!
뭔가 말장난 같지만 사실입니다...
내가 짠 코드가 잘 돌아가는지 확인하기 위한 코드라고 말하면 이해가 좀 더 쉬울 수도 있겠습니다!


## TDD 기본 개념
TDD란?
![Test-Driven Development](https://upload.wikimedia.org/wikipedia/commons/thumb/0/0b/TDD_Global_Lifecycle.png/800px-TDD_Global_Lifecycle.png)
- TDD란 Test-Driven Development의 약자로써, 해석 그대로 테스트가 주도하는 개발입니다. 먼저 테스트 코드를 짜고 그 다음에 그 테스트 코드를 통과할 수 있는 minimal한 코드를 작성하는 것이 TDD의 목표입니다.

- 사용자 스토리(User Story) -> 사용자 관점에서 어떻게 동작해야하는지를 기술한 것, 기능 테스트의 구조화를 위해 사용된다
- 예측된 실패(Excepted failure) -> 의도적으로 구현한 테스트 실패를 의미한다
- 퇴행(Regression) -> 동작하고 있던 애플리케이션 처리가 새롭게 구현된 코드에 의해 망가지는 것
- 예상하지 못한 실패(Unexpected failure) -> 생각지 못하게 테스트에서 실패가 발생하는 것
- 레드(Red), 그린(Green), 리팩터(Refactor) -> TDD 처리를 기술한 다른 방법, 테스트를 작성해서 실패하는지 보고(레드), 코드를 수정해서 테스트를 통과하게끔 만든다(그린). 그 다음 리팩터로 코드를 개선한다.
- 삼각법(Triangulation) -> 기존 코드에 구체적인 테스트 케이스를 추가해서 일반화한 처리를 정당화하는 것
- 스트라이크 세 개면 리팩터(Three strikes and refactor) -> 언제 중복 코드를 제거해야하는지 알려주는 일반적인 규칙, 두 개의 코드가 비슷할 경우 세번째 코드가 나올때 까지 기다려보고 동일 코드가 나온다면 리팩터링을 한다.

이중 반복 TDD
- 단위 테스트를 진행하고, 기능 테스트를 진행함으로서 두 번의 테스트를 거쳐 최소한의 코드로 개발하는 방법

## Selenium

- Browser Emulator
- 코드를 이용해서 실제 브라우저를 열어 웹페이지의 기능을 테스트 해볼 수 있습니다.

간단하게 

```shell
$ pip install selenium
```

로 설치가 가능합니다.

기본으로 Firefox 브라우저를 지원하지만 다른 브라우저들의 Webdriver를 설치하면 다른 브라우저에서도 테스트를 할 수 있습니다.
셀레늄에서는 Waiting이 중요합니다.(the beauty of waiting)
- 주로 Page Loading, Ajax response, Elements Loading, Elements visibillity etc. 을 기다립니다.

PYCON APAC 2016!

[클라우드 상에서 Selenium을 이용한 Django 기능 테스트 자동화!](http://www.slideshare.net/JonghyunPark/automating-django-functional-tests-using-selenium-on-cloud?utm_source=slideshow&utm_medium=ssemail&utm_campaign=post_upload_view_cta)

Selenium을 이용한 기능 테스트를 작성할 때 가장 중요한 점 중 하나는 DOM 객체가 준비(ready)될 때까지 기다리는 것입니다.
기능 테스트는 단위 테스트와는 다르게 실제 웹브라우저 상에서 이루어지므로 브라우저에서 DOM이 작동하는 과정에 대한 고려가 필요합니다.
CPU가 빨리 동작하는 환경이라면 다행히 문제가 없을 수도 있겠지만, 그렇지 않고 객체가 아직 준비되지 않았다면 'undefined'가 반환되면서 적절한 테스트가 불가능하게 될 수 있습니다.
이런 상황을 피하고자 셀레늄에서는 두 가지 기다림, implicit wait(암시적 기다림)과 explicit wait(명시적 기다림) 을 지원합니다.
먼저 implicit wait을 알아보겠습니다만 implicit wait보다는 explicit wait을 사용하는 것이 더 좋습니다.
공식 문서에는 이렇게 나와있습니다.
An implicit wait is to tell WebDriver to pull the DOM for a certain in amount of time when trying to find an element or elements if they are not immediately available.
암시적 대기는 즉시 사용할 수없는 경우 요소를 발견할 때 시간의 양에 특정의 DOM을 끌어 WebDriver 말해주는 것입니다.(feat. Translate Google)
구글 번역에서는 위와 같이 말해줍니다.
즉시 사용할 수 없는 경우, 즉 아직 로딩이 안되었거나 DOM에 없는 요소를 WebDriver에서 쓸 때 사용할 수 있는 것입니다.
그리고 명시적 기다림은 공식 문서에 이렇게 나와있습니다.
An explicit wait is code you define to wait for a certain condition to occur before proceeding further in the code.
명시적 대기는 특정 조건이 코드에서 더 진행하기 전에 발생할 때까지 기다릴 정의 코드입니다.(feat. Translate Google)
implicit wait 같은 경우에는 그 시간내에 로딩이 되지 않을 경우 테스트의 결과가 의도한대로 나오지 않을 수 있기때문에 explicit wait을 쓰는 것이 편합니다.
Never Use Stupid Wait!

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

**Running selenium on AWS**

No Screen
- Virtual Display로 해결!

Link 참고: [Selenium on AWS EC2](http://goo.gl/GRbztO)

Tips.
- Faster Password hasher: Password를 암호화할 때 PBKDF2 알고리즘이 아닌 MD5 해시를 사용(절대 배포환경에서는 사용해서는 안됨!)
- session을 이용해서 Login 부분 테스트를 간략하게 할 수 있음


## Funtional Test
기능 테스트(Functional Test)
- 기능 테스트란? 사용자의 관점에서 애플리케이션 외부를 테스트하는 것
- 기능 테스트는 제대로 된 기능성을 갖춘 애플리케이션을 구축하도록 도와줍니다


## Unit Test
단위 테스트(Unit Test)
- 단위 테스트란? 프로그래머 관점에서 그 내부를 테스트하는 것
- 단위 테스트는 버그없이 깔끔한 코드가 되도록 도와줍니다

## Smoke Test
스모크 테스트(Smoke Test)
- 스모크 테스트란? CSS 같은 정적 파일이 잘 로딩되었는지 테스트하는 것
