---
layout: post
title:  "Python 기초 문법 알아보기 - 1"
date:   2016-07-02
excerpt: "Learning Python Basic Grammar - 1"
categories:
- Python
- Python Grammar
comments: true
---

# Python 문법 알아보기 - 1

## 0. Python(파이썬)
> Python 3.x 버전 기준

일단 Python 문법을 배우기 전에 Python이라는 언어에 대해 알아두면 좋을 것 같아서 내용을 추가합니다.
Python이란 1991년 귀도 반 로섬(Guido van Rossum)이 개발한 고급 프로그래밍 언어입니다.
프로그래머가 바로 사용할 수 있는 라이브러리들과 통합 환경을 제공하는 'Battery Included' 개념을 기본으로 삼고 있다고 합니다.

Python의 특징은 다음과 같습니다.

- 인터프리터 기반의 객체지향 언어
- 플랫폼에 구애받지 않는 언어
- 동적 타이핑 방식의 언어
- 리플렉션을 지원하는 언어
- 확장성이 뛰어난 언어

다음 특징에 대해 자세히 알아보겠습니다.


**인터프리터 기반의 객체지향 언어**
Python은 바이트 코드(Byte code)의 컴파일을 지원하는 동적 인터프리팅 언어입니다...
라는 소리는 Python 인터프리터가 사용자도 모르는 사이에 바이트 코드(Byte code)를 생성해서 해당 코드를 실행할 때 더욱 빠르게 실행할 수 있다는 것입니다.
이러한 인터프리터 덕분에 Python 셸(Shell)에서 interpreter mode 로 코드를 한줄한줄 확인해가면서 작성할 수 있습니다.
이렇게 만들어진 바이트 코드(Byte code)는 플랫폼에 구애받지 않기 때문에 다른 플랫폼에서 다시 컴파일할 필요가 없습니다.
게다가 Python은 객체지향적인 특성도 가지고 있어서 클래스, 다중 상속 등등이 가능한 (짱짱(물론 글쓴이의 매우 주관적인 생각)) 언어입니다.


**플랫폼에 구애받지 않는 언어**
Python은 웬만한 플랫폼에서는 거의 다 작동하는 (짱짱(물론 글쓴이의 매우 주관적인 생각)) 언어입니다.


**동적 타이핑 방식의 언어**
Python은 루비(Ruby), 자바스크립트(Javascript), PHP 와 같이 동적 타이핑(Dynamic Typing) 방식의 언어입니다.
동적 타이핑(Dynamic Typing)이란? 프로그램을 실행할 때 타입 검사를 수행하는 것을 말합니다. C/C++ 은 정적 타이핑(Static Typing) 방식의 언어로 컴파일할 때 타입 검사를 진행하지만, 동적 타이핑 방식의 언어의 경우에는 프로그램을 실행할 때 검사하기 때문에 겉보기에는 문제가 없어보여도 숨어있는 경우가 더러 있습니다.


**리플렉션을 지원하는 언어**
Python은 Module, Class, Object와 같은 언어적 요소에 접근할 수 있는 리플렉션(Reflection)을 지원합니다.
(사실 이 부분은 공부가 더 필요하고 자세히는 모르는 부분입니다. 가르침 환영)
리플렉션(Reflection)을 이용해서 프로그램이 자기 자신의 모듈, 클래스, 객체 등을 조사하고 행위와 구조를 관리할 수 있습니다.(?해석좀...)
이러한 기술을 인트로스펙션(Introspection)이라고 합니다.
인트로스펙션(Introspection)이란? 특정 클래스가 어떤 클래스로부터 파생되었는지, 혹은 어떤 함수가 구현되어 있는지, 객체에는 어떤 속성이 있는지에 대한 상세한 정보를 런타임에 얻거나 조작하는 기술을 뜻합니다.
이러한 인트로스펙션(introspeciton) 덕분에 Python은 실행 시점에 프로그램의 내부 속성 정보를 조작할 수 있다고 합니다.


**확작성이 뛰어난 언어**
Python은 외부의 라이브러리로 쉽게 확장할 수 있습니다.
또한 컴파일된 모듈을 이용하면 새로운 데이터 타입으로도 이용할 수 있습니다.
새로운 모듈은 대부분 플랫폼에서 새롭게 컴파일된 모듈을 Python 인터프리터(interpreter) 내부로 동적 로딩할 수 있습니다.
Python은 접착성이 뛰어나서 C언어와 같은 컴파일 언어나 다른 다양한 언어를 이용해서 모듈화해서 가져올 수도 있고, Python을 모듈화해서 가져다가 붙일 수도 있습니다.


