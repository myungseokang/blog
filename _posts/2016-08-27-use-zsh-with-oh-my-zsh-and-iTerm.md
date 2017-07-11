---
layout: post
title: "mac OS 에서 oh my zsh와 iTerm2 사용하기"
categories: posts
excerpt: "Using oh my zsh and iTerm2 in mac OS"
tags: [mac-os]
author: leop0ld
comments: true
share: true
date: 2016-08-27
---

# Mac에서 oh my zsh와 iTerm2 사용하기

먼저 iTerm2 을 설치해줍니다.

[iTerm2 Download](https://www.iterm2.com/)

잘 받았다면 이제 zsh 설정을 해봅시다.

zsh는 bash 셸의 진화 형태라고 생각하면 편할 것 같습니다.

zsh의 강력한 특징 중에 하나는 Tap-completion 입니다.

이러한 zsh에다가 oh my zsh를 다운받고 Git 관련 테마를 입힐 경우 정말정말 사용이 편한 환경을 구축할 수 있습니다.


## zsh 설치하기

```shell
$ zsh --version
```
명령어로 zsh가 설치되어 있는지부터 확인해봅니다.

잘 설치되어 있다면 다음 단계로 넘어가겠습니다.

설치가 안되어있다면 다음과 같은 명령어를 입력해서 설치해줍니다.

```shell
$ brew update
$ brew install zsh
```
이렇게 설치를 해주고 zsh를 기본 셸로 지정해주기 위해서 아래의 명령어를 실행해줍니다.

```shell
$ chsh -s `which zsh`
```
이렇게 기본 셸을 bash가 아닌 zsh로 지정해줍니다.


## oh my zsh 설치하기

```shell
$ sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```
위의 명령어로 간단하게 설치할 수 있습니다. 단, git이 설치되어 있다는 전제하에 가능합니다.

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

## agnoster & solarized theme 적용하고 사용하기

iTerm을 solarized dark 테마로 적용시켜 봅시다.

![solarized dark theme](/assets/img/solarized_dark_설정.png)

Preference(단축키: <kbd>Command</kbd>+<kbd>,</kbd>) 로 가서 위의 사진과 같이 Profile 탭의 Color preset을 바꿔줍니다.

이제 oh my zsh의 테마를 바꿔봅시다.

하지만 그 전에!! 먼저 폰트가 깨지지 않나 체크부터 해봐야합니다.

```shell
$ echo "\ue0b0 \u00b1 \ue0a0 \u27a6 \u2718 \u26a1 \u2699"
```
를 했을 경우 font가 깨지지 않고 다음과 같이 나와야합니다.

![font test](/assets/img/font_test.png)

만약 font가 깨진다면 iTerm의 Non-ASCII Font 부분을 Powerline font로 바꿔줍니다.

[Font Download Link](https://github.com/powerline/fonts)

설정을 다 했다면 이제 테마를 바꿔봅시다!

```shell
$ vim ~/.zshrc
```
위의 명령어를 통해서 홈 디렉토리에 있는 zsh 설정파일로 들어가줍니다.

그 다음 기존의 ZSH_THEME 부분에 # 으로 주석 처리를 한 뒤

```shell
ZSH_THEME = agnoster
```
를 추가해줍니다.

이런 식으로 말입니다.

![agnoster theme](/assets/img/agnoster_theme_설정.png)

이제 iTerm을 껐다가 다시 켜면...!!

![최종](/assets/img/final.png)

간단한 듯 간단하지 않은 zsh 설치, 설정 및 사용하기! 였습니다.
