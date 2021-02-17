---
layout: post
title: "More Pythonic #3 - generator"
categories: posts
excerpt: "More Pythonic #3 - generator"
tags: [python, more pythonic]
author: myungseokang
comments: true
share: true
ads: true
date: 2021-02-17
---

> 이 글은 Pythonic 한 코드들을 개인적으로 학습하기 위한 포스팅이므로 두서없이 정리되었을 수 있습니다. 주의해주세요!

오늘은 Python 에서 `iterator` (반복자) 혹은 `iterable` (반복형) 한 객체를 더 잘 사용하기 위한 `generator` (제너레이터) 에 대해서 알아볼까 합니다.

### generator (제너레이터)

Python 에는 `generator` (제너레이터) 라는 개념이 있는데, 이는 list 에 비해 메모리 공간을 효율적으로 사용합니다.

대충 뭔지는 알겠는데, 왜 효율적인지 어떻게 동작하는지에 대해 조금 더 깊이 알아보겠습니다.

`generator` 는 <a href="https://docs.python.org/3/howto/functional.html#generators" target="_blank">Python 문서</a>에서 아래처럼 설명합니다.

> Generators are a special class of functions that simplify the task of writing iterators. Regular functions compute a value and return it, but generators return an iterator that returns a stream of values.

영어가 나오니 잘 안 읽혀서 한글로 바꿔보면,

> 제너레이터는 iterator (반복자) 작성 작업을 단순하게 하는 특수한 함수의 클래스입니다. 일반적인 함수는 값을 계산하고 반환하지만, 제너레이터는 값 스트림을 반환하는 iterator (반복자) 를 반환합니다.

라고 하는데... 이해가 잘 되지 않으니, 아래에서 직접 사용해보면서 설명해보도록 하겠습니다.


### yield 키워드

`generator` 는 `yield` 키워드를 통해 명시적으로 만들 수 있습니다.

```python
>>> def gen_cats():
...     cats = ['Bengal', 'Scottish fold', 'Russian Blue']
...     for cat in cats:
...         yield cat
...
>>> 
>>> gen_cats()
<generator object gen_cats at 0x0150B5D8>
>>> [i for i in gen_cats()]
['Bengal', 'Scottish fold', 'Russian Blue']
```

이렇게 명시적으로 `generator` 를 반환하는 함수를 만들어 사용할 수도 있습니다.


### generator expression (제너레이터 표현식)

`generator expression` (제너레이터 표현식, 이하 `genexpr`) 은 위에서 설명했던 `generator` 를 사용할 수 있는 방법 중 하나인데, <a href="/posts/more-pythonic-01/" target="_blank">전 포스팅 (More Pythonic #1)</a> 나왔던 `listcomp` 와 아주 흡사합니다.

`listcomp` 와 다른 점은 대괄호(`[]`) 대신 소괄호(`()`) 를 사용한다는 점이 다릅니다.

아래 예시를 보면 이해가 더 쉽습니다.

```python
>>> alphabets = ['A', 'B', 'C']
>>> listcomp = [i for i in alphabets]
>>> generator_expr = (i for i in alphabets)
>>> listcomp
['A', 'B', 'C']
>>> generator_expr
<generator object <genexpr> at 0x00CEB610>
```

이렇게 만든 변수들을 print 해보면 위처럼 `listcomp` 로 만든 list 는 값을 실제로 확인할 수 있는 반면, `genexpr` 를 사용해서 만든 변수는 그렇지 않습니다.

그러면 인덱스로 접근하면 값을 가져올 수 있을까요?

```python
>>> generator_expr[0]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'generator' object is not subscriptable
>>> listcomp[0]
'A'
```

그것도 아니네요.

그럼 길이도 못 알아낼까요?

```python
>>> len(generator_expr)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: object of type 'generator' has no len()
```

정답입니다.

왜 이런걸까요?


### 도대체 왜 리스트보다 효율적인가요?

결론만 말하면, `generator` 는 정말 값을 반복시켜 가져오기 위한 `iterator` 를 만드는 데에 초점이 맞춰진 기능이기 때문입니다.

이 말을 풀어서 설명해보면,

`generator` 에 대한 설명 해석 중,

> 일반적인 함수는 값을 계산하고 반환하지만, 제너레이터는 값 스트림을 반환하는 iterator (반복자) 를 반환합니다.

라는 부분이 있는데, 이 부분 때문에 메모리를 효율적으로 사용하고 인덱스로는 접근할 수 없는 것이라고 할 수 있습니다.

제너레이터 함수를 호출하면 반환 값으로 단일 값 (위의 예시에선 `list`) 을 반환하지 않습니다.

`yield` 가 실행되면, `return` 키워드와 비슷하게 결과값을 반환합니다.

하지만 두 키워드의 가장 큰 차이점은 `yield` 의 경우, 실행되면 `generator` 의 실행 상태가 잠깐 중지되고 해당 함수의 지역 변수들이 보존된다는 점입니다.

그리고 다음에 해당 `generator`의 `__next__()` 메서드가 실행될 때, 위에서 잠깐 중지된 함수가 다시 실행되고 이것이 반복되는 것입니다.

따라서 메모리 상에 요소들에 미리 접근을 할 수 없습니다.

때문에 인덱스로 접근하거나 길이를 미리 알 수 없답니다.

그래서 `iterator` 와 `generator` 의 관계는,

**`generator` 를 통해 `iterator` 를 만들어낼 수 있는 관계**

정도로 생각할 수 있겠네요.

그래서 이렇게 메모리 공간을 효율적으로 이용하는 `generator` 의 경우에는 매우 많은 요소 (element) 를 다루거나 무한 스트림 (infinite stream) 을 다룰 때 아주 용이합니다.

다만 위에서 말한 인덱스로 접근하지 못하는 문제, 길이를 알 수 없는 문제는 고려해두면 좋겠네요.

다음에는 조금 더 새로운 주제로 찾아와볼게요. 읽어주셔서 감사합니다.