그 외에도 여러가지 특징이 있습니다.
Python의 탄생 이유 자체가 교육을 목적으로 설계되어서 Python 커뮤니티는 아무래도 높은 가독성의 코드를 지향합니다.
예로 들여쓰기로 코드 블럭을 나누는 것을 들 수 있습니다.
일반적으로 인터프리터(interpreter) 언어는 컴파일 언어보다 속도가 느리다고 여겨지고 있습니다.(실제 대부분은 그렇습니다.)
하지만 최근 들어서 인터프리터(interpreter) 언어도 속도를 높이기 위한 방법을 고안하여 다양한 방법들을 내놓고 있습니다.
정말정말 속도가 중요한 모듈같은 경우는 C언어 같은 컴파일 언어를 이용해서 별도의 모듈로 만들어 붙일 수도 있습니다.


## 1. Number(숫자 자료형)

```python
# 기본적인 사칙연산
print(5 + 6)   # 11
print(5 - 2)   # 3
print(3 * 8)   # 24
print(3 ** 3)  # 27 제곱
print(8 / 2)   # 4.0 float형
print(8 // 2)  # 4 int형
print(8 % 3)   # 2 나머지
```
들이 가능합니다.


## 2. String(문자열 자료형)

```python
test = "Hello World!"
print(test)  # Hello World!

test = 'Hello!'
print(test)  # Hello!

test = 'I don\'t need Coke!'
print(test)  # I don't need Coke!

test = "I don't need Coke!"
print(test)  # I don't need Coke!

```

"",'' 로 감싸진 문자열을 string으로 인식합니다.
싱글쿼터 혹은 더블쿼터를 문자열로 사용하려면 앞에 \ 가 들어가야 합니다.
다른 방법으로는 더블쿼터로 문자열을 감싸고 문자열 내에서 싱글쿼터를 사용하는 것입니다.

```python
test = r'C:\Nature'
print(test)  # C:\Nature
```

r'' 로 문자열을 감싸주게 되면 raw라는 뜻으로 아무 의미없는 문자열이라는 것을 나타내줍니다.

```python
first = 'Myungseo'
last = 'Kang'

print(first + last)  # Myungseo Kang
print(last * 5)      # KangKangKangKangKang
```

\+ 기호를 이용해서 문자열을 합치는 것이 가능합니다.
또한 * 기호를 이용해서 문자열을 반복하는 것이 가능합니다.


## 3. Slicing String(문자열 슬라이싱)

```python
test_str = 'Leopold'

print(test_str[0])   # L
print(test_str[1])   # e
print(test_str[-1])  # d
print(test_str[-2])  # l
```

List의 인덱스 부분에 음수를 넣어서 오른쪽부터 가져올 수 있습니다.

주의할 점은 음수로 인덱싱할 경우에는 0부터 시작이 아니라 1부터 시작합니다.

```python
print(test_str[2:5])  # opo
print(test_str[3:6])  # pol
print(test_str[:5])   # Leopo
print(test_str[3:])   # pold
```

이렇게 범위를 인덱스로 지정해서 호출하는 것도 가능합니다.

주의할 점은 콜론 앞의 숫자는 포함되지만 뒤의 숫자는 포함되지 않습니다.

시작지점을 지정하지 않으면 처음부터 콜론 뒷 부분 숫자의 인덱스까지 출력하고,

끝지점을 지정하지 않으면 콜론 앞 부분 숫자부터 끝까지 출력합니다.


## 4. if, elif, else(조건문)

```python
name = 'Leopold'

if name is 'Myungseo':
    print('Hello Myungseo')
elif name is 'Kang':
    print('Hello Kang!')
else:
    print('Hello Everyone!')
```

python의 조건문은 이런식으로 구성되어 있습니다.

```python
if 조건문:
    코드
elif 조건문2:
    코드
else:
    코드
```

이런 식으로 구성되어 있습니다.

특이한 점이 있다면 C언어처럼 else if를 쓰는 것이 아니라,
elif를 쓴다는 것입니다.


## 5. List(리스트 자료형)

List는 배열이라고 생각하면 편합니다.

```python
a = [] # a = list()와 동일
b = [1, 3, 5]
c = ['Leopold', 'Myungseo', 'Kang', 'L3opold7']
d = [7, 9, ['Myungseo', 'L3opold7']]
```

