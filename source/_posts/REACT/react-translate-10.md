---
title: React Interview (리액트 면접 질문) [91 ~ 100]
date: 2018-11-20 10:00:31
tags: React
---

# React Interview

원작자인 [sudheerj](https://github.com/sudheerj) 의 동의를 구하고 진행하는 번역본 입니다 :)     

처음 해보는 번역이라 어색한 문맥이 많습니다 :) ..

-------------------------------------------------------------------
| No. | Questions |
|---- | ---------
||**Core ReactJS**| |
|91 | [What is the difference between super() and super(props) in React using ES6 classes?](#what-is-the-difference-between-super-and-superprops-in-react-using-es6-classes) |
|92 | [How to loop inside JSX?](#how-to-loop-inside-jsx) |
|93 | [How do you access props in attribute quotes?](#how-do-you-access-props-in-attribute-quotes) |
|94 | [What is React PropType array with shape?](#what-is-react-proptype-array-with-shape) |
|95 | [How conditionally apply class attributes?](#how-conditionally-apply-class-attributes) |
|96 | [What is the difference between React and ReactDOM?](#what-is-the-difference-between-react-and-reactdom) |
|97 | [Why ReactDOM is separated from React?](#why-reactdom-is-separated-from-react) |
|98 | [How to use React label element?](#how-to-use-react-label-element) |
|99 | [How to combine multiple inline style objects?](#how-to-combine-multiple-inline-style-objects) |
|100| [How to re-render the view when the browser is resized?](#how-to-re-render-the-view-when-the-browser-is-resized)


91. ### What is the difference between super() and super(props) in React using ES6 classes?
#### ES6 class 를 사용하는 React 에서 super() 와 super(props) 의 차이점은 무엇인가요?
constructor() 에서 this.props 에 접근하고 싶을때 super() 메서드에 props 를 전달해야합니다.

super(props)를 사용:

```js
class MyComponent extends React.Component {
  constructor(props) {
    super(props)
    console.log(this.props) // { name: 'John', ... }
  }
}
```

super() 를 사용:

```js
class MyComponent extends React.Component {
  constructor(props) {
    super()
    console.log(this.props) // undefined
  }
}
```

constructor() 의 밖에서의 this.props 에는 같은 값이 표시됩니다.

92. ### How to loop inside JSX?
#### 어떻게 JSX 안에서 loop 를 하나요?
ES6 arrow function 과 함께 Array.prototype.map 를 사용할 수 있습니다. 예를 들어, 객체의 items 배열은 component 의 배열로 맵핑됩니다.

```js
<tbody>
  {items.map(item => <SomeComponent key={item.id} name={item.name} />)}
</tbody>
```

for loop 를 사용하여 반복할 수 없습니다.

```js
<tbody>
  for (let i = 0; i < items.length; i++) {
    <SomeComponent key={items[i].id} name={items[i].name} />
  }
</tbody>
```

JSX 태그는 함수 호출로 변환되어지기 떄문에 표현식 안에서 JS 표현법을 사용할 수 없습니다.
이것은 stage 1 에 제안되어 변경되어질 수 있습니다.

93. ### How do you access props in attribute quotes?
#### (attribute 따옴표에서 props 에 어떻게 접근하나요?)
React (또는 JSX) 는 속성 값 안에 써넣은 값은 지원되지 않습니다. 아래의 표현식은 동작하지 않습니다. 

```jsx harmony
<img className='image' src='images/{this.props.image}' />
```

그러나 중괄호 안에 JS 표현식을 감싸 속성 값으로 넣을 수 있습니다. 아래의 표현식은 동작합니다.

```jsx harmony
<img className='image' src={'images/' + this.props.image} />
```

템플릿 문자열을 사용해도 동작합니다.

```jsx harmony
<img className='image' src={`images/${this.props.image}`} />
```

94. ### What is React proptype array with shape?
특정 모양을 가진 component 에 객체 배열을 전달하고 싶다면 React.PropTypes.arrayOf()의 인자로 React.PropTypes.shape() 를 사용하세요

```js
ReactComponent.propTypes = {
  arrayWithShape: React.PropTypes.arrayOf(React.PropTypes.shape({
    color: React.PropTypes.string.isRequired,
    fontSize: React.PropTypes.number.isRequired
  })).isRequired
}
```

95. ### How conditionally apply class attributes?
#### (어떻게 조건에 따라 class 속성을 적용하나요?)
따옴표안에 중괄호를 사용하면 string 으로 인식되기 때문에 중괄호를 사용하면 안됩니다.

```jsx harmony
<div className="btn-panel {this.props.visible ? 'show' : 'hidden'}">
```

중괄호 밖으로 옮겨야 합니다. (class 의 이름 사이에 공백을 포함하는 것을 잊으면 안됩니다.)

```jsx harmony
<div className={'btn-panel ' + (this.props.visible ? 'show' : 'hidden')}>
```

템플릿 문자열을 사용해도 동작합니다.

```jsx harmony
<div className={`btn-panel ${this.props.visible ? 'show' : 'hidden'}`}>
```

96. ### What is the difference between React and ReactDOM?
#### (React와 ReactDOM은 무엇이 다른가요?)
React package 에는 React.createElement (), React.Component, React.Children 및 element 와 component class 관련된 helper 들이 포함되어 있습니다.
이런 기능들은 component 를 구축하는데 필요한 helpers 로 생각할 수 있습니다.
react-dom package 에는 ReactDOM.render () 가 포함되어있고, react-dom/server 에는 ReactDOMServer.renderToString() 과 ReactDOMServer.renderToStaticMarkup() 포함되어있어 서버사이드 렌더링을 지원합니다.

97. ### Why ReactDOM is separated from React?
#### (왜 React 와 ReactDOM은 분리되어있나요?)
React 팀은 모든 DOM 과 관련된 기능들을 ReactDOM 이라는 라이브러리로 분리하였습니다. 
React v0.14 는 ReactDOM 이 분리된 첫번째 릴리즈입니다.
React-native, Reaction-Art, Reaction-Canvas, React-3 등 패키지의 일부를 보면 React 의 아름다움과 본질적인 부분이 브라우저나 DOM 과 아무런 관계가 없다는 것을 알 수 있습니다.
React 가 렌더링할 수 있는 많은 환경들을 만들기 위해, React 팀은 React 와 React-dom 을 분리할 계획을 세웠습니다.
이것은 웹 버전의 React 와 React native 사이의 공유할 수 있는 component 를 작성하는 방법을 개척합니다.

98. ### How to use React label element?
#### (React 에서 label element 를 어떻게 사용하나요?)
표준 `for` 속성을 사용하는 text input 에 바인드된 label element 를 렌더링하려고하면 속성이없는 HTML이 생성되고 console 에 경고가 출력됩니다.

```jsx harmony
<label for={'user'}>{'User'}</label>
<input type={'text'} id={'user'} />
```

for 는 JS 의 예약된 키워드입니다, 대신 htmlFor 를 사용하세요 

```jsx harmony
<label htmlFor={'user'}>{'User'}</label>
<input type={'text'} id={'user'} />
```

99. ### How to combine multiple inline style objects?
#### (어떻게 여러개의 inline style object 를 합치나요?)
React 에서 spread operator 를 사용할 수 있습니다.

```jsx harmony
<button style={{...styles.panel.button, ...styles.panel.submitButton}}>{'Submit'}</button>
```

React Native 를 사용하고 있다면 배열 표기법을 사용할 수 있습니다.

```jsx harmony
<button style={[styles.panel.button, styles.panel.submitButton]}>{'Submit'}</button>
```

100. ### How to re-render the view when the browser is resized?
#### (어떻게 브라우저가 resize 될 때 재 렌더링 시키나요?)
componentDidMount() 에서 resize 이벤트를 listen 할 수 있고, 크기(width, height)를 update 할 수 있습니다. 
componentWillUnmount() 에서 listener 를 제거해줘야합니다.
 
```js
class WindowDimensions extends React.Component {
  componentWillMount() {
    this.updateDimensions()
  }

  componentDidMount() {
    window.addEventListener('resize', this.updateDimensions)
  }

  componentWillUnmount() {
    window.removeEventListener('resize', this.updateDimensions)
  }

  updateDimensions() {
    this.setState({width: $(window).width(), height: $(window).height()})
  }

  render() {
    return <span>{this.state.width} x {this.state.height}</span>
  }
}
```