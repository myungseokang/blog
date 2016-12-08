---
layout: post
title:  "pyenv, virtualenv, autoenv 를 사용하여 Python 개발환경 구축하기"
date:   2016-12-08
excerpt: "Building a Python development environment using pyenv, virtualenv, and autoenv"
tag:
- Python
- pyenv
- virtualenv
- autoenv
comments: true
---

## 설치에 앞서

각각이 어떤 역할을 하는지 간략하게 설명하고 넘어가겠습니다.

- pyenv : "*Python Version Management*", 로컬에 다양한 파이썬 버전을 설치하고 사용할 수 있도록 해줍니다.

- virtualenv : “*Virtual Python Environment builder*”, 로컬에 다양한 파이썬 환경을 구축하고 사용할 수 있도록 해줍니다.

- autoenv : 특정 프로젝트 폴더로 들어가면 자동으로 지정해준 쉘 스크립트를 실행해줍니다.

## 본격적으로 설치!

```shell
$ brew update
$ brew install pyenv pyenv-virtualenv autoenv
```

(귀찮으니 한꺼번에...)위의 명령어로 설치할 수 있습니다.


### pyenv 설정하고 사용하기

```shell
$ echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.zshrc
```

이제 쉘을 껐다가 켠 후 확인해보면 됩니다.

```shell
$ pyenv versions
* system (set by /usr/local/opt/pyenv/version)
```

처음에는 이렇게 system이라고만 나올텐데 시스템에 기본적으로 설치되어 있는 파이썬 버전을 사용하고 있다는 뜻입니다.

pyenv 여러 파이썬 버전을 이용할 수 있게 도와주는 도구 이므로, 다른 파이썬 버전을 설치할 수도 있습니다.

```shell
$ pyenv install 3.5.2
$ pyenv versions
* system (set by /usr/local/opt/pyenv/version)
  3.5.2
```

이런 식으로 적어주면 간편하게 여러 파이썬 버전을 설치할 수 있습니다.

```shell
$ pyenv install --list
```

위의 명령어를 입력해주면 설치할 수 있는 파이썬 리스트가 나옵니다. 참고하시면 매우 유용하게 사용할 수 있을 것 같습니다.

하지만 아직 `python` 명령어를 입력했을 경우 system에 설치된 python의 인터프리터가 실행됩니다.

이럴 경우 다음 명령어로 쉘의 python 버전을 바꿔줄 수 있습니다.

```shell
$ pyenv shell 3.5.2
```

이 명령어는 해당 셸, 즉 켜져있는 그 셸에서만 python의 버전을 3.5.2로 바꾸겠다는 것입니다.

그리고 내가 셸에서 나갔다가 다시 들어와도 python 버전이 3.5.2로 유지되게 하게 할 수 있습니다.

```shell
$ pyenv global 3.5.2
```

이런 식으로 `global` 명령어를 통해서 해결합니다.

물론 이러한 버전들을 삭제할 수도 있습니다.

```shell
$ pyenv uninstall {PYTHON_VERSION}
```

더 자세한 설명은 `pyenv --help` 를 입력하시면 확인하실 수 있습니다!


### virtualenv 설정하고 사용하기

pyenv 와 동시에 pyenv-virtualenv 도 설치를 했기 때문에 이것 역시 사용할 수 있습니다.

위에서 `pyenv versions` 라는 명령어를 다뤘었는데, pyenv-virtualenv 가 설치되어 있을 경우 가상 개발 환경 리스트도 모두 보여줍니다.

```shell
$ pyenv versions
  system
* 3.5.2 (set by /usr/local/opt/pyenv/version)
  3.5.2/envs/opengallery
  opengallery
```

현재 제가 `opengallery` 라는 이름을 가진 virtualenv를 생성했습니다. 그리고 현재 사용하는 python은 3.5.2 기본입니다.

이제 방금 만든 `opengallery` 라는 이름을 가진 virtualenv 로 넘어가서 여러 가지 python 패키지들을 설치해봅시다.

```shell
$ pyenv activate {VIRTUALENV_NAME}
```

이렇게 간단한 명령어로 virtualenv를 활성화 시킬 수 있습니다.

이렇게 되면 셸의 제일 앞에 `(VIRTUALENV_NAME)` 과 같이 활성화 되었다는 의미로 virtualenv의 이름이 나오게 됩니다. 성공입니다!

```shell
$ pip install django
```

이렇게 쉽게 설치할 수 있습니다.

이렇게 활성화된 가상 환경도 역시 가상 환경을 해제해줄 수도 있습니다.

```shell
$ pyenv deactivate
```

로 간단하게 해제 가능합니다.

사실 이제 모두 사용가능합니다.

하지만 조금 더 편하게 하기 위해서 저는 autoenv를 사용 중이긴 합니다.

이게 되게 작은 차이일 수 있지만 엄청 편합니다.

그렇다면 이제 autoenv로 넘어 가보겠습니다.

### autoenv 설정하고 사용하기

```shell
$ echo 'source /usr/local/opt/autoenv/activate.sh' >> ~/.zshrc
```

일단 다음 명령어를 입력해줍니다.

처음에 설치할 때 autoenv까지 같이 설치해줬기 때문에 설정만 잘 해준다면 바로 사용이 가능합니다.

사용법도 아주 간단합니다.

특정 폴더(프로젝트 최상위 폴더가 좋을 듯 합니다)에 `.env` 라는 이름을 가진 파일을 만들고 거기에 내가 그 특정 폴더에 들어갔을 시에 설정해줄 셸 스크립트를 입력해주면 됩니다.

예를 들어보겠습니다.

제가 `opengallery` 라는 Django 프로젝트 폴더에 진입했을 시에 `pyenv activate opengallery` 라고 입력해줬으면 좋겠다 라고 생각하면 opengallery 라는 폴더 안에 `.env` 라는 파일을 만들고 내용을 `pyenv activate opengallery` 라고 적어주면 됩니다.

이렇게 해주면 pyenv + pyenv-virtualenv + autoenv 모두 사용이 가능합니다!

감사합니다! :D