List 안에는 여러가지 자료형을 담을 수 있습니다.

List에도 Slicing String에서 말한 것들을 적용할 수 있습니다.

```python
print(b[-1])     # 5
print(c[-2])     # Kang
print(d[-1][0])  # Myungseo
```

이중 List에서 인덱싱은 다음과 같이 할 수 있습니다.

```python
# List 값 수정
test = [1, 2, 3, 4, 5]
test[3] = 6

print(test)  # [1, 2, 3, 6, 5]
```

이렇게 인덱스를 지정해서 직접 값을 바꿔줄 수 있습니다.

```python
# List 연속된 값으로 변경
test = [1, 2, 3, 4, 5]
test[2:3] = ['a', 'b', 'c']

print(test)  # [1, 2, 'a', 'b', 'c', '4', '5']
```

2이상 3미만의 인덱스 부분에 a,b,c List를 변경해주는 것입니다.

```python
# List 요소 삭제
test = ['a', 'b', 'c', 'd', 'e']
test[2:4] = []

print(test)  # ['a', 'b', 'e']
# del 함수 사용
test = ['a', 'b', 'c', 'd', 'e']
del test[2]

print(test)  # ['a', 'b', 'd', 'e']
```

del 함수를 사용해서 삭제할 수도 있습니다.

```python
test = ['a', 'b', 'c', 'd', 'e']
del test[2:4]

print(test)  # ['a', 'b', 'e']
```

마찬가지로 인덱스를 범위로 지정하는 것 또한 가능합니다.


**List 내장 함수들!**

```python
test = [1, 2]
test.append(3)  # 맨 뒤에 값 추가
print(test)  # [1, 2, 3]
```

append(x) 함수는 인자를 1개밖에 받지 않기 때문에 여러개의 인자를 넘겨줄 경우 에러가 납니다.

```python
test = [3, 1, 2, 5, 4]
test.sort()
print(test)  # [1, 2, 3, 4, 5]

test.sort(reverse=True)
print(test)  # [5, 4, 3, 2, 1]
```

sort() 함수는 List를 자동으로 정렬해줍니다.
역순으로 정렬하기 위해서는 sort 함수에 reverse 옵션을 True로 설정해주면 됩니다.

```python
test = [3, 1, 2]
test.reverse()
print(test)  # [2, 1, 3]
```

reverse() 함수는 현재의 List를 역순으로 뒤집어 줍니다.
정렬은 하지 않고 현재의 List를 역순으로 뒤집어 줍니다.

```python
test = [1, 2, 3, 4, 5]
print(test.index(3))  # 2
print(test.index(5))  # 4
```

index(x) 함수는 x 라는 값이 있는 경우 , x 의 인덱스를 반환해주는 함수입니다.

```python
test = [1, 2, 3, 4, 5]
test.insert(0, 6)
print(test)  # [6, 1, 2, 3, 4, 5]
```

insert(x, y) 함수는 x 위치에 y 라는 값을 삽입해주는 함수입니다.

```python
test = [1, 2, 3, 4, 3]
test.remove(3)
print(test)  # [1, 2, 4, 3]
```

remove(x) 함수는 첫 번째로 나오는 x 라는 값을 List에서 삭제해주는 함수입니다.
보시다시피 뒷부분에 있는 3은 삭제되지 않았습니다.

```python
test = [1, 2, 3]
print(test.pop())  # 3
print(test)        # [1, 2]
```

pop() 함수는 List의 가장 마지막 인덱스의 값을 반환해주고 그 값을 삭제해주는 함수입니다.
위의 예제에서 굳이 3이라는 값이 필요없을 경우에는 print() 함수를 빼도 상관없습니다.

```python
test = [1, 2, 3, 1, 1]
print(test.count(1))  # 3
```

count(x) 함수는 x 라는 값이 List 안에 몇 개나 있는지 반환해주는 함수입니다.

```python
test = [1, 2, 3]
test.extend([4, 5, 6])
print(test)  # [1, 2, 3, 4, 5, 6]
```

extend(x) 함수는 x 부분에 List를 받아서 원래의 List와 병합시켜주는 함수입니다.

List에서는 위와 같은 내장 함수들을 사용할 수 있습니다.
여기에 더해서 len() 함수로 List 값들의 개수를 얻을 수 있습니다.


## 6. Tuple(튜플 자료형)

Tuple은 조금 특이한 List라고 해도 무방할 정도로 List와 성격이 비슷합니다.

