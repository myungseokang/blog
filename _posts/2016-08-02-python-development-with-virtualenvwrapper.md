---
layout: post
title:  "Ubuntu에서 virtualenv + virtualenvwrapper 사용해서 python 개발환경 세팅하기"
date:   2016-08-02
excerpt: "Using Virtualenv + Virtualenvwrapper at Ubuntu"
tag:
- Python
- Ubuntu
- Virtualenv
- virtualenvwrapper
comments: true
---

> python 3.x 기준

## 0. 필수 패키지 설치
> pass

## 1. virtualenv + virtualenvwrapper 설치

```shell
$ sudo apt-get update
$ sudo apt-get install python3-pip
```

python3 를 사용할 경우 python3-pip 패키지를 받고 pip 대신 pip3 명령어를 사용합니다.

```shell
$ sudo pip3 install virtualenv virtualenvwrapper
```

pip3를 이용해서 두 개의 모듈을 설치했다면 이제 셸에서 virtualenvwrapper를 사용할 수 있도록 해주어야 합니다.

```shell
$ echo "export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3"
$ echo "export WORKON_HOME=~/.virtualenvs" >> ~/.bashrc
$ echo "source /usr/local/bin/virtualenvwrapper.sh" >> ~/.bashrc
```

이렇게 설정해주고

```shell
$ source ~/.bashrc
```

이렇게 셸을 재시작 해줍니다.

virtualenvwrapper의 사용법을 간단하게 알아보겠습니다.

```shell
$ mkvirtualenv ENV_NAME
```

이렇게 virtualenv를 생성하고

```shell
$ workon ENV_NAME
```

이렇게 workon을 이용해서 activate 시킬 수 있습니다.

```shell
$ lsvirtualenv
```

lsvirtualenv 명령어를 통해서 현재 어떤어떤 virtualenv들이 있는지도 확인할 수 있습니다.

```shell
$ deactivate
```

deactivate를 통해 가상환경에서 빠져나올 수도 있습니다.

```shell
$ rmvirtualenv ENV_NAME
```

rmvirtualenv 명령어를 통해서 virtualenv를 삭제할 수 있습니다.

이상 읽어주셔서 감사합니다 :)
