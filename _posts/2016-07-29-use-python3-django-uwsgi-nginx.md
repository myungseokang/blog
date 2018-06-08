---
layout: post
title: "EC2에서 Nginx + uWSGI로 python3로 작성한 django project 배포하기"
categories: posts
excerpt: "Deploying django project made by python3 using Nginx + uWSGI"
tags: [django, nginx, uwsgi, aws, deploy]
author: leop0ld
comments: true
share: true
ads: true
date: 2016-07-29
---

# EC2에서 Nginx + uWSGI로 Python3로 작성한 django project 배포하기
> Python 3.x 버전 기준

매우 많이 참고한 곳

[유유자적 블로그](http://jeongyoungho80.blogspot.kr/2015/06/1404-django-uwsgi-nginx.html)

[nerogit.github.io](http://nerogit.github.io/2016/02/23/python3-django-nginx-uwsgi-deploy/)

--------------------------------------------------------------

OS     : Ubuntu 14.04

Python : 3.4.3(default python version에 virtualenv 만 사용)

Nginx  : 1.4.6

uWSGI  : 2.0.13.1

Django : 1.10.rc1

--------------------------------------------------------------


## 0. virtualenv, virtualenvwrapper 설치와 세팅

```shell
$ sudo apt-get update
$ sudo apt-get install python3-pip
$ sudo pip3 install virtualenv virtualenvwrapper
```

sudo apt-get 업데이트 한번 진행해주고
python3를 사용하기에 python3-pip를 설치해줍니다.
가상환경 관리를 위한 virtualenv+virtualenvwrapper도 사용해줍니다. + pyenv로 python 버전 관리를 할 생각입니다.

```shell
$ echo "export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3"
$ echo "export WORKON_HOME=~/.virtualenvs" >> ~/.bashrc
$ echo "source /usr/local/bin/virtualenvwrapper.sh" >> ~/.bashrc
$ source ~/.bashrc
```

그리고 virtualenvwrapper와 virtualenv 관한 설정을 bashrc에 넣어줍니다.


## 1. Django 설정

```shell
user@hostname:~$ mkvirtualenv ENV_NAME
(ENV_NAME)user@hostname:~$
(ENV_NAME)user@hostname:~$ pip install -r requirements.txt # or pip install --pre django
(ENV_NAME)user@hostname:~$ django-admin.py startproject PROJECT_NAME
(ENV_NAME)user@hostname:~$ cd ~/PROJECT_NAME
(ENV_NAME)user@hostname:~/PROJECT_NAME$ python manage.py migrate
(ENV_NAME)user@hostname:~/PROJECT_NAME$ python manage.py createsuperuser
(ENV_NAME)user@hostname:~/PROJECT_NAME$ python manage.py collectstatic
(ENV_NAME)user@hostname:~/PROJECT_NAME$ python manage runserver 0.0.0.0:8000
```

collectstatic 명령어 쓸 때, settings.py에 static_root는 설정되어 있다고 가정함
마지막으로 runserver로 확인함
잘 실행된다면 uwsgi 설정쪽으로 넘어감

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

## 2. uWSGI 설정

```shell
$ sudo apt-get install python-dev
$ sudo pip3 install uwsgi # sudo를 이용한 전체 시스템에 설치
$ uwsgi --http :8000 --home /home/user/.virtualenvs/ENV_NAME --chdir /home/user/PROJECT_NAME -w PROJECT_NAME.wsgi
```

이 때 AWS security group에 8000 번 포트가 열려있는지 확인해봐야 합니다.

uWSGI 를 독립된 애플리케이션들을 자동으로 관리해주는 "Emperor mode" 로 동작시킬 것입니다.

그리고 전체시스템에 적용되어야 하기 때문에 /etc/uwsgi/sites 폴더를 생성하고 설정파일을 저장합니다.

```shell
$ sudo mkdir -p /etc/uwsgi/sites
$ cd /etc/uwsgi/sites
$ sudo vim PROJECT_NAME.ini
```

내용은 아래에서 자신에 환경에 맞게끔 바꿔주면 됩니다.

```
[uwsgi]
project = PROJECT_NAME
base = /home/user

chdir = %(base)/%(project)
home = %(base)/.virtualenvs/ENV_NAME
module = %(project).wsgi:application

master = true
processes = 2

socket = /tmp/%(project).sock
chmod-socket = 664
vacuum = true
```

uWSGI 설정 파일은 [uwsgi]로 시작합니다.
그리고 각종 옵션들을 정해주는 방식의 파일입니다.

여기에서 쓰여진 옵션들을 간단하게 설명하자면
project = PROJECT_NAME을 담는 변수입니다.
%(VAR_NAME) = VAR_NAME에 변수 이름을 넣으면 그 값을 가져와서 사용할 수 있습니다.

chdir 에는 project root 경로를 알려주고, module 에는 wsgi:applicaion을 넘겨줍니다.
home 에는 만들어둔 가상환경의 경로를 알려줍니다.

그리고 uWSGI의 network 부분은 유유자적 블로그의 말을 인용하겠습니당

```
다음으로, uWSGI 가 어떻게 네트웤을 연결해야할지를 설정한다. 위 테스트에서는 HTTP 와 네트웤 포트를 사용했지만, Nginx 를 역프록시로 사용할 것이기 때문에 더 좋은 옵션이 있다.

모든 모듈들이 하나의 서버에서 동작하기 때문에 네트웤 포트를 사용하는 대신 더욱 안전하고 성능좋은 유닉스소켓을 사용할 수 있다. 유닉스소켓은 HTTP 를 사용하지 않고, 다른 서버와 통신하기 위해 디자인된  바이너리 프로토콜인 uWSGI의 uwsgi 프로토콜을 사용한다. Nginx 는 기본적으로 uwsgi 프로토콜을 사용할수 있기 때문에 유닉스소켓을 사용하는 것이 가장 좋은 선택이라고 볼수있다.

또한 웹서버가 쓰기권한을 가질수 있도록 소켓의 권한을 변경하여야 한다.
```

그리고 마지막으로 vacuum 부분은 서버가 멈췄을 때 자동으로 sock 파일을 삭제하는 옵션입니다.

master = true 부분은 마스터 프로세스를 세팅하겠다는 부분이고, processes = 2 부분은 프로세스를 2개로 설정하겠다는 것입니다.

그리고 부팅할 때에 자동으로 uWSGI가 실행되도록 하기위해 upstart 스크립트를 씁니다.

```
$ sudo vim /etc/init/uwsgi.conf
description "uWSGI application server in Emperor mode"

start on runlevel [2345]
stop on runlevel [!2345]

setuid user
setgid www-data

exec /usr/local/bin/uwsgi --emperor /etc/uwsgi/sites
```

start on, stop on -> uWSGI가 자동으로 동작할 때의 런레벨을 알려줍니다.
아래는 리눅스 런레벨에 대한 설명입니다.

```
# 0 - halt (DO NOT set initdefault to this)
시스템 종료

# 1 - Single user mode
시스템 복원모드, 기본적으로 관리자 셸

# 2 - Multiuser mode, without NFS (The same as 3, if you do ot have networking)
NFS(Network File System)을 지원하지 않는 다중 사용자 모드입니다.

# 3 - Full muliuser mode
일반적인 쉘 기반의 인터페이스를 가진 다중 사용자 모드입니다.

# 4 - unused
4번은 기본적으로는 쓸 수 없는 레벨입니다. 사용자가 직접 정의해서 사용할 수 있는 레벨입니다.

# 5 - X11
일반적인 그래픽 기반의 인터페이스를 가진 다중 사용자 모드입니다.

# 6 - reboot (DO NOT set initdefault to this)
시스템 재부팅
```

setuid, setgid -> 어떤 유저와 그룹으로 동작될 지를 설정해줍니다.
유저는 지금까지 작업한 유저로 하고, 그룹은 nginx를 아직 설치하지 않았지만 www-data 그룹으로 해줍니다.

세팅 명령어는 다 입력해주었고, 이제 uwsgi를 emperor 모드로 실행시키는 명령어만 마지막에 쓴 것처럼 넣어주면 됩니다.
emperor 옵션을 주면 디렉토리를 인자로 넘겨주게 되는데 이 디렉토리 안에 있는 앱들이 모두 작동하는 것 같습니다.


## 3. Nginx 설정

마지막으로 nginx를 설치하고 nginx 설정 파일만 만들어주면 됩니다.

```
$ sudo apt-get install nginx
$ sudo vim /etc/nginx/sites-avaliable/PROJECT_NAME
server {
    listen 80;
    server_name URL.com www.URL.com;

    location = /favicon.ico { access_log off; log_not_found off; }
    location /static/ {
        root /home/user/PROJECT_NAME;
    }

    location / {
        include         uwsgi_params;
        uwsgi_pass      unix:/tmp/PROJECT_NAME.sock;
    }
}
```

이렇게 sites-available에 생성해준 파일을 sites-enabled에 심볼릭 링크로 파일을 생성해줍니다.

```shell
sudo ln -s /etc/nginx/sites-available/PROJECT_NAME /etc/nginx/sites-enabled/
```

```shell
$ sudo service nginx configtest
```

nginx 설정이 제대로 되었는지 확인해줍니다.
그리고 default 디렉토리를 삭제해줘야 configtest가 제대로 나옵니다.

```shell
$ sudo service nginx restart
$ sudo service uwsgi restart
```

제대로 되었다면 위의 명령어로 설정파일이 잘 적용되게끔 재시작 해줍니다.

그리고 아까 설정해준 도메인으로 들어가게 되면 잘 나올겁니다!

이상 읽어주셔서 감사합니다 :)
