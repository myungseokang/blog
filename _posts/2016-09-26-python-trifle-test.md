---
layout: post
title: "Python으로 문자열 가지고 놀기"
date: 2016-09-26
excerpt: "Trifle text with Python"
tag:
- Python
comments: true
---

Python으로 문자열 가지고 놀기
=====
> Python 3.x 버전 기준

1. 구분자를 사용해서 문자열 자르기

다음은 파이썬 인터프리터를 이용한 구분자를 이용한 문자열 자르기의 예시입니다.

```python
>>> import re
>>> text = 'asd221; asdasdv, fklgj;      asdfh5t, ogjwoi'
>>> re.split(r'[;,\s]\s*', text)
['asd221', 'asdasdv', 'fklgj', 'asdfh5t', 'ogjwoi']
```

이렇게 re(파이썬 내부 정규식 모듈)의 split함수를 이용해서 문자열을 `;`,`,`을 기준으로 리스트 형태로 나타내주게 하는 예제입니다.
```str.split``` 보다 ```re.split```이 좋은 이유는 분리 문자를 더 다양하게 지정해서 좀 더 편하게 사용할 수 있습니다.

정규 표현식(Regex)은 쓰이는 데가 굉장히 많으므로 알아두면 매우매우 좋습니다.

2. 텍스트 매칭

매칭하려고자 하는 텍스트가 간단하다면 str.find()라는 함수도 있고, str.endswith(), str.startswith()라는 함수도 있습니다.

```python
filename = 'test.txt'
print(filename.endswith('.txt')) # True
print(filename.startswith('test')) # True
```


