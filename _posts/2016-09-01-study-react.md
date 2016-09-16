---
layout: post
title:  "React 공부하기"
date:   2016-09-01
excerpt: "Studying React"
tag:
- JavaScript
- React
comments: true
---

React 공부하기
=====
> 이 블로그, 저 블로그 많이 참고했습니다.

페이스북에서 밀고있는 JS 라이브러리인 **React** 에 대해서 공부해볼 생각입니다.
요즘 굉장히 핫 해서 공부하는게 재밌을 것 같습니다.

# Index

* [React 개념](#react-개념)
* [npm? es6? jsx? 그게 뭔데?](#npm?-es6?-jsx?-그게-뭔데?)
* [React 특징](#react-특징)
* [React 개발환경 세팅하기](#react-개발환경-세팅하기)

-----

# React 개념

우선 React가 어떤 일을 하는 애인지 알아보도록 하겠습니다.
React는 위에서 언급했듯이 페이스북에서 사용하는 JS 라이브러리 입니다.
정확하게는 **UI 컴포넌트 라이브러리** 입니다.
또한 React는 Yahoo, Netflix, Instagram 등등 다양한 서비스에서 유용하게 사용되고 있습니다.
React는 View 레이어만 담당하기 때문에 다른 부분 에는 다른 기술(EX. Node.js, AngularJS) 같은 것이 필요합니다.


Addition.

**Babel?**
Babel이란 ES6 문법으로 작성된 자바스크립트를 ES5 문법으로 바꿔주는 역할을 합니다.
왜 ES5 문법으로 굳이 바꿔야하냐면 이유는 크로스 브라우징 때문입니다.
최신 브라우저는 ES6 문법을 지원하지만 오래된 브라우저는 지원을 안하기 때문입니다.


# npm? es6? jsx? 그게 뭔데?

- npm: Node Package Manager 의 약자로 JS 모듈들을 다운받을 수 있게 해주는 도구입니다. [npm 공식 홈페이지](https://docs.npmjs.com/getting-started/what-is-npm)

- ES6: ECMAScript6 의 약자로써 2016년 1월에 발표된 JavaScript 문법의 표준입니다. [ES6 홈페이지](http://es6-features.org/#Constants)

- **JSX**: Babel에서 지원하는 HTML or XML 같은 문법을 자동으로 native JavaScript로 변환해줍니다. [자세한 링크](https://leop0ld.github.io/posts/study-jsx)


# React 특징

React의 특징을 알아보도록 하겠습니다.

## 1. Virtual DOM
- Virtual DOM이란 쉽게 말해서 기존에 있던 View와 새로이 만들어진 Virtual View를 비교해서 다른 부분만 렌더링을 해주는 방법입니다. 이 방법 덕분에 React의 속도가 빠를 수 있는 것 같습니다.

## 2. 컴포넌트(Component)
- React는 컴포넌트 로 이루어져 있기 때문에 컴포넌트 단위로 개발하는 것이 좋습니다.

## 3. 단방향(Unidirectional)
- React는 Action –> Dispatcher –> Store –> View 이렇게 단방향으로 데이터를 전달하기 때문에 데이터를 추적하는게 비교적 쉽고, 디버깅 또한 편리합니다.
- 이러한 특징을 살려서 Flux 패턴을 살려서 Redux([Redux 홈페이지](http://redux.js.org/)) 라는 라이브러리를 많이 사용합니다.

## 4. FLUX Pattern / Architecture
- Action > Dispatcher > Store > View 구조로 데이터를 전달하고 업데이트 할 수 있도록 구성된 패턴입니다.
- 위에서 말했듯이 Flux 패턴을 아주 잘 살린 Redux 라는 라이브러리를 많이 사용합니다.

## 5. props
- props 는 컴포넌트 내에서 사용할 변경 불가능한 데이터를 지칭합니다.
- props 는 Parent(부모) 에서 Child(자식) 에게 데이터를 전달할 때 사용합니다.

## 6. state
- state 는 컴포넌트 내에서 변경 가능한 데이터를 지칭합니다.
- state 는 외부에서 데이터를 변경할 때, this.setState()를 사용해서 변경합니다.


# React 개발환경 세팅하기

방법은 두 가지가 있습니다.

1. 웹 IDE를 이용합니다.
 - 사실상 제일 쉽고 빠른 방법입니다. 하지만 불편함을 느낄 수도 있습니다.(규모가 커졌을 때)
 - 웹 IDE도 무지 많습니다(Codepen, Goorm, C9, Nitrous etc)

2. 로컬에 개발환경을 직접 세팅합니다.
 - 솔직히 귀찮다.(지금은 세팅을 해놨지만 이것저것 알아야할 것도 많고 귀찮습니다.)
 - 그치만 한번 세팅해놓고, 문서화 해놓으면 나중이 편합니다.(Pyenv 같은 경우도 문서화 해놨더니 매우 편리합니다.)

필자는 후자를 추천하는 바입니다.
왜냐면 필자는 js를 계속 공부할 것이고, React 뿐만 아니라 다른 js 프레임워크들도 공부해서 사용할 생각이 있기 때문입니다.
2번의 경우 npm, webpack 등등 귀찮고 어려운 것들을 사용하게 됩니다.
이것들은 후에 도움이 많이 되겠거니 하면서 열심히 친숙해져 가고 있습니다.

필자는 2번을 선택했기 때문에 2번으로 설명해드리겠습니다.
1번으로 진행할 분들은 [JSFiddle](https://jsfiddle.net/) 이라는 것을 써보시는 걸 추천드립니다.
JS에 특화된 웹 IDE 같습니다.
하지만 JavaScript 를 한 파일에 작성해야 하고, ES6 문법이 지원되지 않는 단점이 있습니다. 참고하시면 좋을 것 같습니다.

## 0. 사용할 것들 미리 알아보기

* React: 공부를 하게 될 UI 컴포넌트 라이브러리

* webpack: 모듈 번들러로, 의존성을 가진 모듈들을 다루고, 그 모듈로부터 정적인 asset을 생성한다.

* Redux: Redux는 자바스크립트 앱을 위한 예측 가능한 상태 컨테이너입니다.


여기서는 추후 적용해보고 싶은 것들입니다.

* Node.js: 이미 npm을 사용하고 있지만 Node.js는 사용하고 있지 않아서 Express.js 같은 프레임워크의 형태로 사용할 예정입니다.

* MongoDB: NoSQL(Not Only SQL)이라는 의미의 데이터베이스입니다. Express를 사용하면서 같이 사용해보고 싶습니다.

* JEST: Test를 위한 JS 프레임워크를 사용해볼 예정입니다.

## 1. Global Package 설치하기

## 2. Project 생성

## 3. Dependency 설치

## 4. 여러 가지 설정

## 5. First 앱 작성

## 6. Webpack 서버 실행
