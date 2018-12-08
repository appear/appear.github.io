---
title: React Interview (리액트 면접 질문) [101 ~ 110]
date: 2018-11-21 10:00:31
tags: React
---

# React Interview

원작자인 [sudheerj](https://github.com/sudheerj) 의 동의를 구하고 진행하는 번역본 입니다 :)     

처음 해보는 번역이라 어색한 문맥이 많습니다 :) ..

-------------------------------------------------------------------
| No. | Questions |
|---- | ---------
||**Core ReactJS**| |
|101| [What is the difference between setState and replaceState methods?](#what-is-the-difference-between-setstate-and-replacestate-methods) |
|102| [How to listen to state changes?](#how-to-listen-to-state-changes) |
|103| [What is the recommended approach of removing an array element in react state?](#what-is-the-recommended-approach-of-removing-an-array-element-in-react-state) |
|104| [Is it possible to use React without rendering HTML?](#is-it-possible-to-use-react-without-rendering-html) |
|105| [How to pretty print JSON with React?](#how-to-pretty-print-json-with-react) |
|106| [Why you can't update props in React?](#why-you-cant-update-props-in-react) |
|107| [How to focus an input element on page load?](#how-to-focus-an-input-element-on-page-load) |
|108| [What are the possible ways of updating objects in state?](#what-are-the-possible-ways-of-updating-objects-in-state) |
|109| [Why function is preferred over object for setState?](#why-function-is-preferred-over-object-for-setstate) |
|110| [How can we find the version of React at runtime in the browser?](#how-can-we-find-the-version-of-react-at-runtime-in-the-browser) |


101. ### What is the difference between setState() and replaceState() methods?
#### (setState() 와 replaceState() 의 차이점은 무엇인가요?)
setState() 를 사용하면 이전 state 와 다음 state 가 병합됩니다. replaceState()는 현재 state 를 버리고 당신이 제공한 state 로 바꿉니다.
이전의 key 들을 모두 제거해야되는 경우가 아니라면 보통 setState() 가 사용됩니다.
replaceState()를 사용하는 대신 setState()에서 state 를 flase/null 로 설정할 수 있습니다.

102. ### How to listen to state changes?
#### (어떻게 state 의 변경을 listen 하나요?)
state 가 변경될때 다음의 lifecycle 메서드들이 호출됩니다.
이전에 제공된 state 와 props 의 값을 현재의 state와 props와 비교하여 의미있는 변화가 있는지 확인할 수 있습니다.

```js
componentWillUpdate(object nextProps, object nextState)
componentDidUpdate(object prevProps, object prevState)
```

103. ### What is the recommended approach of removing an array element in React state?
#### (React state 에서 배열요소를 제거하는 추천방법은 무엇인가요?)
좋은 방법은 Array.prototype.filter() 메서드를 사용하는 것입니다.

예를 들어, state update를 위한 removeItem () 메서드를 만들겠습니다.

```js
removeItem(index) {
  this.setState({
    data: this.state.data.filter((item, i) => i !== index)
  })
}
```

104. ### Is it possible to use React without rendering HTML?
#### (HTML 렌더링 없이 React를 사용할 수 있나요?)
latest version (최신버전 >= 16.2) 부터 가능합니다. 가능한 옵션은 다음과 같습니다.

```jsx harmony
render() {
  return false
}
```

```jsx harmony
render() {
  return null
}
```

```jsx harmony
render() {
  return []
}
```

```jsx harmony
render() {
  return <React.Fragment></React.Fragment>
}
```

```jsx harmony
render() {
  return <></>
}
```

105. ### How to pretty print JSON with React?
#### (React와 함께 어떻게 이쁘게 JSON 을 print 하나요?)
`<pre>` 태그를 사용하여 JSON.stringify() 형식이 유지되도록 할 수 있습니다.

```jsx harmony
const data = { name: 'John', age: 42 }

class User extends React.Component {
  render() {
    return (
      <pre>
        {JSON.stringify(data, null, 2)}
      </pre>
    )
  }
}

React.render(<User />, document.getElementById('container'))
```

106. ### Why you can't update props in React?
#### (왜 React에서 props 를 update 할 수 없나요?)
React 의 철학은 props 는 immutable(불변) 이어야하고 top-down (부모 -> 자식) 방식입니다.
부모는 모든 props 값을 자식에게 보낼 수 있지만, 자식은 받은 props 를 변경할 수 없습니다.

107. ### How to focus an input element on page load?
#### (어떻게 페이지 로드시에 input element 에 focus 하나요?)
input element 를 위한 ref 를 생성하여 componentDidMount() 에서 사용하면 됩니다.

```jsx harmony
class App extends React.Component{
  componentDidMount() {
    this.nameInput.focus()
  }

  render() {
    return (
      <div>
        <input
          defaultValue={'Won\'t focus'}
        />
        <input
          ref={(input) => this.nameInput = input}
          defaultValue={'Will focus'}
        />
      </div>
    )
  }
}

ReactDOM.render(<App />, document.getElementById('app'))
```

108. ### What are the possible ways of updating objects in state?
#### (state 의 object 를 update 할 수 있는 방법은 무엇인가요?)

1. state 와 합쳐질 object 를 사용하여 setState() 호출  
- Object.assign() 을 사용하여 object 의 복사본을 만듭니다.

```js
const user = Object.assign({}, this.state.user, { age: 42 })
this.setState({ user })
```

- spread operator 를 사용:

```js
const user = { ...this.state.user, age: 42 }
this.setState({ user })
```


2. function 을 이용하여 setState() 호출:

```js
this.setState(prevState => ({
  user: {
    ...prevState.user,
    age: 42
  }
}))
```

109. ### Why function is preferred over object for setState()?
#### (왜 setState()를 위한 function 이 object 보다 선호되나요?)
React 는 성능을 위해 여러 setState() 호출들을 일괄 처리 합니다.
이유는 this.props 와 this.state는 비동기로 update 될 수 있습니다. 다음 state 를 계산할 때 계산된 값을 신뢰하면 안됩니다.

카운터 예제는 기대한대로 update 되지 않습니다.

```js
// Wrong
this.setState({
  counter: this.state.counter + this.props.increment,
})
```

추천되는 접근방법은 object 가 아닌 function 으로 setState() 를 호출하는 것입니다.
function 은 첫번째 인자로 이전 state를 받고 두 번째 인자로 update가 적용될 때의 props 를 받습니다.

```js
// Correct
this.setState((prevState, props) => ({
  counter: prevState.counter + props.increment
}))
```

110. ### How can we find the version of React at runtime in the browser?
#### (브라우저에서 React의 버전을 runtime 시 어떻게 찾을 수 있나요?)

React.version 을 사용하면 버전을 가져올 수 있습니다.

```jsx harmony
const REACT_VERSION = React.version

ReactDOM.render(
  <div>{`React version: ${REACT_VERSION}`}</div>,
  document.getElementById('app')
)
```