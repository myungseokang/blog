---
layout: post
title: "Mocking 알아보기 with Python"
categories: posts
excerpt: "Mocking with Python"
tags: [python, test, mocking]
author: leop0ld
comments: true
share: true
date: 2018-05-28
---

## Mocking 이 필요하다고 느낀 순간

테스트 코드를 작성하다보면 가끔은 실제로 실행할 수 없는 코드들을 마주하게 된다.

예를 들어, 외부 API를 호출하여 그에 대한 응답값을 사용한다던지 실제 파일을 다룬다던지 말이다.

이러한 외부 API를 테스트 코드에서 호출 못하는 것은 아니다.

하지만 테스트 코드에서 실제 외부 API를 호출하면 문제가 생긴다.

### 1. 외부 API 서비스가 정상적으로 작동하지 않을 때

이때는 테스트의 결과가 보장받지 못한다. 

무슨 소리냐하면 예를 들어, 내가 테스트 코드를 작성할 때에는 외부 API가 응답을 잘 해줬지만, CI에서 테스트할 때는 외부 API를 제공해주는 서비스가 뻗어버려서 테스트의 결과가 뒤죽박죽이 될 수도 있다.

### 2. 외부 API를 호출할 때 비용이 발생하는 경우

외부 API를 호출할 때 그 횟수에 따라 비용이 발생하는 경우가 있다. SMS API 혹은 Email API가 그렇다.



> 테스트 관련하여 참고하면 좋을만한 글: [유닛테스트에 대한 생각](https://blog.outsider.ne.kr/1275) - Outsider
