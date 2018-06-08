---
layout: post
title: "Python 개발환경 세팅하기"
categories: posts
excerpt: "Building a Python development environment"
tags: [pyenv, virtualenv, autoenv, python]
author: leop0ld
comments: true
share: true
ads: true
date: 2016-07-01
---

# Ubuntu Python 개발환경 세팅하기

## 0. Git과 필수 패키지 설치기

```shell
sudo apt-get update
```
```shell
sudo apt-get install -y make build-essential libssl-dev zlib1g-dev libbz2-dev \
libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev xz-utils
```
```shell
sudo apt-get install git
```
```shell
sudo apt-get install python3-pip
```
```shell
pip install virtualenv
```

## 1. pyenv 설치

pyenv는 git을 통해 받을 수 있습니다.

```shell
$ git clone https://github.com/yyuu/pyenv.git ~/.pyenv
```

그 다음에는 다음과 같은 명령어를 실행시켜줍니다.

```shell
$ echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bash_profile
$ echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bash_profile
```

사용자의 홈 디렉터리에 있는 .bash_profile이라는 파일에 pyenv의 path를 설정해주는 스크립트를 넣어주는 명령어입니다.

그리고 아래의 명령어도 실행해줍니다.

```shell
$ echo 'eval "$(pyenv init -)"' >> ~/.bash_profile
```

잘 설치되었는지 확인하는 쉬운 방법은 셸을 다시 실행시켜주는 것입니다.

```shell
$ pyenv --version
```

을 입력해주게 되면 pyenv의 버전이 나오게 됩니다.


## 2. pyenv-virtualenv 설치하기

그 다음으로는 pyenv 안에서 virtualenv를 사용하기 위해서 pyenv-virtualenv 라는 플러그인을 설치해야 합니다.

```shell
$ git clone https://github.com/yyuu/pyenv-virtualenv.git ~/.pyenv/plugins/pyenv-virtualenv
```

설치하고 난 뒤에 이것 또한 .bash_profile에 설정하는 코드를 한줄 추가해줘야 합니다.

```shell
$ echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.bash_profile
```

그 다음은 또다시 셸을 재시작해줍니다.

그 다음에 이제 pip를 이용해서 virtualenv를 설치해줍니다.

```shell
$ sudo pip install virtualenv
```

그럼 간단하게 잘 설치되었는지 테스트를 한번 해보도록 하겠습니다.

```shell
$ pyenv install 3.4.4
```

pyenv를 이용해서 파이썬 3.4.4 버전을 설치합니다.

잘 설치가 되었다면 pyenv shell을 이용해서 python 버전을 바꿔줍니다.

```shell
$ pyenv shell 3.4.4
```

를 입력해준 뒤 실제로 바뀌었는지 체크를 하기위해서 python -V 를 입력해보세요!

그 다음 python 3.4.4 버전으로 virtualenv를 하나 만드려면 다음과 같이 명령어를 작성해줍니다.

```shell
$ pyenv virtualenv venv
```

이 명령어로 만들어진 파이썬 버전들과 virtualenv들을 보려면

```shell
$ pyenv versions
```

를 입력하면 설치된 python의 버전뿐만 아니라 virtualenv들도 확인할 수 있습니다.

혹은 virtualenv들만 보고 싶다면

```shell
$ pyenv virtualenvs
```

로 확인이 가능합니다.

이렇게 만들어진 virtualenv 를 사용하려면 다음 명령어를 입력하시면 됩니다.

```shell
$ pyenv activate venv
```

매우 간단하게 할 수 있습니다.

그리고 이런 복잡한 과정들을 폴더를 옮기면 자동으로 virtualenv를 바꿔주는 것이 autoenv입니다.

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

## 3. autoenv 설치하기

autoenv는 폴더를 이동하면 자동으로 virtualenv가 설정되도록 해주는 것입니다.

```shell
$ touch project/.env
$ echo "echo 'woah'" > project/.env
$ cd project
woah
```

짜잔! 이런식으로 프로젝트 폴더 아래에 .env라는 파일을 만들고 폴더로 진입시에 그 .env 파일을 실행시켜주는 방식으로 이루어집니다.

그럼 한번 해보도록 하겠습니다!

먼저 autoenv를 설치해줘야겠죠?

```shell
$ git clone git://github.com/kennethreitz/autoenv.git ~/.autoenv
```

git을 이용해서 설치해줍니다.

다음에는 홈 디렉토리에 있는 .bash_profile 부분에 스크립트를 추가해주기 위한 명령어입니다.

```shell
$ echo 'source ~/.autoenv/activate.sh' >> ~/.bash_profile
```

추가가 잘 되었는지 vi 에디터로 확인을 한번하고 확실하게 셸을 껐다가 켜줍니다.

자 그럼 잘 설치가 되었는지 테스트를 해봐야겠습니다.

먼저 test라는 폴더를 만들고 그 아래에 .env라는 파일을 만들어서 test 디렉토리로 진입했을시에 .env 있는 스크립트가 실행되면 성공한 것입니다.

```shell
$ mkdir test && touch test/.env
```

test라는 디렉토리를 만들고 test 디렉토리 아래에 .env 라는 파일을 touch 명령어로 생성해주었습니다.

다음에 vi 에디터를 이용해서 test/.env 를 바꿔주시면 됩니다.

일단 위에 나와있는 예제처럼 echo 명령어가 확인하기 가장 쉬울듯합니다.

```shell
$ echo "echo 'L3opold7'" > test/.env
```

처럼 추가를 해주시게 되면 준비는 끝났습니다...!

이제 cd 명령어로 test 디렉토리에 들어가게 되면 echo 명령어로 지정한 L3opold7 라는 문자가 출력됩니다.

잘 나왔다면 성공입니다! 안나왔다면 홈 디렉토리에 .bashrc 파일을 잘 봐주시길 바랍니다~!

그렇다면 이제 극단적으로 폴더를 들어갔을때 파이썬 버전과 virtualenv가 바뀌어야 하는 상황입니다.

그렇다면 .env 파일에 어떤 스크립트를 넣어야하는지 알아보도록 하겠습니다.

먼저 pyenv로 파이썬 버전을 바꿔주고, 다음에 virtualenv를 설정해주면 됩니다.

먼저 vi 에디터로 .env 파일을 열어줍니다.

그 다음에 안에 내용을 아래와 같이 바꿔줍니다.

```shell
pyenv activate venv
```

참고로 여기서 지정한 venv 라는 virtualenv는 미리 만들어져 있어야합니다!

위와 같이 .env 파일을 설정해주게 되면 디렉토리 진입시 .env 파일이 실행되어 venv 라는 가상환경이 activate 되는 것입니다.

이상 읽어주셔서 감사합니다 :)
