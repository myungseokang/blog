---
layout: post
title: "More Pythonic #2 - Duck Typing, iterator"
categories: posts
excerpt: "More Pythonic #2 - Duck Typing, iterator"
tags: [python, more pythonic]
author: myungseokang
comments: true
share: true
ads: true
date: 2021-02-15
---

> 이 글은 Pythonic 한 코드들을 개인적으로 학습하기 위한 포스팅이므로 두서없이 정리되었을 수 있습니다. 주의해주세요!
> Python 3.9 버전 기준으로 작성되었습니다.

오늘은 Python 에서 `iterator` (반복자) 혹은 `iterable` (반복형) 과 `sequence` (시퀀스) 에 대해 알아보려고 합니다.

그 전에 Python 에서 타입을 판별하는 Duck Typing (덕 타이핑) 이라는 기법에 대해서 잠깐 짚고 넘어가보겠습니다.

### Duck Typing (덕 타이핑)

> "만약 어떤 새가 오리처럼 걷고, 헤엄치고, 꽥꽥거리는 소리를 낸다면 나는 그 새를 오리라고 부를 것이다."

라는 말이 있듯이, Duck Typing 은 동적 타이핑의 한 종류로 런타임에 객체의 변수 혹은 메서드 집합으로 객체의 타입을 결정짓는 방법을 말합니다.

이게 무슨 말이냐함은,

```python
>>> # https://docs.python.org/ko/3/library/collections.abc.html#collections.abc.Sized
>>> from collections import abc
>>>
>>> abc.Sized
<class \'collections.abc.Sized\'>
>>> class Cup:
...     def __len__(self):
...         return 0
...
>>> issubclass(Cup, abc.Sized)
True
>>> isinstance(Cup(), abc.Sized)
True
```

위 코드에 있는 Python 공식 문서를 참고하면 `abc.Sized` 클래스가 `__len__()` 추상 메서드를 가지고 있는 것을 알 수 있습니다.

따라서 제가 만든 `Cup` 이라는 클래스에서 `__len__()` 메서드를 정의하게 된다면, `Cup` 클래스는 `abc.Sized` 클래스의 서브 클래스라고 볼 수 있다는 것이 Duck Typing 입니다.

이러한 개념은 Python 뿐만 아니라 다른 동적 타이핑 언어에서도 많이 사용하고 있는 방식으로 알고 있어서 알아두면 좋을 것 같습니다.


### sequence (시퀀스)

이제 본격적으로 `sequence` 부터 Python 공식 문서에서 알아보자면,

> An iterable which supports efficient element access using integer indices via the __getitem__() special method and defines a __len__() method that returns the length of the sequence.
> Some built-in sequence types are list, str, tuple, and bytes. Note that dict also supports __getitem__() and __len__(), but is considered a mapping rather than a sequence because the lookups use arbitrary immutable keys rather than integers.

> __getitem__() 메서드를 통해 정수 인덱스를 사용하여 효율적으로 개체에 접근을 지원하고, 시퀀스의 길이를 반환하는 __len__() 메서드를 정의하는 반복형입니다.
> 내장 시퀀스 타입은 list, str, tuple, bytes 가 있습니다. dict 타입도 __getitem__(), __len__() 메서드를 지원하지만 조회할 때, 정수가 아닌 불변 (immutable) 키를 사용하기 때문에 시퀀스가 아닌 매핑으로 간주됩니다.

요약하면, `sequence` 타입은 `__getitem__()`, `__len__()` 메서드를 지원하는 `iterable` 이 될 것 같습니다.


### iterable (반복형)

먼저 `iterable` 에 대해서 알아보면, Python 공식 문서에서는 아래와 같이 설명하고 있습니다.

> An object capable of returning its members one at a time.
> Examples of iterables include all sequence types (such as list, str, and tuple) and some non-sequence types like dict, file objects, and objects of any classes you define with an __iter__() method or with a __getitem__() method that implements Sequence semantics.

해석해보면,

