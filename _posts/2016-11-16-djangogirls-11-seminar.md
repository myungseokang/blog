---
layout: post
title: "Django Girls Seoul 11월 세미나 발표자를 맡게 되었습니다!"
categories: posts
excerpt: "I'm presenter at Django Girls Seoul Nov seminar"
tags: [djangogirls, django]
author: myungseokang
comments: true
share: true
ads: true
date: 2016-11-16
---

Django Girls 11월 세미나에서 발표자로 서게 되었습니다.

이렇게 서게 된 이유를 간단하게 얘기해보고 발표 주제에 대해서도 간단히 말해보고자 글을 적습니다.

## 발표 주제에 대한 이야기

발표 주제가 **settings.py에서 SECRET_KEY 분리해보기** 입니다.

이 주제를 선정한 이유는 제가 정답을 추구하고 추구하고 끈질기게 추구한 끝에 얻은 답이 너무나도 허무해서 이것에 대한 이야기를 말하려고 했으나, 너무 지루할 수 있을 것 같아서 튜토리얼 형식으로 진행하게 되었습니다.

간단하게 이야기를 해보자면 settings.py에 있는 SECRET_KEY는 django의 보안 기능에 있어서 아주아주 중요한 놈이라서 VCS에 올려서는 안됩니다. 

여기에 더해서 AWS 혹은 다른 중요한 값들이 위치하고 있는 settings.py 인데 이게 공개적으로 올라가게 되면 매우매우 안좋은 상황이 발생하기 때문에 이것을 분리해서 .gitignore에 추가해서 관리하고자 하는 방법을 알아볼 것입니다.

## 이러한 주제로 발표자로 서게 된 이유

이러한 주제로 발표자로 서게 된 이유는 저는 개발자 모임에 관심이 많고, 남을 도와주는 게 좋습니다.

남을 도와줄 능력이 된다면 최대한 알려주고 싶습니다. 더해서 개발 관련, 제가 관심있는 기술스택에 대해서 이야기를 나누는 것이 즐겁습니다.

모르는 걸 배우는 즐거움 또한 좋습니다.

이 모든 걸 사람들과 이야기를 함으로써 얻을 수 있습니다.

사람들과 얘기하며 django에 대한 이야기를 한다던가, 모르는 것을 이야기하면서 배운다던가, 혹은 제가 아는 것을 얘기하면서 알려드릴 수도 있습니다.

이런 것이 정말 즐겁습니다. 매우매우 좋습니다. 환영합니다.

그래서 이러한 자리에서 제가 좀 더 많은 분들에게 도움이 될 수 있게, 제가 삽질한 부분을 남들이 혹은 미래의 내가 삽질하지 않게끔 도와주고 싶어서 발표를 하게된 것 같습니다.

사실 좀 더 다듬어서 쓰려고 했으나... 말솜씨가 부족해서 ㅠㅠ 이정도까지만 써보겠습니다(책을 읽어야되나...)

이상입니다. 감사합니다 :)
