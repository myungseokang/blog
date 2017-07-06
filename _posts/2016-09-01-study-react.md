---
layout: post
title: "React 공부하기"
date: 2016-09-01
categories: React
author: leop0ld
---

React 공부하기
=====
> 이 블로그, 저 블로그 많이 참고했습니다.

페이스북에서 밀고있는 JS 라이브러리인 **React** 에 대해서 공부해볼 생각입니다.
요즘 굉장히 **핫** 해서 공부하는게 재밌을 것 같습니다.

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

- **npm** : Node Package Manager 의 약자로 JS 모듈들을 다운받을 수 있게 해주는 도구입니다. [npm 공식 홈페이지](https://docs.npmjs.com/getting-started/what-is-npm)

- **ES6** : ECMAScript6 의 약자로써 2016년 1월에 발표된 JavaScript 문법의 표준입니다. [ES6 홈페이지](http://es6-features.org/#Constants)

- **JSX** : Babel에서 지원하는 HTML or XML 같은 문법을 자동으로 native JavaScript로 변환해줍니다. [자세한 링크](https://leop0ld.github.io/posts/study-jsx)


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
- 사용법은 { this.props.propsKey } 로 사용 가능합니다.
- <> 괄호 안에 propsKey="propsValue" 이렇게 써주면 위에 말한 것처럼 사용 가능합니다.
- this.props.children 은 기본적으로 가지고 있는 props입니다.
- <Hello>Hello, React</Helllo> 의 경우 ```Hello, React``` 부분이 this.props.children 입니다.

## 6. state
- state 는 컴포넌트 내에서 변경 가능한 데이터를 지칭합니다.
- state 는 외부에서 데이터를 변경할 때, this.setState()를 사용해서 변경합니다.

<script async src="//pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-format="fluid"
     data-ad-layout="in-article"
     data-ad-client="ca-pub-1864899826477546"
     data-ad-slot="2703362319"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

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

**babel** 
– ECMAScript6 를 지원하지 않는 환경에서 ECMAScript6 Syntax를 사용 할 수 있게 해줍니다.

**webpack** 
– 모듈 번들러로서, 자바스크립트 파일들을 하나로 합쳐줍니다.

**webpack-dev-server** 
– wepback에서 지원하는 간단한 개발서버로서 별도의 서버를 구축하지 않고도 웹서버를 열 수 있으며 hot-loader를 통하여 코드가 수정될때마다 자동으로 리로드 되게 할 수 있습니다.

```
$ npm install -g babel webpack webpack-dev-server
```
위의 명령어로 설치가 가능합니다.
-g 옵션은 Global로 설치하겠다는 옵션입니다.


## 2. Project 생성

```
$ mkdir react_study && cd react_study
$ npm init
```

위의 명령어로 react_study라는 폴더를 만들고 nodejs 프로젝트를 init, 생성했습니다.

```
├── node_modules/      # Nodejs 패키지들
├── package.json       # 패키지 의존성 관리
├── public             # Public path
│   └── index.html     # index 페이지
├── src                # React js 프로젝트 루트
│   ├── components     # 컴포넌트 폴더
│   │   └── App.js     # App 컴포넌트
│   └── index.js       # webpack Entry Point
└── webpack.config.js  # webpack 설정 파일
```

그리고 프로젝트의 구조는 위와 같습니다.
이렇게 설정을 해줄 예정입니다.

<script async src="//pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-format="fluid"
     data-ad-layout="in-article"
     data-ad-client="ca-pub-1864899826477546"
     data-ad-slot="2703362319"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

## 3. Dependency 설치

React를 사용할 것이므로 React를 설치하고, 디펜던시에 추가해주어야 합니다.
디펜던시들은 package.json 파일에 쓰여집니다.

설치와 동시에 디펜던시에 추가할 수 있는 **--save** 옵션을 사용합니다.

```
$ npm install --save react react-dom
```

babel에 대한 디펜던시도 추가해주어야 합니다.
이것들은 개발 환경에서만 쓰일 것이기 때문에 **--save-dev** 옵션으로 설치해줍니다.

```
$ npm install --save-dev babel-core babel-loader babel-preset-react babel-preset-es2015 webpack webpack-dev-server
```

```json
"dependencies": {
  "react": "^15.3.1",
  "react-dom": "^15.3.1"
},
"devDependencies": {
  "babel-cli": "^6.14.0",
  "babel-core": "^6.14.0",
  "babel-loader": "^6.2.5",
  "babel-preset-es2015": "^6.14.0",
  "babel-preset-react": "^6.11.1",
  "webpack": "^1.13.2",
  "webpack-dev-server": "^1.15.1"
}
```


## 4. 여러 가지 설정

**Webpack 설정**

webpack.config.js 파일에서 webpack에 관한 설정을 해줄 수 있습니다.

```javascript
module.exports = {
    entry: './src/index.js',
 
    output: {
        path: __dirname + '/public/',
        filename: 'bundle.js'
    },
 
    devServer: {
        inline: true,
        port: 7777,
        contentBase: __dirname + '/public/'
    },
 
    module: {
            loaders: [
                {
                    test: /\.js$/,
                    loader: 'babel',
                    exclude: /node_modules/,
                    query: {
                        cacheDirectory: true,
                        presets: ['es2015', 'react']
                    }
                }
            ]
        }
};
```

요렇게 설정을 해줍니다.
entry 부분은 제일 먼저 어떤 파일을 읽어들일건지 설정하는 부분입니다.
entry 부터 시작해서 필요한 부분을 전부 읽어들인 뒤, 하나로 합쳐서 bundle.js 에 저장하게 됩니다.
추가적으로는, 모듈을 통하여 ES6 문법으로 작성된 코드를 ES5 형태로 변환도 해줍니다.
포트는 7777로 설정되었습니다.

그리고 추가적으로 npm start 명령어를 입력했을 때 개발 서버가 작동되게끔 package.json 파일에 scripts 부분에 start를 추가해줍니다.

```json
"scripts": {
  "start": "webpack-dev-server --hot"
},
```

이제 설정이 얼추 마무리 되었습니다.


## 5. First 앱 작성

App.js에 다음과 같이 작성했습니다.

<script src="https://gist.github.com/Leop0ld/fc21c66a385dc2ff2857b557311dfbec.js"></script>

그리고 아래는 index.html입니다.

```html
<!DOCTYPE html>
<html>
 
   <head>
      <meta charset="UTF-8">
      <title>React Study</title>
   </head>
 
   <body>
      <div id="root"></div>
      <script src="bundle.js"></script>
   </body>
 
</html>
```

마지막으로 아래는 webpack entry point인 index.js 파일입니다.

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import App from './components/App';
 
const rootElement = document.getElementById('root');
ReactDOM.render(<App />, rootElement);
```


## 6. Webpack 서버 실행, 결과 확인

이제 터미널에서

```
$ npm start
```

명령어만 입력해주면 webpack dev server가 실행됩니다.

이제 localhost:7777로 들어가서 확인해보면 아래와 같이 나옵니다.

<center>
  <img src="/assets/img/react1.png" alt="First React App" style="width: 400px; height: 300px; border-radius: 5px;"/>
</center>

감사합니다 :D