> 한 번에 한 개의 멤버를 반환할 수 있는 객체.
> 예를 들어 모든 시퀀스 타입 (list, str, tuple) 을 포함한 반복형 그리고 논시퀀스 타입 (dict, file 객체 혹은 시퀀스 문법에 맞게 __iter__(), __getitem__() 메서드가 구현된 클래스) 가 있다.


설명에 따르면 `iterable` 타입은 `__iter__()` 메서드를 지원하거나 `__getitem__()` 메서드를 지원하는 클래스인데, <a href="https://docs.python.org/ko/3/library/collections.abc.html#collections.abc.Iterable" target="_blank">이 링크</a> 에서는 주의해서 봐야한다고 말하고 있습니다.

> isinstance(obj, Iterable)를 검사하면 Iterable로 등록되었거나 __iter__() 메서드가 있는 클래스를 감지하지만, __getitem__() 메서드로 이터레이트 하는 클래스는 감지하지 않습니다. 객체가 이터러블인지를 확인하는 유일하게 신뢰성 있는 방법은 iter(obj)를 호출하는 것입니다.

위의 말에 따라서 추상 베이스 클래스인 `abc.Iterable` 에는 `__iter__()` 만 포함되어 있지만, 실제로 객체가 이터러블한 지는 `iter()` 메서드를 호출해야 알 수 있다는 것입니다.


### iterator (반복자)

`iterator` 클래스 역시 <a href="https://docs.python.org/ko/3/library/collections.abc.html#collections.abc.Iterator" target="_blank">Python 공식 문서</a>에서 확인해봤을 때, `__iter__()`, `__next__()` 메서드를 제공하는 클래스라고 설명되어 있습니다.

```python
class CardDeck:
    def __init__(self):
        self.cards = list(range(1, 53))
        self.index = 0

    def __iter__(self):
        return self

    def __next__(self):
        self.index += 1
        return self.index


deck = CardDeck()
print(deck.index)
next(deck)
print(deck.index)
```

위 코드를 실행해보면,

```shell
0
1
```

위와 같은 실행 결과를 얻을 수 있고, 이렇게 하면 밑도 끝도 없이 정수의 최대값까지 실행될 것이기 때문에 iteration (반복) 을 종료하는 로직을 아래에서 구현해보겠습니다.

`__next__()` 메서드에서 임의의 한계값을 정해서 처리하면 쉽게 할 수 있습니다.

```python
class CardDeck:
    def __init__(self, index = 0):
        self.cards = list(range(1, 53))
        self.index = index

    def __iter__(self):
        return self

    def __next__(self):
        self.index += 1
        if self.index > 52:
            raise StopIteration
        return self.index


deck = CardDeck(index=52)
print(deck.index)
next(deck)
print(deck.index)
```

위 코드를 실행해보면,

```shell
52
Traceback (most recent call last):
  File ".\tmp.py", line 18, in <module>
    next(deck)
  File ".\tmp.py", line 12, in __next__
    raise StopIteration
StopIteration
```

이렇게 index 에 넣어준 값 52가 나오고, `__next__()` 가 불려지면서 `StopIteration` 오류가 발생하게 됩니다.

이런 식으로 `iterator` 클래스를 직접 만들고, 사용할 수 있습니다.

조금 더 실용적인 예제를 보자면,

```python
deck = CardDeck(index=50)

for card in deck:
    print(card)
```

이렇게 하고 실행 결과를 보게 되면,

```
51
52
```

for 문이 실행되면서 deck 객체에 대해서 `next()` 메서드가 호출되고, 기본적으로 `next()` 메서드에서 `StopIteration` 오류가 발생하면 iterator 가 중지되기 때문에 52까지만 출력되고 종료되는 것을 확인할 수 있습니다.

이렇게 이번 포스팅에서는 Python 에서의 Duck Typing, `sequence`, `iterable`, `iterator` 에 대해서 적어보았습니다. 원래 이 포스팅에서는 `iterable`, `iterator` 에 대해 얘기하려고 했으나 작성하다보니 포스팅 내용이 길어지게 되었네요...

그래도 읽어주셔서 감사합니다.
