---
title: React Interview (리액트 면접 질문) [130 ~ 139]
date: 2018-12-01 10:00:31
tags: React
---

# React Interview

원작자인 [sudheerj](https://github.com/sudheerj) 의 동의를 구하고 진행하는 번역본 입니다 :)     

처음 해보는 번역이라 어색한 문맥이 많습니다 :) ..

-------------------------------------------------------------------
| No. | Questions |
|---- | ---------
||**Core ReactJS**| |
|131| [What are the \<Router> components of React Router v4?](#what-are-the-router-components-of-react-router-v4) |
|132| [What is the purpose of push and replace methods of history?](#what-is-the-purpose-of-push-and-replace-methods-of-history) |
|133| [How do you programmatically navigate using React router v4?](#how-do-you-programmatically-navigate-using-react-router-v4) |
|134| [How to get query parameters in React Router v4](#how-to-get-query-parameters-in-react-router-v4) |
|135| [Why you get "Router may have only one child element" warning?](#why-you-get-router-may-have-only-one-child-element-warning) |
|136| [How to pass params to history.push method in React Router v4?](#how-to-pass-params-to-historypush-method-in-react-router-v4) |
|137| [How to implement default or NotFound page?](#how-to-implement-default-or-notfound-page) |
|138| [How to get history on React Router v4?](#how-to-get-history-on-react-router-v4) |
|139| [How to perform automatic redirect after login?](#how-to-perform-automatic-redirect-after-login) |

131. ### What are the <Router> components of React Router v4?
#### (React router v4 의 <Router> component 는 무엇이 있나요?)

React router v4는 아래와 같은 3가지 component 를 제공합니다.

```jsx harmony
<BrowserRouter>
<HashRouter>
<MemoryRouter>
```

위의 components 들은 browser, hash, 그리고 memory history 인스턴스를 만듭니다.
React Router v4 는 Router Object 의 context 를 통해 history 인스턴스의 속성과 메서드를 이용할 수 있게 해줍니다.

132. ### What is the purpose of push() and replace() methods of history?
#### (history의 push() 와 replace()  메서드의 목적은 무엇인가요?)
history 인스턴스에는 navigation 을 위한 두 가지 메서드가 있습니다.

- push()
- replace()

방문한 위치의 history 를 배열로 생각한다면, push() 는 배열에 새 위치를 추가하는 것이고 replace() 는 현재의 위치를 새 위치로 변경하는 것입니다.

133. ### How do you programmatically navigate using React Router v4?
#### (어떻게 프로그래밍 방식으로 React Router v4 의 navigate  를 사용하나요?)

component 안에서 프로그래밍 방식으로 routing/navigation 을 수행하는 세가지의 다른 방법이 있습니다.

- withRouter() higher-order function 을 사용하기:
withRouter () higher-order function 은 history object 를 component 의 prop 로 삽입합니다.
이 객체는 context 의 사용을 피하기 위해 push() 그리고 replace() 메서드를 제공합니다.

```jsx harmony
import { withRouter } from 'react-router-dom' // this also works with 'react-router-native'

const Button = withRouter(({ history }) => (
  <button
    type='button'
    onClick={() => { history.push('/new-location') }}
  >
    {'Click Me!'}
  </button>
))
```

- <Route> component 그리고 render props 패턴 사용하기:
<Router> component 는 withRouter() 와 같은 prop 를 전달하므로, history prop 를 통해 history 메서드에 접근할 수 있습니다.

```jsx harmony
import { Route } from 'react-router-dom'

const Button = () => (
  <Route render={({ history }) => (
    <button
      type='button'
      onClick={() => { history.push('/new-location') }}
    >
      {'Click Me!'}
    </button>
  )} />
)
```

- context 사용
이 옵션은 추천되지 않으며 불안정한 API 로 처리됩니다.

```jsx harmony
const Button = (props, context) => (
  <button
    type='button'
    onClick={() => {
      context.history.push('/new-location')
    }}
  >
    {'Click Me!'}
  </button>
)

Button.contextTypes = {
  history: React.PropTypes.shape({
    push: React.PropTypes.func.isRequired
  })
}
```

134. ### How to get query parameters in React Router v4?
#### (React Router v4 에서 어떻게 query parameters 가져오나요?)
수년동안 다른 구현 지원에 대한 사용자들의 요청이 있었기 때문에 React Router v4의 query strings 을 parse 하는 기능이 제거되었습니다.
그래서 사용자들이 선호하는 구현방식을 선택하도록 결정되었습니다.
추천 방법은 query strings 라이브러리를 사용하는것 입니다.

```jsx harmony
const queryString = require('query-string');
const parsed = queryString.parse(props.location.search);
```

native 방식을 원한다면 URLSearchParams 을 사용할 수 있습니다.

```jsx harmony
const params = new URLSearchParams(props.location.search)
const foo = params.get('name')
```

IE11에는 polyfill을 사용해야합니다.

135. ### Why you get "Router may have only one child element" warning?
#### (왜 "Router 는 오직 하나의 자식 element 만 있을 수 있습니다" 라는 경고를 받나요?)
<Switch> 는 route 를 독점적으로 렌더링하기 때문에 <Switch> 으로 Route 들을 감싸야합니다.

먼저 Switch 를 가져와야 추가해야 합니다.

```jsx harmony
import { Switch, Router, Route } from 'react-router'
```

<Switch> 블록안에 routes 를 정의해아합니다.

```jsx harmony
<Router>
  <Switch>
    <Route {/* ... */} />
    <Route {/* ... */} />
  </Switch>
</Router>
```

136. ### How to pass params to history.push method in React Router v4?
#### (React Router v4 에서 어떻게 history.push 메서드에 파라미터를 전달하나요?)

navigating 하는 동안 history object 에 props 를 전달할 수 있습니다.

```js
this.props.history.push({
  pathname: '/template',
  search: '?name=sudheer',
  state: { detail: response.data }
})
```

`search` 속성은 push() 메서드에 query params 을 전달하는데 사용됩니다.

137. ### How to implement default or NotFound page?
#### (기본 페이지 또는 NotFound 를 어떻게 구현하나요?)

`<Switch>` 는 일치하는 첫번째 자식 `<Route>` 를 렌더링합니다. 경로가 없는 `<Route>` 는 항상 일치합니다.
아래와 같이 경로를 속성을 제거해주면됩니다.

```jsx harmony
<Switch>
  <Route exact path="/" component={Home}/>
  <Route path="/user" component={User}/>
  <Route component={NotFound} />
</Switch>
```

138. ### How to get history on React Router v4?
#### (어떻게 React Router v4에서 history를 얻나요?)

- history object 를 내보내고 이 모듈을 전체 프로젝트에 가져오는 모듈을 만드세요.

예를 들면, history.js 파일을 만듭니다.

```js
import { createBrowserHistory } from 'history'

export default createBrowserHistory({
  /* pass a configuration object here if needed */
})
```

- 기본 구현된 routers 대신 `<Router>` component 를 사용해야합니다. index.js 에서 위의 history.js 를 가져왔습니다.

```jsx harmony
import { Router } from 'react-router-dom'
import history from './history'
import App from './App'

ReactDOM.render((
  <Router history={history}>
    <App />
  </Router>
), holder)
```

- 기본 구현된 history object 와 유사하 history object 의 push 메서드를 사용할 수 있습니다.

```jsx harmony
// some-other-file.js
import history from './history'

history.push('/go-here')
```

139. ### How to perform automatic redirect after login?
#### (어떻게 로그인후에 자동으로 redirect 를 시키나요?)

`react-router` 패키지는 React Router 에 `<Redirect>` component 를 제공합니다. `<Redirect>` 을 렌더링하면 새로운 위치로 이동합니다.
server-side redirect 와 같이, history stack 의 현재 위치를 새로운 위치로 덮어씌웁니다. 

```jsx harmony
import React, { Component } from 'react'
import { Redirect } from 'react-router'

export default class LoginComponent extends Component {
  render() {
    if (this.state.isLoggedIn === true) {
      return <Redirect to="/your/redirect/page" />
    } else {
      return <div>{'Login Please'}</div>
    }
  }
}
```