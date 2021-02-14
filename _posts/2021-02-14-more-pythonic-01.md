---
layout: post
title: "More Pythonic #1 - comprehension"
categories: posts
excerpt: "More Pythonic #1 - comprehension"
tags: [python]
author: myungseokang
comments: true
share: true
ads: true
date: 2021-02-12
---

> 이 글은 Pythonic 한 코드들을 개인적으로 학습하기 위한 포스팅이므로 두서없이 정리되었을 수 있습니다. 주의해주세요!

오랜만에 글이네요. 

이 포스팅은 시리즈로 연재할 계획이며, Pythonic 한 코드를 잊어버리고 있는 것 같아서 다시 배우는 느낌으로 작성해볼까 합니다.

이 글에서 다룰 주제는 좀 더 Pythonic 한 코드를 작성하게 도와주는 각종 자료형의 comprehension (컴프리헨션, 이하 comp) 에 대해 정리해볼까 합니다.


### list comprehension (리스트 컴프리헨션)

워낙 익숙한 문법이다보니 이 문법인지 모르고 접하는 경우도 꽤 있는 것 같습니다. (실제로 제가 그랬던 듯)

대표적인 listcomp 사용 예시는 아래와 같습니다.

```python
>>> alphabets = ['A', 'B', 'C']
>>> new_alphabets = [ord(alphabet) for alphabet in alphabets]
>>> new_alphabets
[65, 66, 67]
```

listcomp 는 일반적인 리스트 생성 방식과는 다르게 작동하는데, 그 중 하나가 listcomp 안에서 사용한 변수의 경우에는 고유한 scope 를 가지게 됩니다.

```python
>>> alphabets = ['A', 'B', 'C']
>>> new_alphabets = [ord(alphabet) for alphabet in alphabets]
>>> new_alphabets
[65, 66, 67]
>>> alphabet
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'alphabet' is not defined
>>> for alphabet in alphabets:
...     print(alphabet)
...
A
B
C
>>> alphabet
'C'
```

위 코드처럼 listcomp 에서 사용한 변수를 그 scope 밖에서 접근하려고 하면 `NameError` 가 발생합니다.

반면에 for 문에서 사용한 임시 변수의 경우, 당연히 접근이 가능한 모습을 볼 수 있습니다.

set comprehension (이하 setcomp), dict comprehension (이하 dictcomp) 도 있는데, 아래에서는 간략하게 2개에 대해서 짚고 넘어가보겠습니다.


### set comprehension (셋 컴프리헨션) & dictionary comprehension (딕셔너리 컴프리헨션)

setcomp 와 dictcomp 는 listcomp 와 비슷한 형태를 띄고 있지만 대괄호를 쓰는 게 아니라 중괄호(`{}`) 를 사용한다는 게 다릅니다.

```python
>>> # setcomp
>>> alphabets = ['A', 'B', 'C']
>>> new_alphabets = {ord(alphabet) for alphabet in alphabets}
>>> new_alphabets
set([65, 66, 67])
```


```python
>>> # dictcomp
>>> alphabets = ['A', 'B', 'C']
>>> new_alphabets = {
...     alphabet: ord(alphabet)
...     for alphabet in alphabets
... }
>>> new_alphabets
{'A': 65, 'C': 67, 'B': 66}
```


이런 식으로 comprehension 을 사용하면 훨씬 더 Pythonic 한 코드를 작성할 수 있습니다.

읽어주셔서 감사합니다.
