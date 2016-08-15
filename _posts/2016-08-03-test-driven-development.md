---
layout: post
title:  "Django TDD 알아보기"
date:   2016-08-03
excerpt: "Learning Test-Driven Development"
tag:
- Python
- Django
- Functional Test
- Unit Test
- Test-Driven Development
comments: true
---

## TDD 개요

### What is Test Code?!

Test Code란 코드를 테스트하는 코드를 의미합니다!
뭔가 말장난 같지만 사실입니다...
내가 짠 코드가 잘 돌아가는지 확인하기 위한 코드라고 말하면 이해가 좀 더 쉬울 수도 있겠습니다!


### TDD 기본 개념
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

- 이중 반복 TDD
 - 단위 테스트를 진행하고, 기능 테스트를 진행함으로서 두 번의 테스트를 거쳐 최소한의 코드로 개발하는 방법

### Selenium
 - Django에서 기능 테스트를 할 떄 사용하는 라이브러리
 - 실제 브라우저를 열어서 웹페이지의 기능을 테스트 해볼 수 있는 라이브러리
 - 기본으로 Firefox 브라우저를 지원하지만 다른 Webdriver를 설치하면 다른 브라우저에서도 테스트를 할 수 있다.

### Funtional Test
기능 테스트(Functional Test)
 - 기능 테스트란? 사용자의 관점에서 애플리케이션 외부를 테스트하는 것
 - 기능 테스트는 제대로 된 기능성을 갖춘 애플리케이션을 구축하도록 도와준다


### Unit Test
단위 테스트(Unit Test)
 - 단위 테스트란? 프로그래머 관점에서 그 내부를 테스트하는 것
 - 단위 테스트는 버그없이 깔끔한 코드가 되도록 도와준다
