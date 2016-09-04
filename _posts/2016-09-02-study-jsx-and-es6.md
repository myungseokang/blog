---
layout: post
title:  "JSX & ES6 공부하기"
date:   2016-09-02
excerpt: "Studying JSX & ES6"
tag:
- JavaScript
- ES6
- JSX
comments: true
---

JSX & ES6 간단 정리
=======

들어가기에 앞서 JSX를 굳이 하는 이유는 JSX를 썼을 때 개발하기 수월하고(러닝 커브가 있을지라도) 많은 예제들이 JSX를 사용하기에 저도 굳이 한번 배워보고자 합니다.

## 1. JSX 정리

### JSX 개념

**JSX**란?
JavaScript + XML 의 합성으로 JSX 라는 이름이 생겼다고 합니다.
기존의 XML을 허용하기 위해서 사용하는 JavaScript의 확장 문법입니다.
JSX는 단순히 XML을 허용만 하는게 아니라, JavaScript의 대한 추가적인 기능도 제공합니다. 
사용법은 아래와 같습니다.
```
// JSX가 없을 때는 이렇게
React.createElement('a', {href: 'https://leop0ld.github.io/posts/study-jsx-and-es6/'}, 'JSX & ES6 간단 정리')
// JSX를 사용했을 때는 이렇게!
<a href="https://leop0ld.github.io/posts/study-jsx-and-es6/">JSX & ES6 간단 정리</a>
```

**[JSX와 HTML과의 차이점](https://facebook.github.io/react/docs/jsx-gotchas.html)** 링크 참조

### JSX 특징

1. JSX의 장점
 - **빠르다**. 컴파일링 되면서 최적화가 되기 때문에 빠르다
 - **익숙하다**. XML이나 HTML에 익숙한 사람들도 JSX 금방 익힐 수 있습니다.
 - **의미를 잘 나타낼 수 있다**. JavaScript 코드를 마크업 형태로 만들어서 의미를 잘 드러낼 수 있습니다.
 - **시각화가 잘된다**. 코드를 더욱 간결하고 읽기 쉽게 만들어준다.
 - **추상화 할 수 있다**. 서로 다른 JavaScript를 사용해도 같은 JSX를 지원하게 할 수 있다. JSX 트랜스파일러를 이용해서 마크업 -> JS 과정을 추상화 할 수 있어서 가능한 일입니다.

2. [React 공식 홈페이지](https://facebook.github.io/react/)
 - [링크](https://facebook.github.io/react/docs/displaying-data.html)에 JSX 부분 나와 있습니다. 개쩝니다. 영어 된다면 꼭 봅시다.

### JSX 문법


## 2. ES6 정리

### ES6 개념

### ES6 특징

### ES6 문법
