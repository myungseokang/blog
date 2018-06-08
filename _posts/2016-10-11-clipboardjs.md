---
layout: post
title: "Bootstrap 없이 Clipboard.js 적용하기"
categories: posts
excerpt: "Applying Clipboard.js without Bootstrap"
tags: [javascript]
author: leop0ld
comments: true
share: true
ads: true
date: 2016-10-11
---

Bootstrap 없이 Clipboard.js 적용하기
===

지금 진행중인 프로젝트에 Clipboard.js를 적용해야 하는데 Bootstrap을 사용하지 않아서 Copied라는 팝업을 띄우는 데에 많이 애를 먹었습니다;
되게 간단한 부분이었음에도 불구하고 굉장한 삽질을 했기에 간단하게나마 적어보려고 합니다.

먼저 Github 레포지토리에 별 부터 살포시 눌러줍니다. [Cilpboard Github Repository](https://github.com/zenorocha/clipboard.js/)

그런 다음에 사용하는 방법을 살펴보니... 너무나도 간단했습니다.
그냥 cdn으로 넣어주고 ```new Clipboard('.btn');``` 하면 되잖아? 라고 간단하게 생각했습니다.
그래서 jsDeliver에 있는 cdn을 사용해서 적용해주고 간단하게 demo 코드를 따라해보았습니다.

저의 경우는 button을 누를 경우 정해진 텍스트가 복사되게끔 하는 것이었기 때문에

```html
<button class='btn' data-clipboard-text="복사되는 텍스트는 여기에">
  COPY
</button>
```

이렇게 해주고 아래 부분에 다음과 같은 js를 추가해주었습니다.

```javascript
var clipboard = new Clipboard('.btn');

clipboard.on('success', function(e) {
    console.info('Action:', e.action);
    console.info('Text:', e.text);
    console.info('Trigger:', e.trigger);
        
    e.clearSelection();
});

clipboard.on('error', function(e) {
    console.error('Action:', e.action);
    console.error('Trigger:', e.trigger);
});
```

여기까지는 너무나 간단했습니다.

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

버튼을 눌러보니 복사는 잘됬으나 tooltip이 뜨지 않았습니다.
aria-label이라는 놈이 필요했습니다.
그냥 무작정 추가해주었더니 되지 않았습니다... ㅠㅠ

그래서 StackOverflow에는 있겠지 하고 검색을 해보았습니다만, 영어가 부족하고 검색 실력도 부족했던 저라서 찾지 못하고...
결국에는 [Clipboard 페이지](http://clipboardjs.com)로 들어가서 개발자 도구>Source>clipboardjs.com/assets/scripts/tooltip.js 라는 파일을 보게 되었습니다.
(이게 minify 되어 있기때문에 이것을 왼쪽 아래에 있는 {}(Pretty Print) 버튼을 클릭해서 예쁘게 보시면 됩니다.)
갑자기 이것을 보게된 이유는 검색을 하다하다가 clipboardjs 페이지에는 있는데 왜 안되지 라면서 한탄을 하다가 보게 되었습니다.

각설하고 tooltip.js의 소스코드를 보게되면

```javascript
var btns = document.querySelectorAll('.btn');
for (var i = 0; i < btns.length; i++) {
    btns[i].addEventListener('mouseleave', function(e) {
        e.currentTarget.setAttribute('class', 'btn');
        e.currentTarget.removeAttribute('aria-label');
    });
}
function showTooltip(elem, msg) {
    elem.setAttribute('class', 'btn tooltipped tooltipped-s');
    elem.setAttribute('aria-label', msg);
}
function fallbackMessage(action) {
    var actionMsg = '';
    var actionKey = (action === 'cut' ? 'X' : 'C');
    if (/iPhone|iPad/i.test(navigator.userAgent)) {
        actionMsg = 'No support :(';
    } else if (/Mac/i.test(navigator.userAgent)) {
        actionMsg = 'Press ⌘-' + actionKey + ' to ' + action;
    } else {
        actionMsg = 'Press Ctrl-' + actionKey + ' to ' + action;
    }
    return actionMsg;
}
```

이러한 소스코드를 만나볼 수 있습니다.
자세히 보니 mouseleave 이벤트를 잡아서 aria-label이라는 속성을 제거하게하는 반복구문이 있습니다.
setAttribute는 속성값을 설정해주는 놈입니다.
따라서 class 가 tooltipped tooltipped-s로 바뀌는 것뿐이지 다른 추가적인 작업이 없기에 css까지 봐보기로 했습니다.
tooltipped 클래스와 tooltipped-s 클래스가 쓰인 곳을 가져와보니 아래 코드 같았습니다.

```css
.tooltipped {
    position: relative
}

.tooltipped:after {
    position: absolute;
    z-index: 1000000;
    display: none;
    padding: 5px 8px;
    font: normal normal 11px/1.5 Helvetica,arial,nimbussansl,liberationsans,freesans,clean,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";
    color: #fff;
    text-align: center;
    text-decoration: none;
    text-shadow: none;
    text-transform: none;
    letter-spacing: normal;
    word-wrap: break-word;
    white-space: pre;
    pointer-events: none;
    content: attr(aria-label);
    background: rgba(0,0,0,0.8);
    border-radius: 3px;
    -webkit-font-smoothing: subpixel-antialiased
}

.tooltipped:before {
    position: absolute;
    z-index: 1000001;
    display: none;
    width: 0;
    height: 0;
    color: rgba(0,0,0,0.8);
    pointer-events: none;
    content: "";
    border: 5px solid transparent
}

.tooltipped:hover:before,.tooltipped:hover:after,.tooltipped:active:before,.tooltipped:active:after,.tooltipped:focus:before,.tooltipped:focus:after {
    display: inline-block;
    text-decoration: none
}

.tooltipped-s:after {
    top: 100%;
    right: 50%;
    margin-top: 5px
}

.tooltipped-s:before {
    top: auto;
    right: 50%;
    bottom: -5px;
    margin-right: -5px;
    border-bottom-color: rgba(0,0,0,0.8)
}

.tooltipped-s:after {
    -webkit-transform: translateX(50%);
    -ms-transform: translateX(50%);
    transform: translateX(50%)
}
```

그래서 이것을 한군데에 모아보면 다음과 같은 코드가 나옵니다.

<script src="https://gist.github.com/Leop0ld/e8b7234421049657767b11ef5cd4181d.js"></script>

이렇게 쓰니 tooltip이 잘 떠주었습니다.
도움 되셨길 바랍니다.

읽어주셔서 감사합니다 :D