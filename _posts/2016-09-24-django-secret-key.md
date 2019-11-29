---
layout: post
title: "Django에서 SECRET_KEY 관리하기"
categories: posts
excerpt: "Manage SECRET_KEY in Django"
tags: [python, django]
author: myungseokang
comments: true
share: true
ads: true
date: 2016-09-24
---

Django는 보안이 잘 되어있는 편입니다.

김승진 역자님은 보안에 신경을 덜 쓰기 위해 프레임워크를 사용한다고 말씀하셨습니다.

그만큼 보안이 잘 되어있지만 **SECRET_KEY** 라는 것을 settings.py에서 한번쯤은 보셨을 겁니다.

이것이 잘 막아주고 있는 편입니다.

하지만 Github이나 Bitbucket 같은 원격 저장소에 업로드할 때 이 SECRET_KEY를 올리게 되면 보안에 **매우매우매우매우** 취약해집니다.

물론 private repo면 상관이 없으나 public으로 업로드를 한다면...? + AWS를 사용하면서 ACCESS_KEY와 SECRET_ACCESS_KEY를 같이 올렸다면...?

개인 프로젝트 같은 경우 운영 서버에 올려서 돌리는 경우가 적기 때문에 피해가 덜 하지만 실제 운영중인 서비스가 이런 실수를 하게되면 정말 치명적입니다.

그만큼 중요한 이 SECRET_KEY를 그럼 어떻게 관리하느냐가 요즘 저의 고민거리입니다.

1. 운영 서버에서 웹서버를 Apache 를 사용한다 -> 설정 파일 패턴 사용
2. 운영 서버에서 웹서버를 Nginx  를 사용한다 -> 환경 변수 패턴 사용

으로 결정을 내렸습니다.

그리고 또 한 가지의 옵션이 있습니다.

1. 원격 저장소(Ex. Github)   가 Private repo 다 -> 설정 파일 그냥 업로드(gitignore ㄴㄴ)
2. 원격 저장소(Ex. Bitbucket)가 Public  repo 다 -> 설정 파일 gitignore에 추가한 뒤 서버에서 따로 생성

이라는 결론을 내렸습니다.

그리고 Nginx의 경우에는 배포 구동 스크립트에서 `export` 를 통해 추가를 해주면 됩니다.

서론은 **여기까지만** 하고 이제 직접 해보겠습니다.
먼저 설정 파일로부터 값을 가져오는 방법을 소개해보도록 하겠습니다.

## 설정 파일 패턴

저는 가독성도 좋고 파일을 읽어들여서 편집하기도 쉬운 json 포맷을 사용할 것이지만 yml 파일이나 config 파일을 사용하셔도 무방합니다.
저는 아래와 같이 secret.json 파일을 설정해봤습니다.

secret.json

```json
{
  "AWS_SECRET_ACCESS_KEY": "aws-secret-access-key",
  "AWS_ACCESS_KEY_ID": "aws-access-key-id",
  "SECRET_KEY": "my-secret-key"
}
```

이렇게 필요한 것들을 적어준 뒤 setting 파일에서 다음과 같이 함수를 만들어보았습니다.

settings.py의 일부

```python
with open('secret.json', 'r') as f:
    secret = json.loads(f.read())

def get_secret(setting, secret=secret):
    try:
        return secret[setting]
    except:
        msg = "Set key '{0}' in secret.json".format(setting)
        raise ImproperlyConfigured(error_msg)

# USAGE

SECRET_KEY = get_secret('SECRET_KEY') # my-secret-key
```

요렇게 작성을 해줬더니 잘 가져왔다...!

이렇게 분리해주는 것이 **설정 파일 패턴** 이고, 환경 변수 패턴은 환경 변수에 대한 지식이 어느정도 필요합니다.
막 어려운 부분은 아니니(그리고 대부분의 개발자 분들이라면 알고 계실듯) 쉽게 하실 수 있습니다.

## 환경 변수 패턴

linux or OS X 기준으로 설명하겠습니다(윈도우 사용자분들에게는 미리 사과의 말씀을...)

Linux or OS X(or macOS) 를 사용하신다면 .bashrc, .bash_profile 혹은 .zshrc 같은 셸이 구동될 때 참고할 사항들을 미리 적어놓는 스크립트 파일입니다.
따라서 이 곳에 필요한 값들을 넣어놓게 되면 완전히 안전한 것은 아니지만 그래도 안전하게 사용할 수 있습니다.

저는 OS X에서 zsh를 사용하기 때문에 .zshrc에 이렇게 추가해주었습니다.

.zshrc

```shell
export AWS_SECRET_ACCESS_KEY="aws-secret-access-key"
export AWS_ACCESS_KEY_ID="aws-access-key-id"
export SECRET_KEY="my-secret-key"
```

이렇게 해준 뒤 setting 파일에서 다음과 같이 해주면 간단하게 추가가 가능합니다.

settings.py의 일부

```python
import os

SECRET_KEY = os.environ['SECRET_KEY'] # my-secret-key
```

요로코롬 사용해주시면 됩니다.

그리고 이 부분을 virtualenv에 bin/activate 스크립트에 넣어줄 수도 있고, autoenv를 사용해서 설정해줄 수도 있지만 별로 추천드리고 싶지 않습니다.
autoenv 부분은 잠깐 짚고 넘어가자면

.env

```shell
pyenv activate VENV_NAME
export SECRET_KEY="my-secret-key"
```

이런 식으로 .env 가 있는 폴더에 진입했을 때 자동으로 셸 스크립트를 실행시켜주는 것입니다.
잘 사용하면 엄청 편하기 때문에 이 부분에 이렇게 넣고 사용하셔도 될 것 같습니다.

혹은 Pycharm을 사용하실 경우 Edit Run configurations 를 들어가보시면 환경 변수를 추가해줄 수 있는 부분이 있습니다.
그 부분에 추가해주면 Pycharm Run 시킬 경우 사용이 가능합니다.(Pycharm에서는 위의 두 방법다 통하지 않고 오직 이 방법만이 통합니다. Pycharm Run을 위해서라면 이렇게 설정해주세요!)


#### 2016.09.27 추가

Ubuntu의 Apache 환경으로 배포할 때 secret.json을 못찾아서 500 에러가 발생하는 경우가 있었습니다.
이 경우에는 아래와 같이 작성해주었습니다.

settings/base.py 의 일부

```python
BASE_DIR = os.path.dirname(os.path.dirname(os.path.dirname(os.path.abspath(__file__))))

secret_file = os.path.join(BASE_DIR, 'secret.json')

with open(secret_file, 'r') as f:
    secret = json.loads(f.read())


def get_secret(setting, secret=secret):
    """
    :param setting: secret Dict 의 원하는 value 값을 가져올 수 있게하는 key 값
    :param secret: 비밀 변수들의 실제 값을 담은 json 파일을 Dict화 한 변수
    :return secret[setting]: secret.json 에서 가져온 Dict 의 setting 키를 가진 값을 리턴해줍니다.
    """
    try:
        return secret[setting]
    except KeyError:
        error_msg = "Set the {0} environment variable".format(setting)
        raise ImproperlyConfigured(error_msg)
```

이렇게 해주니 Ubuntu(AWS 서버)와 OS X(로컬) 모두 secret.json을 잘 가져왔습니다.
이렇게 사용해주는 것이 좋을 듯 합니다.

이상입니다.
읽어주셔서 감사합니다 :D

