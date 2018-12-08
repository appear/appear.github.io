---
title: React Interview (리액트 면접 질문) [111 ~ 120]
date: 2018-11-22 10:00:31
tags: React
---

# React Interview

원작자인 [sudheerj](https://github.com/sudheerj) 의 동의를 구하고 진행하는 번역본 입니다 :)     

처음 해보는 번역이라 어색한 문맥이 많습니다 :) ..

-------------------------------------------------------------------
| No. | Questions |
|---- | ---------
||**Core ReactJS**| |
|111| [What are the approaches to include polyfills in your create-react-app?](#what-are-the-approaches-to-include-polyfills-in-your-create-react-app) |
|112| [How to use https instead of http in create-react-app?](#how-to-use-https-instead-of-http-in-create-react-app) |
|113| [How to avoid using relative path imports in create-react-app?](#how-to-avoid-using-relative-path-imports-in-create-react-app) |
|114| [How to add Google Analytics for react-router?](#how-to-add-google-analytics-for-react-router) |
|115| [How to update a component every second?](#how-to-update-a-component-every-second) |
|116| [How do you apply vendor prefixes to inline styles in React?](#how-do-you-apply-vendor-prefixes-to-inline-styles-in-react) |
|117| [How to import and export components using react and ES6?](#how-to-import-and-export-components-using-react-and-es6) |
|118| [Why React component names must begin with a capital letter?](#why-react-component-names-must-begin-with-a-capital-letter) |
|119| [Why is a component constructor called only once?](#why-is-a-component-constructor-called-only-once) |
|120| [How to define constants in React?](#how-to-define-constants-in-react) |


111. ### What are the approaches to include polyfills in your create-react-app?
#### (create-react-app 에 polyfills를 포함시키는 방법은 무엇인가요?)

1. core-js 에서 수동 import:
- polyfills.js 파일을 만들어 root index.js 에서 import 합니다. `npm install core-js` 또는 `yarn add core-js` 를 추가하고 필요한 기능을 import 합니다.

```js
import 'core-js/fn/array/find'
import 'core-js/fn/array/includes'
import 'core-js/fn/number/is-nan'
```

2. Polyfill 서비스 사용:
- polyfill.io CDN 을 사용하여 다음의 줄을 index.html 에 추가하고, 사용자 지정 브라우저별 Polyfill을 검색합니다.

```js
<script src='https://cdn.polyfill.io/v2/polyfill.min.js?features=default,Array.prototype.includes'></script>
```

위의 스크립트에서 Array.prototype.includes 는 기본 스펙에 포함되지 않았기 때문에 기능을 요청했습니다.

112. ### How to use https instead of http in create-react-app?
#### (create-react-app 에서 http 대신 https 를 어떻게 사용하나요?)
`HTTPS = true` 설정이 필요합니다.  package.json 에 scripts 부분을 편집할 수 있습니다.

```js
"scripts": {
  "start": "set HTTPS=true && react-scripts start"
}
```

또는 `set HTTPS=true && npm start` 을 실행하세요

113. ### How to avoid using relative path imports in create-react-app?
#### (create-react-app 에서 상대경로 import 를 어떻게 피하나요?)
프로젝트의 root 에 `env` 파일을 만들고 import path 를 작성해주세요.

```js
NODE_PATH=src/app
```

설정후 개발서버를 재시작 해주세요. 상대 경로없이 src/app 안에 있는 모든것을 import 할 수 있습니다.

114. ### How to add Google Analytics for React Router?
#### (어떻게 React Router 에 Google Analytics 를 추가하나요?)
history 객체에 listener 를 추가하여 각 페이지 뷰를 기록하세요.

```js
history.listen(function (location) {
  window.ga('set', 'page', location.pathname + location.search)
  window.ga('send', 'pageview', location.pathname + location.search)
})
```

115. ### How to update a component every second?
#### (어떻게 매 초 마다 component 를 업데이트 하나요?)
setInterval() 를 사용하여 변경을 트리거해야합니다. 오류 및 메모리 누수를 방지하려면 component unmount 시 타이머를 제거해야합니다.

```js
componentDidMount() {
  this.interval = setInterval(() => this.setState({ time: Date.now() }), 1000)
}

componentWillUnmount() {
  clearInterval(this.interval)
}
```

116. ### How do you apply vendor prefixes to inline styles in React?
#### (React 에서 어떻게 인라인 스타일에 vendor prefixes 를 붙일수 있나요?)
React 에서는 [vendor prefixes](https://developer.mozilla.org/en-US/docs/Glossary/Vendor_Prefix) 를 자동으로 적용해주지 않습니다. vendor prefixes 를 수동으로 추가해야합니다.

```jsx harmony
<div style={{
  transform: 'rotate(90deg)',
  WebkitTransform: 'rotate(90deg)', // note the capital 'W' here
  msTransform: 'rotate(90deg)' // 'ms' is the only lowercase vendor prefix
}} />
```
 
117. ### How to import and export components using React and ES6?
#### (React와 ES6에서 어떻게 component 를 import 와 export 할 수 있나요?)
component 를 export 하려면 default 를 사용해야합니다.

```jsx harmony
import React from 'react'
import User from 'user'

export default class MyProfile extends React.Component {
  render(){
    return (
      <User type="customer">
        //...
      </User>
    )
  }
}
``` 

export 를 사용하면 MyProfile 이 모듈로 내보내집니다. 다른 component 의 이름을 사용하지 않아도 동일한 파일을 import 할 수 있습니다.

118. ### Why React component names must begin with a capital letter?
#### (왜 React compnent 의 이름은 대문자여야하나요?)
JSX 에서 소문자 태그 이름은 HTML 태그로 간주됩니다. 그러나 점 ((property accessors) 이 있는 대문자, 소문자 태그 이름은 HTML 태그로 간주되지 않습니다.

- <component /> compiles to React.createElement('component') (i.e, HTML tag)
- <obj.component /> compiles to React.createElement(obj.component)
- <Component /> compiles to React.createElement(Component)

119. ### Why is a component constructor called only once?
#### (왜 component 생성자는 한번만 호출되나요?)
React의 reconciliation  알고리즘에서는 custom component 가 다음 렌더링의 같은 위치에 나타나면 이전 component 와 동일하므로 인스턴스를 새로 만들지 않고 다시 사용한다고 가정합니다.

120. ### How to define constants in React?
##### (React 에서 어떻게 상수를 정의하나요?)
ES7 의 static 필드를 사용하여 상수를 정의 할 수 있습니다.

```js
class MyComponent extends React.Component {
  static DEFAULT_PAGINATION = 10
}
```

static 필드는 statge 3 의 제안된 부분입니다.
