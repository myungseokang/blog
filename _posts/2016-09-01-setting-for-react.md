---
layout: post
title:  "React 공부하기"
date:   2016-09-01
excerpt: "Studying React"
tag:
- React
comments: true
---

# React 공부하기

페이스북에서 밀고있는 JS 라이브러리인 **React** 에 대해서 공부해볼 생각입니다.
요즘 굉장히 핫 해서 공부하는게 재밌을 것 같습니다.

## 1. React 개념

우선 React가 어떤 일을 하는 애인지 알아보도록 하겠습니다.
React는 위에서 언급했듯이 페이스북에서 사용하는 JS 라이브러리 입니다.
정확하게는 **UI 컴포넌트 라이브러리** 입니다.
또한 React는 Yahoo, Netflix, Instagram 등등 다양한 서비스에서 유용하게 사용되고 있습니다.

Addition. 

**Babel?**
Babel이란 ES6 문법으로 작성된 자바스크립트를 ES5 문법으로 바꿔주는 역할을 합니다.
왜 ES5 문법으로 굳이 바꿔야하냐면 이유는 크로스 브라우징 때문입니다.
최신 브라우저는 ES6 문법을 지원하지만 오래된 브라우저는 지원을 안하기 때문입니다.


## 2. React 특징

리액트는 두개의 파일로 구성되어 있습니다(물론 min.js 버전도 있습니다.)

react.js file 그리고 react-dom.js file

react.js 파일은 컴포넌트 부분을 담당하고, react-dom.js 파일은 실제 DOM에 렌더링하는 부분을 맡고 있습니다.

그리고 React가 주목받고 있는 이유이자 가장 React다운 특징은 바로 **Virtual DOM**입니다.

## 3. npm? es6? jsx? 그게 뭔데?

- npm

- ES6: ECMAScript6 라는 말의 약자로써 2016년 1월에 발표된 JavaScript 문법의 표준입니다.

**JSX**: Babel에서 지원하는 HTML or XML 같은 문법을 자동으로 native JavaScript로 변환해줍니다.

