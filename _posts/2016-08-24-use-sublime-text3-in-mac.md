---
layout: post
title: "macOS 에서 Sublime Text3 사용하기"
categories: posts
excerpt: "Using Sublime Text3 in macOS"
tags: [macos, sublimetext3]
author: myungseokang
comments: true
share: true
ads: true
date: 2016-08-24
---

# Mac에서 Sublime Text3 설치하고 사용하기

> 참고 링크: [Link](http://jos39.tistory.com/243)(여러가지 좋은 플러그인들을 소개해줍니다.)

윈도우에 Atom이 있다면 Mac에는 Sublime Text 가 있습니다!

## Sublime Text3 설치하기

서브라임 텍스트를 설치합니다. [Download Link](https://www.sublimetext.com)

저는 가난한 신입 개발자라서 구입은 하지 않았습니다 ㅠㅠ

설치가 끝났다면 간단한 설정을 하면 셸에서 사용할 수 있습니다.


## Sublime Text3 설정하기

서브라임을 터미널에서

```shell
$ sublime .
```

이런 식으로 사용하기 위해서는 약간의 설정이 필요합니다.

그리고 아래의 명령어를 입력합니다.

```shell
$ ln -s "/Applications/Sublime Text.app/Contents/SharedSupport/bin/subl" /usr/local/bin/sublime
```

usr/local/bin/sublime 에 심볼릭 링크를 걸어줍니다.


## Sublime Text3 사용하기

이제 

```shell
$ sublime 
```

을 입력하게 되면 sumblime이 정상적으로 열립니다!

이제 plugin을 설치하고 사용하는 것을 해보도록 하겠습니다.

1. <kbd>Ctrl</kbd>+<kbd>`</kbd> 를 눌러서 Text Console을 열어줍니다.

2. 그 다음 아래의 코드를 입력해줍니다!!

```python
import urllib.request,os,hashlib; h = 'eb2297e1a458f27d836c04bb0cbaf282' + 'd0e7a3098092775ccb37ca9d6b2e4b7d'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)
```

입력하게 되면 Package Controller가 설치됩니다.

이제 Package Controller를 이용해서 여러가지 플러그인들을 설치해보도록 하겠습니다.

1. Sublime Text Command Pallet를 엽니다. (단축키: Command-Shift-P, 메뉴: Tools > Command Pallet)
2. install 을 입력하면 Install Package 가 나오는데 클릭해줍니다.

이제 원하는 패키지명을 입력하고 설치하면 됩니다!