List에 대한 설명은 위에서 자세하게 했으므로 Tuple과의 차이점을 간단하게 언급하고자 합니다.

List는 [] 대괄호로 묶이지만 Tuple은 () 소괄호로 묶입니다.

```python
tp1 = ()
tp2 = (1,)
tp3 = (1, 2, 3, 4, 5)
tp4 = (1, 2, (3, 4, 5))
tp5 = 1, 2, 3
```

Tuple의 선언은 다음과 같이 할 수 있습니다.
List와 거의 비슷하지만 다른 점이 조금 있습니다.
1개의 요소만을 가질때 튜플은 tp2 와 같이 뒤에 반드시 콤마(,) 가 와야합니다.
또한 tp5 처럼 괄호를 생략해도 된다는 점입니다.

Tuple과 List의 차이점은 이뿐만이 아닙니다.
Tuple과 List의 가장 큰 차이점은 **Tuple은 값을 변경할 수 없다** 입니다.
List는 항시 값의 변화가 가능하지만 Tuple은 불가능합니다.
그래서 **값의 변화를 원하지 않는 List의 경우에는 Tuple로 선언하는 것이 바람직** 합니다.

간단하게 List와 비슷한 점도 짚고 넘어가겠습니다.

Tuple은 인덱싱, 슬라이싱, 병합, 반복 모두 가능합니다.

```python
tp1 = (1, 2, 3)
tp2 = (4, 5, 6)

print(tp1[2])     # 3
print(tp1[1:])    # (2, 3)
print(tp1 + tp2)  # (1, 2, 3, 4, 5, 6)
print(tp2 * 2)    # (4, 5, 6, 4, 5, 6)
```


## 7. Dictionary(딕셔너리 자료형)

Dictionary는 키=값 형태로 이루어진 자료형입니다.
이렇게 대응 관계를 나타내는 자료형을 연관 배열 혹은 Hash라고 합니다.
대표적인 예로는 루비의 Hash와 C#의 Dictionary가 있습니다.

이제 Dictionary라는 것은 어떻게 생겼는지 알아보도록 하겠습니다.

```python
dic1 = dict()
dic2 = {'k1': 'v1', 'k2': 'v2', 'k3': 'v3'}
dic3 = dict([('name', 'L3opold7'), ('phone', '010-1234-5678')])
dic4 = dict(firstname='Myungseo', lastname='Kang')
dic5 = {'ls': ['a', 'b', 'c']}

print(dic2)               # {'k1': 'v1', 'k3': 'v3', 'k2': 'v2'}
print(dic2['k2'])         # v2
print(dic3)               # {'phone': '010-1234-5678', 'name': 'L3opold7'}
print(dic3['name'])       # L3opold7
print(dic4)               # {'firstname': 'Myungseo', 'lastname': 'Kang'}
print(dic4['firstname'])  # Myungseo
print(dic5['ls'])         # ['a', 'b', 'c']
```

빈 Dictionary를 만들땐 dict() 함수를 사용하면 됩니다.
물론 내용이 있는 Dictionary를 만들 때 사용해도 됩니다!
그리고 value 값을 호출할 때는 Dictionary이름['키값'] 으로 호출하게 되면 값을 얻을 수 있습니다.
또한 Dictionary의 값으로 List도 넣을 수 있다.

```python
test = {1: 'first'}
test[2] = 'second'

print(test)  # {2: 'second', 1: 'first'}
```

Dictionary는 간단하게 키값을 지정해주고 추가해주면 됩니다.

```python
test = {1: 'first', 2: 'second', 3: 'third'}

del test[2]
print(test)  # {1: 'first', 3: 'third'}
```

삭제는 List에서 사용했듯이 del() 함수를 사용하면 됩니다.

```python
test = {'name': 'Myungseo', 'nickname': 'L3opold7', 'birthday': '0523'}
print(test.keys())    # dict_keys(['name', 'nickname', 'birthday'])
print(test.values())  # dict_values(['Myungseo', 'L3opold7', '0523'])
print(test.items())   # dict_items([('nickname', 'L3opold7'), ('name', 'Myungseo'), ('birthday', '0523')])
```

keys(), values() 함수를 통해서 딕셔너리의 key 혹은 value를 dict_keys 혹은 dict_values 객체로 얻을 수 있습니다.
items() 함수는 key와 value를 Tuple을 사용해서 묶은 값을 dict_items 라는 객체로 반환해줍니다.

