---
layout: post
title:  "Buy Macbook Pro Retina!"
date:   2016-08-22
excerpt: "Macbook Pro Retina"
tag:
- Macbook Pro Retina
- Macbook
- Apple
comments: true
---

## Mac에서 Homebrew 사용하기 + Sublime Text 사용하기vim

```shell
$ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
위의 명령어를 입력해주면 됩니다.

너무 간단합니다.

그리고 터미널에서 brew update 만 해주면 잘 설치되었는지 확인할 수 있습니다.

그리고 서브라임을 터미널에서

```shell
$ sublime .  # open current directory using Submlime Text
```

이런 식으로 사용할 수 있게할 수 있습니다.

먼저 서브라임 텍스트를 설치합니다. [Download Link](https://www.sublimetext.com)

그리고 아래의 명령어를 입력합니다.

```shell
$ ln -s "/Applications/Sublime Text.app/Contents/SharedSupport/bin/subl" /usr/local/bin/sublime
```

usr/local/bin/sublime 에 심볼릭 링크를 걸어줍니다.

이제 
```shell
$ sublime 
```
을 입력하게 되면 sumblime이 정상적으로 열립니다!
