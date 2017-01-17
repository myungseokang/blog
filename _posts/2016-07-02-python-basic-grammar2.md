---
layout: post
title:  "Python 기초 문법 알아보기 - 2"
date:   2016-07-02
excerpt: "Learning Python Basic Grammar - 2"
categories:
- Python
- Python Grammar
comments: true
---

# Python 문법 알아보기 - 2
> Python 3.x 버전 기준

## 9. for, while(반복문)

for, while 문은 반복문입니다.
말그대로 반복시키기 위한 구문입니다.
for문의 기본 구조는 아래와 같습니다.

```python
test = [1, 2, 3, 4, 5]

for i in test:
    print(i)
    '''
    1
    2
    3
    4
    5
    '''
```

다음과 같이 i 부분에는 변수, test 부분에는 List나 Tuple 혹은 String 같은 반복가능한 변수가 옵니다.
그 다음에는 하고싶은 코드를 적으면 됩니다.
그리고 아래와 같이도 사용할 수 있습니다.

```python
test = [(1, 2), (3, 4)]

for (i, j) in test:
    print(i+j)
    '''
    3
    7
    '''
```

이렇게도 사용이 가능합니다.
C언어의 for문보다는 간편하게 사용할 수 있는 것 같습니다.

```python
for i in range(0, 10):
    print(i)
```

range 객체를 이용해서 쉽게 for문을 만들수도 있습니다.

간단하게 List 내장 함수와 for문을 이용한 예제를 보겠습니다.

```python
test_list = [1, 2, 3, 4, 5]
result = []

for num in test_list:
    result.append(num*3)

print(result)  # [3, 6, 9, 12, 15]
```

이런 코드를 아래와 같이 요약할 수 있습니다

```python
test_list = [1, 2, 3, 4, 5]

result = [num * 3 for in test_list]

print(result)  # [3, 6, 9, 12, 15]
```

이렇게 for문을 알아보았습니다.


## 10. Function(함수)

함수(Function)는 여러 프로그래밍 언어에서 등장하는 개념입니다.
함수를 사용함으로써 얻는 이점은 많습니다.
일단 코드의 가독성이 높아지고, 코드를 재사용 할수도 있습니다.

Python에서는 다음과 같이 함수를 정의합니다.

```python
def fuction_name(parameter):
    code here
```

호출할 때도 간단합니다.

```python
function_name(parameter)
```

처럼 호출해주게 되면 함수의 코드가 실행됩니다.

예를 들어보겠습니다.

```python
def hello(num):
    for i in range(0,num):
        print('hello,'+str(i))
```

이렇게 함수를 설정해주게 되면
hello 함수를 인자값으로 5를 넘겨서 실행해주게 되면

```
hello,0
hello,1
hello,2
hello,3
hello,4
```

이렇게 나오게 됩니다.
range 객체로 0, num 까지의 iterable 객체를 만들어줬고, print 함수로 i를 str 함수를 이용해 문자열로 바꾼 뒤, 출력해주고 있습니다.

이번에는 다른 예제를 다뤄보겠습니다.

int형 값을 넘겨받아서 그 수에 1100을 곱해서 print 해주는 함수입니다.

```python
def multiply_number(num):
    print(num * 1500)

prime_number_check(5)  # 7500
```

이런 식으로 multiply_number 함수안에 print 함수를 써서 multiply_number만 호출해도 print 함수까지 같이 호출할 수 있습니다.

다음에는 Lambda라는 익명 함수에 대해 알아보겠습니다.


## 11. Lambda(익명 함수)

Lambda(람다) 라는 익명 함수는 다른 프로그래밍 언어에도 있는 개념입니다.
일종의 작은 함수입니다.
지금까지 알아본 Function(함수)는 어떤 코드를 실행하는 함수를 정의하고, 인자를 전달하면서 함수를 호출합니다.
하지만 Lambda(익명 함수)는 일반 함수와 달리 함수명이 없습니다.
그리고 여러번 호출하지도 않고 1회성으로 사용하는 함수입니다.

```python
multiply_number = lambda x: x*1500
print(multiply_number(5))  # 7500
```

이런 식으로 익명 함수를 정의해서 사용할 수 있습니다.

조금만 더 복잡하게 들어가보겠습니다.

```python
func_range = list(map(lambda x: x*1000, (range(1, 6))))
print(func_range)  # [1000, 2000, 3000, 4000, 5000]
```

이 코드는 lambda 키워드로 Lambda(익명 함수)를 만들고, range 함수로 1~5 객체를 만들었습니다.
두개의 값을 map()이라는 함수를 사용해서 Mapping(매핑)을 해주고 있습니다. 즉, x의 값이 1,2,3,4,5가 됩니다.
그 다음 list() 함수로 List 객체를 만듭니다.

아무래도 Lambda(익명 함수)는 일반 함수보다 실제 사용이 적겠지만
그래도 Lambda(익명 함수)라는 것을 알아두면 일반 함수로 만들 것을 Lambda(익명 함수)로 간결하게 코딩할 수 있습니다.


## 12. File I/O(파일 입출력)

파일 입출력은 다양하게 많이 쓰입니다.
예를 들면 엑셀 파일 같은 것들을 읽어들일 때나 텍스트 파일에서 원하는 문자열을 추출할 때 등등 꼭 짚고 넘어가는 것이 일반적입니다.

아래의 코드로 설명을 시작해보도록 하겠습니다.

```python
f = open("filename.txt", 'w')
f.write("이 문자열은 파일에 기록됩니다.")
f.close()

# 파일 객체 = open(파일 이름, 모드)
```

모드의 종류에는 3가지가 있습니다.

