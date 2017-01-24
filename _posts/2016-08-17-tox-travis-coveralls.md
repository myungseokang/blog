---
layout: post
title: "Github, Tox, Travis CI and Coveralls"
date: 2016-08-17
categories: Python
author: leop0ld
---

## Github
소스코드 저장소.
하지만 단순히 저장소 역할만은 하지 않습니다.

## Tox
Python, Django를 버전별로 묶어서 envlist로 간편하게 관리할 수 있는 도구입니다.

## Travis CI
Test를 자동으로 빌드해주고, 커밋 단위로 Test가 성공했는지 실패했는지도 나타내주는 도구입니다.
내가 한 커밋은 물론이고, Pull Request에 대해서도 success 와 fail 여부를 알려줍니다.
그래서 pull request를 merge 할지말지도 쉽게 정할 수 있습니다.
build passing 뱃지를 달 수 있습니다...!

## Coveralls
코드가 얼마나 간결하고 깔끔한지를 나타내주는 도구입니다.
커밋 단위, 파일 단위로 coverage를 알 수 있습니다.
이 도구 역시 pull request에 대해 merge했을 경우 coverage가 떨어지는지도 알 수 있어서 굉장히 좋은 도구입니다.
coverage ?% 라는 뱃지를 달 수 있습니다...!
