---
layout: post
title: "Django 나만의 Command 만들어보기"
date: 2017-03-23
categories: Django
author: leop0ld
---

Django 에는 `Command` 가 있다.

예를 들면 `python manage.py <command>` 혹은 `django-admin <command>` 로 사용한 것이 바로 `Command` 이다.

보통 사용하는 커맨드(Django 내장 Command)로는 아래와 같은 것들이 있다.

```
$ django-admin startapp
$ django-admin startproject
$ python manage.py migrate
$ python manage.py runserver
$ python manage.py shell
```

등등 더 많은 내장 Command 가 있다.

자세한 건 [공식문서](https://docs.djangoproject.com/en/1.10/ref/django-admin/#available-commands) 를 찾아가서 보면 된다.

오늘 이야기하고 싶은 것은 이러한 내장 Command 가 아닌 내가 원하는 로직을 수행할 수 있는 **나만의 Custom Command** 를 만드는 것이다.

이러한 Custom Command 를 사실 어디다가 써야할 지 궁금해할 수도 있다.

(잘못된 방법일 수 있으나) 현재 회사에서는 Cron Task 를 이 Custom Command 를 만들어서 처리하고 있다.

이 말인 즉슨, Custom Command 를 만들어두고, Crontab 을 이용해서 일정 주기마다 그 명령어를 실행되게끔 하고 있다.

내 개인적인 생각으로는 Custom Command는 **정형화 되어있는 반복적인 작업** 에 좋지 않을까 생각된다.

장고 공식 문서에도 이렇게 적혀있다.

```
Standalone scripts

Custom management commands are especially useful for running standalone scripts or for scripts that are periodically executed from the UNIX crontab or from Windows scheduled tasks control panel.
```

Custom Command 는 주기적으로 실행하는 UNIX의 Crontab이나 Window의 Schedule Task, 독립 실행형(?) 스크립트에 특히 유용하다. ( ~~영어를 잘 못합니다 ㅠㅠ~~ )

라고 적혀있는 듯 하다.

우리 회사는 그럭저럭 잘 쓰고 있는 모양이다.

서론이 길었다. 바로 본론으로 들어가서 직접 만들어보도록 하겠다!

장고 공식 문서([이 곳](https://docs.djangoproject.com/en/1.10/howto/custom-management-commands/))를 많이 참고할 것입니다.

1. 위치 선정

Command 는 정해진 위치에 있어야만 실행 가능하다.

그 위치는 아래와 같다.

```
polls/
    __init__.py
    models.py
    management/
        __init__.py
        commands/
            __init__.py
            _private.py
            closepoll.py
    tests.py
    views.py
```

위는 Django 에서 하나의 App 구조이다.

참고로 Command 는 `<앱이름>/management/commands/<커맨드이름>.py` 와 같은 경로로 되어있다.

따라서 위와 같이 파일을 만들어놓고 `INSTALLED_APPS` 에 `polls` 를 추가해주게 되면 `python manage.py closepoll` 이라는 나만의 커스텀 Command 를 사용할 수 있는 것이다.

그리고 같이 나온 저 `_private.py` 라는 파일은 장고 공식 문서에 나온 설명에 따르면 저 파일은 커맨드로 사용할 수 없다고 한다.

즉 커맨드로 만들고 싶지 않은 유틸 같은 것들을 작성하면 좋을 것 같다.

그러면 `closepoll.py` 라는 Python 파일은 어떻게 작성해야 할까?

이것또한 뒤에서 설명해보겠다.

2. Command Python 파일 작성하기

Command Python 파일은 단 1개의 조건이 필요하다.

`Command` 라는 클래스를 작성하되, `BaseCommand` or `BaseCommand`의 서브 클래스 중 하나를 상속받는 클래스를 작성하여야 한다.

장고 공식 문서에는 다음과 같은 코드가 예시로 나온다.

```python
from django.core.management.base import BaseCommand, CommandError
from polls.models import Question as Poll

class Command(BaseCommand):
    help = 'Closes the specified poll for voting'

    def add_arguments(self, parser):
        parser.add_argument('poll_id', nargs='+', type=int)

    def handle(self, *args, **options):
        for poll_id in options['poll_id']:
            try:
                poll = Poll.objects.get(pk=poll_id)
            except Poll.DoesNotExist:
                raise CommandError('Poll "%s" does not exist' % poll_id)

            poll.opened = False
            poll.save()

            self.stdout.write(self.style.SUCCESS('Successfully closed poll "%s"' % poll_id))

```

먼저 `Command` 에서 상속받을 `BaseCommand` 와 Error를 내뿜어줄 `CommandError` 를 import 해준다.

그 다음은 `polls.models` 에서 Question 이라는 모델을 `as` 키워드를 사용해서 Poll 이라는 이름으로 import 한다.

`Command` 라는 이름의 클래스는 위에서 말했듯이 `BaseCommand` 라는 것을 상속받는다.

help 변수 같은 경우에는 `python manage.py closepoll --help` 를 입력했을 때 나오는 Command 에 대한 간단한 설명이다.

`add_arguments()` 와 `handle()` 은 뒤에 따로따로 설명을 해보겠다.

먼저 `add_arguments()` 부터 해보겠다.

메서드 이름에서부터 인자를 추가해줄 것만 같이 생겼다.

정답이다.( ~~응?~~ )

아래 코드를 보고 설명해보겠다.

```python
class Command(BaseCommand):
    def add_arguments(self, parser):
        # Positional arguments
        parser.add_argument('poll_id', nargs='+', type=int)

        # Named (optional) arguments
        parser.add_argument(
            '--delete',
            action='store_true',
            dest='delete',
            default=False,
            help='Delete poll instead of closing it',
        )

    def handle(self, *args, **options):
        # ...
        if options['delete']:
            poll.delete()
        # ...
```

`add_arguments()` 의 인자로는 넘겨줄 값의 Key 값, nargs, 그리고 type 이 있다.

먼저 하나 위의 코드를 보면 `poll_id` 를 options에서 Key 값으로 사용하는 걸 볼 수 있다.

저렇게 넘겨주기 위해서 필요하고, `nargs` 는 저도 이 포스팅을 작성하면서 처음 봤다. (아직 멀었다 ㅠㅠ)

그래서 좀 찾아보니 

```
nargs : 스위치나 파라미터가 받을 수 있는 값의 개수를 가리킨다. 이 값보다 많은 값이 들어오는 경우 무시된다. “+”로 설정하는 경우 1개 이상.
```

이라고 찾게 되었다. (아마 Number of Arguments 가 아닐까 싶긴 하다.)

공식 문서에서는 `+`로 설정해서 여러 개를 받을 수 있도록 한 뒤에 `for`문을 이용해서 처리할 수 있게끔 한 것 같다.

그리고 뒤에는 Named arguments 가 있다.

위에서 알아본 것은 Positional arguments, 즉 위치가 정해진(?) 뭐 차이는 느낌적인 느낌으로다가 받을 수 있겠다.

Named Arguments 는 나도 사용을 잘 안해본 지라 잘 모르겠으니 넘기겠다.

바로 `handle()` 로 넘어왔다.

`handle()` 같은 경우는 역시나 이름에서 알 수 있듯이 뭔가 대부분의 처리를 해줄 것 같은 메서드다.

역시나 정답이다. 커맨드의 로직을 여기서 처리해주면 된다.

그리고 `Command` 클래스에 대한 자세한 내용은 공식 문서인 [이 곳](https://docs.djangoproject.com/en/1.10/howto/custom-management-commands/#command-objects) 에 잘 나와있다.

그리고 `Command` 의 Subclass 들은 [여기](https://docs.djangoproject.com/en/1.10/howto/custom-management-commands/#basecommand-subclasses) 서 확인할 수 있다.

마지막 줄인 `self.stdout.write(~~)` 부분이 있다.

역시나 공식 문서에 관련 팁이 있다.

```
Note

When you are using management commands and wish to provide console output, you should write to self.stdout and self.stderr, instead of printing to stdout and stderr directly. By using these proxies, it becomes much easier to test your custom command. Note also that you don’t need to end messages with a newline character, it will be added automatically, unless you specify the ending parameter
```

Custom Command 를 사용하고 콘솔 출력을 제공하려면 stdout 및 stderr에 직접 인쇄하는 대신 self.stdout 및 self.stderr 에 기록해야 한다고 적혀있다.

그리고 또 end message 매개변수를 설정해주지 않으면 자동으로 개행문자가 추가된다.

ending 인자를 굳이 정해줄 필요없다.

Custom Command 에 대해서 조금 알아봤다.

관련된 한글 자료가 많이 없는 듯 하여 작성해보았는데 도움이 되셨길 바랍니다.

읽어주셔서 감사합니다 :)