| 모드 | 설명                                        |
|:----|:------------------------------------------:|
| r   | 읽기 모드 - 파일을 읽어들일 때 사용함              |
| w   | 쓰기 모드 - 파일에 내용을 쓸 때 사용함             |
| a   | 추가 모드 - 파일의 끝에 새로운 내용을 추가할 때 사용함 |
{: rules="groups"}

요렇게 3가지로 분류할 수 있습니다.

w 모드로 열게 되면 해당 파일이 이미 존재하면 원래 있던 내용을 다 지워지고 새로 작성됩니다.
또한 해당 파일이 없을 경우 새로 생성을 하게 됩니다.

```python
# w 모드 예제

f = open("example.txt", 'w')
for i in range(1, 6): # 1 이상부터 6 미만까지
    data = "%d번째 줄입니다.\n" % i
    f.write(data)
f.close()
```

1 이상부터 6 미만까지의 리스트를 range 메서드로 생성해서 1부터 5까지의 숫자가 i에 들어가면서 파일에 쓰여진다.

```python
# r 모드 예제

f = open("example.txt", 'r')
data = f.read()
print(data)
f.close()
```

파일을 전부 문자열로 리턴하는 read 메서드를 사용해서 파일 전체를 읽어들일 수 있다.

```python
# a 모드 예제

f = open("example.txt",'a')
for i in range(6, 11):
    data = "%d번째 줄입니다.\n" % i
    f.write(data)
f.close()
```

요렇게 example.txt 뒤에 추가해줄 수 있습니다.

그리고 추가적인 부분을 살펴보겠습니다.

```python
with open("example.txt", "w") as f:
    f.write("Keep Calm and Code Python")
```

f.open, f.close를 따로 써줄 필요 없이 `with` 키워드로 이렇게 작성해줄 수도 있습니다.


## 13. try ~ expect, finally(예외 처리)

Python에서 예외 처리를 하는 방법을 알아보겠습니다.

먼저 예외가 발생할 수 있는 부분을 try 문으로 묶은 다음에 except문으로 예외를 지정해서 처리해줄 수 있습니다.
다음 코드를 보겠습니다.

```python
try:
    num = 10 / 0
    print(num)
except Exception:
    print("0으로 나눌 수 없습니다.")
```

이 코드를 실행하게 되면 except 문만 실행되는 것을 알 수 있습니다.
10을 0으로 나눌 때 오류가 발생해서 except 문으로 넘어갔기 때문에 print(num) 부분은 실행되지 않습니다.

만약에 위의 코드에서 0이 아닌 5로 나누었다면 어떤 결과가 나올까요?

```python
try:
    num = 10 / 5
    print(num)
except Exception:
    print("0으로 나눌 수 없습니다.")
```

이 코드를 실행하게 되면 2.0 이 나올 것입니다.
except 문이 아닌 try 부분만 실행됨을 알 수 있습니다.

그리고 except 문에 Exception이 아닌 좀 더 명확한 예외를 표기할 수도 있습니다.
예를 들면 이런 식이죠.

```python
try:
    num = 10 / 0
    print(num)
except ZeroDivisionError as e:
    print("0으로 나눌 수 없습니다.")
```

바뀐 부분이 두 가지 있습니다.
except 문에 Exception이 아닌 ZeroDivisionError 로 명시한 것과 as e 라는 것을 추가한 것입니다.
또한 except (ZeroDivisionError, IndexError) as e: 처럼 () 로 묶어서 함께 처리하는 것도 가능합니다.

as e 부분은 말 그대로 alias e 라는 뜻으로 except 문 안에서 ZeroDivisionError 라는 이름을 e 라는 이름으로 쓰겠다는 말입니다.
그리고 ZeroDivisionError 는 Exception 중에 하나로 0으로 나눌 경우 발생하는 에러입니다.

ZeroDivisionError 는 Exception 클래스를 상속받기 때문에 Exception으로 호출해도 불리게 됩니다.

그 다음에는 finally 키워드에 대해 알아보겠습니다.

finally 키워드는 예외가 발생하든 안하든 무조건 실행시킬 코드입니다.

```python
try:
    num = 10 / 5
    print(num)
except ZeroDivisionError as e:
    print('0으로 나눌 수 없습니다.')
finally:
    print('실행 완료')
```

라고 코드를 적게 될 경우
2.0이 출력된 후, 실행 완료가 출력됩니다.

```python
try:
    num = 10 / 0
    print(num)
except ZeroDivisionError as e:
    print('0으로 나눌 수 없습니다.')
finally:
    print('실행 완료')
```

이 경우에도 except 문이 실행된 후에 실행 완료 가 출력됩니다.

Exception에 종류에는 다양한 것들이 있으므로 때에 맞춰서 다양한 Exception 클래스를 사용할 수 있습니다.
그리고 Exception 클래스를 상속받아서 자신만의 클래스를 정의해서 사용할 수도 있습니다.

그리고 try 문에서 raise 키워드를 사용해서 사용자가 직접 오류를 발생시키는 것 또한 가능합니다.

```python
class CustomError(Exception):
    def __str__(self):
        return "올바르지 않은 이름입니다."

def say_hello(name):
    if name == 'Leopold':
        print('Hello!')
    else:
        raise CustomError()

try:
    say_hello('Leopold')
    say_hello('myungseo')
except CustomError as e:
    print(e)
```

이런 식으로 raise, CustomError 를 모두 사용해보았습니다. 그리고 __str__ 함수 같은 경우에는 더블언더바 함수라고 하는 특정한 용도로 미리 정의해둔 함수입니다.

이렇게 예외 처리를 알아보았습니다.

2편은 여기서 마칩니다!
감사합니다 :)