```python
test = {'name': 'Myungseo', 'nickname': 'L3opold7', 'birthday': '0523'}
test.clear()
print(test)  # {}
```

clear() 함수를 이용해서 모두 지워버릴 수 있다!

```python
test = {'name': 'Myungseo', 'nickname': 'L3opold7', 'birthday': '0523'}

print(test.get('no_key'))  # None
print(test.get('name'))    # Myungseo
print(test['name'])        # Myungseo
print(test['no_key'])      # Error
```

test['no_key'] 의 경우에는 Error를 내뱉지만 test.get('no_key')는 None 객체를 반환하기 때문에 get(x, y) 함수를 쓰는 것이 더 적절해보입니다.
get(x, y) 함수는 Dictionary 안에 x 라는 키 값이 없을 경우 y 라는 디폴트 값을 반환해줍니다.

```python
test = {'name': 'Myungseo', 'nickname': 'L3opold7', 'birthday': '0523'}

print('name' in test)    # True
print('no_key' in test)  # False
```


## 8. Set(집합 자료형)

집합 자료형인 Set 입니다.
말 그대로 집합을 나타내기 위한 자료형입니다.
특징으로는 중복을 허용하지 않고, 순서가 없다는 것이 있습니다.

```python
s = set([1, 2, 3, 4, 5])
print(s)  # {1, 2, 3, 4, 5}

hello = set('Hello World!')
print(hello)  # {' ', 'H', '!', 'e', 'l', 'o', 'd', 'W', 'r'}
```

위와 같이 선언할 수 있습니다.
위에서 말한 두 가지 특징이 잘 드러나는 것을 볼 수 있습니다.

List와 Tuple은 순서가 있기 때문에 인덱싱을 통해 원하는 값을 가져올 수 있었지만, Set은 Dictionary와 비슷하게 순서가 없는 자료형이기 때문에 인덱싱이 불가능합니다.
만약 Set에서 인덱싱을 하고 싶다면 List나 Tuple로 형 변환을 시킨 뒤에 해야합니다.

아무래도 Set은 집합 자료형이다보니 교집합, 차집합, 합집합 등 집합 연산에 있어 매우 유리합니다.

```python
set1 = set([1, 2, 3, 4, 5, 6])
set2 = set([5, 6, 7, 8, 9, 0])

print(set1 & set2)  # {5, 6}
print(set1 | set2)  # {0, 1, 2, 3, 4, 5, 6, 7, 8, 9}
print(set1 - set2)  # {1, 2, 3, 4}
print(set2 - set1)  # {0, 8, 9, 7}
```

차례대로 교집합, 합집합, set1-set2 차집합, set2-set1 차집합 입니다.
위의 코드는 아래와 같이 나타낼수도 있습니다.

```python
set1 = set([1, 2, 3, 4, 5, 6])
set2 = set([5, 6, 7, 8, 9, 0])

print(set1.intersection(set2))  # {5, 6}
print(set1.union(set2))         # {0, 1, 2, 3, 4, 5, 6, 7, 8, 9}
print(set1.difference(set2))    # {1, 2, 3, 4}
print(set2.difference(set1))    # {0, 8, 9, 7}
```

이렇게 Set 자료형의 내장 함수를 통해서 교집합, 차집합, 합집합을 구할 수 있습니다.

```python
set1 = set([1, 2, 3, 4])
set1.add(4)
print(set1)  # {1, 2, 3, 4}

set1.add(5)
print(set1)  # {1, 2, 3, 4, 5}
```

add(x) 함수를 통해서 값을 추가할 수 있습니다.
Set 자료형의 특징답게 기존에 있던 값을 추가할 경우에는 추가되지 않습니다.

```python
set1 = set(1, 2)
set1.update([3, 4, 5])

print(set1)  # {1, 2, 3, 4, 5}
```

update(x) 함수를 통해서 여러 개의 값을 추가할 수 있습니다.
x의 위치에는 iterable, 즉 반복 가능한 자료형이 와야합니다.
List나 Tuple이 대표적인 예입니다.

```python
set1 = set([1, 2, 3, 4, 5])
set1.remove(3)

print(set1)  # {1, 2, 4, 5}
```

특정 값을 제거하고 싶을 경우에는 remove(x) 함수를 사용하면 됩니다.
x의 위치에는 제거하고 싶은 값을 적어줍니다.

1편은 여기서 마칩니다!
2편은 [여기](https://leop0ld.github.io/python-basic-grammar2/)서 보실 수 있습니다!
감사합니다 :)
