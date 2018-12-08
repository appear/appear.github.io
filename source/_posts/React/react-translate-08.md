---
title: React Interview (리액트 면접 질문) [71 ~ 80]
date: 2018-11-13 10:00:31
tags: React
---

# React Interview

원작자인 [sudheerj](https://github.com/sudheerj) 의 동의를 구하고 진행하는 번역본 입니다 :)     

처음 해보는 번역이라 어색한 문맥이 많습니다 :) ..

-------------------------------------------------------------------
| No. | Questions |
|---- | ---------
||**Core ReactJS**| |
|71 | [How you implement Server-Side Rendering or SSR?](#how-you-implement-server-side-rendering-or-ssr) |
|72 | [How to enable production mode in React?](#how-to-enable-production-mode-in-react) |
|73 | [What is CRA and its benefits?](#what-is-cra-and-its-benefits) |
|74 | [What is the lifecycle methods order in mounting?](#what-is-the-lifecycle-methods-order-in-mounting) |
|75 | [What are the lifecycle methods going to be deprecated in React v16?](#what-are-the-lifecycle-methods-going-to-be-deprecated-in-react-v16) |
|76 | [What is the purpose of getDerivedStateFromProps() lifecycle method?](#what-is-the-purpose-of-getderivedstatefromprops-lifecycle-method) |
|77 | [What is the purpose of getSnapshotBeforeUpdate() lifecycle method?](#what-is-the-purpose-of-getsnapshotbeforeupdate-lifecycle-method) |
|78 | [What is the difference between createElement() and cloneElement() methods?](#what-is-the-difference-between-createelement-and-cloneelement-methods) |
|79 | [What is the recommended way for naming components?](#what-is-the-recommended-way-for-naming-components) |
|80 | [What is the recommended ordering of methods in component class?](#what-is-the-recommended-ordering-of-methods-in-component-class) |


71. ### How you implement Server Side Rendering or SSR?
#### (Server Side Rendering(SSR) 을 어떻게 구현하나요?)
React 는 이미 Node 서버에서 렌더링을 처리할 수 있도록 준비되어 있습니다.
특수한 버전의 DOM renderer 를 사용할 수 있으며, client side 와 동일한 패턴을 따릅니다.


```jsx harmony
import ReactDOMServer from 'react-dom/server'
import App from './App'

ReactDOMServer.renderToString(<App />)
```

이 method 는 HTML 을 문자열로 내보내고, 문자열은 서버 응답의 일부로 페이지 본문 내부에 배치 할 수 있습니다.
client side 에서 React 는 이미 렌더링된 content 를 감지하여 중단된 부분을 원활히 처리합니다.


72. ### How to enable production mode in React?
#### (React에서 어떻게 production 모드를 활성화 하나요)
Webpack 의 DefinePlugin 메서드를 사용하여 NODE_ENV 를 production 으로 설정해야
propType 의 유효성 검사 및 추가적인 경고 같은 것들을 제거할 수 있습니다.

production 모드와는 별도로 코드를 minify 하면 Uglify 의 dead-code 의 제거와 주석을 제거하여 bundle 크기를 줄일 수 있습니다.

 
73. ### What is CRA and its benefits?
#### (CRA는 무엇이고 CRA의 이점은 무엇인가요?)

Create-Reaction-App CLI 사용하면 설정 단계 없이 React applications 생성하고 실행할 수 있습니다.

CRA 를 사용하여 Todo App 을 만듭니다

```bash
# Installation
$ npm install -g create-react-app

# Create new project
$ create-react-app todo-app
$ cd todo-app

# Build, test and run
$ npm run build
$ npm run test
$ npm start
```

74. ### What is the lifecycle methods order in mounting?
#### (마운팅에서 lifecycle 메서드의 순서는 무엇인가요?)
lifecycle 메서드는 component 의 instance 가 생성되어 DOM에 삽입 될 때 호출됩니다.

- constructor()
- static getDerivedStateFromProps()
- render()
- componentDidMount()

75. ### What are the lifecycle methods going to be deprecated in React v16?
#### (React v16 에서 사용되지 않을 lifecycle 메서드는 무엇인가요?)
안전하지 않은 코딩 방법이 될 수 있는 lifecycle 메서드는 비동기적으로 렌더링시 문제가 될 것 입니다. 

- componentWillMount()
- componentWillReceiveProps()
- componentWillUpdate()

React v16.3 부터는 UNSAFE_가 prefix 붙고, prefix 가 없는 버전은 React v17 에서 제거됩니다.

76. ### What is the purpose of getDerivedStateFromProps() lifecycle method?
#### (getDerivedStateFromProps() lifecycle 메서드의 목적은 무엇인가요?)
새로운 정적 getDerivedStateFromProps() lifecycle 메서드는 component 가 인스턴스화 된 후, 다시 렌더링 되기 전에 호출됩니다.  
object 를 반환하여 state 를 update 하거나, null 을 반환하여 새로운 props 에서 state update 가 필요하지 않음을 나타낼 수 있습니다.

```js
class MyComponent extends React.Component {
  static getDerivedStateFromProps(props, state) {
    // ...
  }
}
```

이 lifecycle 메서드는 componentDidUpdate() 와 함께 componentWillReceiveProps() 의 사용 사례에 적용할 수 있습니다.

77. ### What is the purpose of getSnapshotBeforeUpdate() lifecycle method?
#### (getSnapshotBeforeUpdate() lifecycle 메서드의 목적은 무엇인가요?)
새로운 [getSnapshotBeforeUpdate()](https://reactjs.org/docs/react-component.html#getsnapshotbeforeupdate) lifecycle 메서드는 DOM update 직전에 호출됩니다. 
이 메서드의 반환값은 componentDidUpdate() 의 세번째 파라미터로 전달됩니다.

```js
class MyComponent extends React.Component {
  getSnapshotBeforeUpdate(prevProps, prevState) {
    // ...
  }
}
```

이 lifecycle 메서드는 componentDidUpdate()와 함께 componentWillUpdate () 의 사용 사례에 적용할 수 있습니다.

78. ### What is the difference between createElement() and cloneElement() methods
#### (createElement() 와 cloneElement() 의 차이점은 무엇인가요?)
JSX 에서 React element 는 UI element 를 타내는 React.createElement() 변환됩니다.  
React.cloneElement ()는 element 를 복제하고 새로운 props를 전달하는데 사용됩니다.

79. ### What is the recommended way for naming components?
#### (component 의 이름을 지정하는 권장방법은 무엇인가요?)
displayName 대신 reference 를 이용하여 component 의 이름을 정하는 것을 추천한다.

displayName 사용
```js
export default React.createClass({
  displayName: 'TodoApp',
  // ...
})
```

추천 방법:
```js
export default class TodoApp extends React.Component {
  // ...
}
```

80. ### What is the recommended ordering of methods in component class?
#### (component class 안에서 메서드의 추천 순서는 무엇인가요?)

마운트에서 부터 렌더링 단계의 권장 메서드 지정 순서  

- static methods
- constructor()
- getChildContext()
- componentWillMount()
- componentDidMount()
- componentWillReceiveProps()
- shouldComponentUpdate()
- componentWillUpdate()
- componentDidUpdate()
- componentWillUnmount()
- click handlers or event handlers like onClickSubmit() or onChangeDescription()
- getter methods for render like getSelectReason() or getFooterContent()
- optional render methods like renderNavigation() or renderProfilePicture()
- render()
