---
title: React Interview (리액트 면접 질문) [61 ~ 70]
date: 2018-11-09 16:56:20
tags: React
---

# React Interview

원작자인 [sudheerj](https://github.com/sudheerj) 의 동의를 구하고 진행하는 번역본 입니다 :)     

처음 해보는 번역이라 어색한 문맥이 많습니다 :) ..

-------------------------------------------------------------------
| No. | Questions |
|---- | ---------
||**Core ReactJS**| |
|61 | [How to use styles in React?](#how-to-use-styles-in-react) |
|62 | [How events are different in React?](#how-events-are-different-in-react) |
|63 | [What will happen if you use setState in constructor?](#what-will-happen-if-you-use-setstate-in-constructor) |
|64 | [What is the impact of indexes as keys?](#what-is-the-impact-of-indexes-as-keys) |
|65 | [Is it good to use setState() in componentWillMount() method?](#is-it-good-to-use-setstate-in-componentwillmount-method) |
|66 | [What will happen if you use props in initial state?](#what-will-happen-if-you-use-props-in-initial-state) |
|67 | [How do you conditionally render components?](#how-do-you-conditionally-render-components)
|68 | [Why we need to be careful when spreading props on DOM elements??](#why-we-need-to-be-careful-when-spreading-props-on-dom-elements) |
|69 | [How you use decorators in React?](#how-you-use-decorators-in-react) |
|70 | [How do you memoize a component?](#how-do-you-memoize-a-component) |




61. ### How to use styles in React?
#### (React 스타일을 어떻게 사용하나요?)
style 속성은 CSS 문자열 대신 camelCased 속성의 Javascript 객체를 허용합니다. 이것은 DOM Style Javascript 속성과 일치하며 효율적이고, XSS 보안의 허점을 방지할 수 있습니다.

```jsx harmony
const divStyle = {
  color: 'blue',
  backgroundImage: 'url(' + imgUrl + ')'
};

function HelloWorldComponent() {
  return <div style={divStyle}>Hello World!</div>
}
```

Style의 key는 Javascript DOM Node (예 : node.style.backgroundImage)의 속성에 접근하는 것과 일관되게 하기 위해 camelCased 를 사용합니다. 
 
62. ### How events are different in React?
#### (React 에서 이벤트는 어떻게 다른가요?)
React element 에서의 이벤트 처리는 몇가지 문법상 차이점이 있습니다.

- React 이벤트 핸들러는 소문자보다는 camelCase 를 사용합니다.
- JSX 에서는 문자열이 아닌 이벤트 핸들러를 통해 function 을 전달합니다.

63. ### What will happen if you use setState() in constructor?
#### (constructor 에서 setState를 사용하면 무슨일이 생기나요? )
setState() 를 사용할 때 객체 상태가 할당되고 또한 component 와 모든 자식들을 재렌더링 합니다. 
이런 에러를 봤을겁니다. `Can only update a mounted or mounting component`. 그래서 우리는 this.state 를 사용하여 constructor 내부의 변수를 초기화 해야합니다.

64. ### What is the impact of indexes as keys?
#### (index 가 key 로 미치는 영향은 무엇인가요?)
React element 가 추적할 수 있도록 key 는 안정적, 예측가능, 고유 하여야합니다.
아래 코드 조각에서의 각 element 의 key 는 데이터를 따르지 않고 순서에 따라 결정됩니다. 
이것은 React 가 할 수 있는 최적화를 제한합니다.

```jsx harmony
{todos.map((todo, index) =>
  <Todo
    {...todo}
    key={index}
  />
)}
```

만약 element 에 data 의 고유한 키값을 사용한다면, todo.id 가 목록에서 고유하고, 안정적이라고 가정한다면 React 는 elements 를 재평가 할 필요없이 element 를 재정렬 할 수 있습니다. 

```jsx harmony
{todos.map((todo) =>
  <Todo {...todo}
    key={todo.id} />
)}
```

65. ### Is it good to use setState() in componentWillMount() method?
#### (componentWillMount 에서 setState를 사용하는게 좋나요 ?)
`componentWillMount` 안에서는 비동기적인 초기화는 피하는 것이 좋습니다. 
`componentWillMount` 는 마운트가 실행되기 직전에 호출됩니다. 
render () 전에 호출되기 때문에 상태를 설정하더라도 re-render 되지 않습니다.
`componentWillMount` 에서는 side-effects or subscriptions 을 피해야합니다.
우리는 `componentWillMount` 대신 `componentDidMount` 에서 component 초기화를 위한 비동기 호출을 실행해야 합니다.

```js
componentDidMount() {
  axios.get(`api/todos`)
    .then((result) => {
      this.setState({
        messages: [...result.data]
      })
    })
}
```

66. ### What will happen if you use props in initial state?
#### (state 의 초기상태에서 props 를 사용하면 어떻게되나요?)
만약 component 를 새로 고치지 않고 component 의 props 를 변경한다면 생성자 함수가 component 의 현재 상태를 업데이트 하지 않습니다. 
새로운 props 의 값이 표시되지 않습니다. state 의 초기화는 component 를 처음 만들 때만 실행됩니다.

 아래의 component 는 업데이트 된 input 의 값을 표시하지 않습니다.
 
 ```jsx harmony
class MyComponent extends React.Component {
  constructor(props) {
    super(props)

    this.state = {
      records: [],
      inputValue: this.props.inputValue
    };
  }

  render() {
    return <div>{this.state.inputValue}</div>
  }
}
```

render 메소드 안에서 props 를 사용하면 값이 업데이트 됩니다.

```jsx harmony
class MyComponent extends React.Component {
  constructor(props) {
    super(props)

    this.state = {
      record: []
    }
  }

  render() {
    return <div>{this.props.inputValue}</div>
  }
}
```

67. ### How do you conditionally render components?
#### (조건에 따른 component 렌더링은 어떻게 하나요 ?)
경우에 따라서 state 에 의존하여 다르게 component 를 렌더링 하기를 원할것이다.
JSX 는 false 또는 undefined 때는 렌더링하지 않으므로 조건을 사용하여 특정 조건이 true 인 경우에만 
component의 특정 부분을 렌더링 할 수 있습니다.

```jsx harmony
const MyComponent = ({ name, address }) => (
  <div>
    <h2>{name}</h2>
    {address &&
      <p>{address}</p>
    }
  </div>
)
```

만약 if-else 를 사용하기를 원한다면 삼항연산자를 쓰세요 

```jsx harmony
const MyComponent = ({ name, address }) => (
  <div>
    <h2>{name}</h2>
    {address
      ? <p>{address}</p>
      : <p>{'Address is not available'}</p>
    }
  </div>
)
```

68. ### Why we need to be careful when spreading props on DOM elements?
#### (DOM element 에 props 를 spreading  할때 왜 조심해야 하나요?)
props 를 spreading 할 때 알려지지않은 HTML 속성을 추가 할 위험이 있습니다. 그것은 좋지않습니다.
대신 ...rest 연산자를 사용하여 prop destructuring 을 할 수 있으므로 필요한 props 만 추가할 수 있습니다.

 ```jsx harmony
const ComponentA = () =>
  <ComponentB isDisplay={true} className={'componentStyle'} />

const ComponentB = ({ isDisplay, ...domProps }) =>
  <div {...domProps}>{'ComponentB'}</div>
```
69. ### How you use decorators in React?
#### (React 에서 decorators 는 어떻게 사용하나요?)
Class component 를 하나의 function 으로 변환하는 방식으로 decorate 할 수 있습니다.
Decorators 는 component 의 기능을 수정하기 위한 유연하고 읽기 쉬운 방법입니다.

```jsx harmony
@setTitle('Profile')
class Profile extends React.Component {
    //....
}

const setTitle = (title) => (WrappedComponent) => {
  return class extends React.Component {
    componentDidMount() {
      document.title = title
    }

    render() {
      return <WrappedComponent {...this.props} />
    }
  }
}
```

NOTE: Decorators 는 ES7 에 포함되지 않았습니다. 현재 stage 2 에 제안되어 있습니다.

70. ### How do you memoize a component?
#### (memoize component 는 어떻게 사용하나요?)
function component 에 사용할 수 있는 memoize 라이브러리가 있습니다.
예를 들면 `moize` 라이브러리는 다른 component 에 component 를 memoize 할 수 있습니다.

```jsx harmony
import moize from 'moize'
import Component from './components/Component' // this module exports a non-memoized component

const MemoizedFoo = moize.react(Component)

const Consumer = () => {
  <div>
    {'I will memoize the following entry:'}
    <MemoizedFoo/>
  </div>
}
```


영어 원본: [Git Hub](https://github.com/sudheerj/reactjs-interview-questions#what-is-reactjs)    
블로그 번역: [Git Hub](https://github.com/appear/reactjs-interview-questions-ko)