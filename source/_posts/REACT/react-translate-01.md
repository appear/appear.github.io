---
title: React Interview (리액트 면접 질문) [1 ~ 10]
date: 2018-10-20 15:51:20
tags: [react]
categories: [react]
---

# React Interview (리액트 면접질문)

원작자인 [sudheerj](https://github.com/sudheerj) 의 동의를 구하고 진행하는 번역본 입니다 :)     

처음 해보는 번역이라 어색한 문맥이 많습니다 :) ..

-------------------------------------------------------------------
| No. | Questions |
|---- | ---------
||**Core ReactJS**| |
|1  | [What is ReactJS?](#what-is-reactjs) |
|2  | [What are the major features of ReactJS?](#what-are-the-major-features-of-reactjs)|
|3  | [What is JSX?](#what-is-jsx)|
|4  | [What is the difference between Element and Component?](#what-is-the-difference-between-element-and-component)|
|5  | [How to create components in ReactJS?](#how-to-create-components-in-reactjs)|
|6  | [When to use a Class Component over a Functional Component?](#when-to-use-a-class-component-over-a-functional-component)|
|7  | [What are Pure Components?](#what-are-pure-components)|
|8  | [What is state in ReactJS?](#what-is-state-in-reactjs)|
|9  | [What is props in ReactJS?](#what-is-props-in-reactjs)|
|10 | [What is the difference between state and props?](#what-is-the-difference-between-state-and-props)|

1. ### What is ReactJS? 
#### (React란 무엇인가요?)
[React](https://reactjs.org/docs/hello-world.html)는 SPA (Single Page Application) 즉, 단일 페이지 응용 프로그램에서 사용자 인터페이스를 구성는데 사용되는 오픈 소스 프론트엔드 JS 라이브러리 입니다.   
웹 및 모바일 앱의 Layer 를 다루는데 사용됩니다. 리액트는 페이스북의 소프트웨어 엔지니어 [Jordan Walke](https://twitter.com/jordwalke) 에 의해 만들어졌습니다.  
리액트는 2011 년 페이스북 뉴스피드에 발표되었고, 2012년 인스타그램에 처음 구축되었습니다.

2. ### What are the major features of ReactJS? 
#### (React의 특징은 무엇이 있을까요?)
React 의 주요 특징은 아래와 같습니다.
- RealDOM 을 조작하는데 많은 비용이 들어간다는 점을 고려하여 리액트는 RealDOM 대신 [VirtualDOM](https://www.youtube.com/watch?v=BYbgopx44vo)을 사용합니다.
- [서버 사이드 렌더링](https://subicura.com/2016/06/20/server-side-rendering-with-react.html)을 지원합니다.
- 단방향 데이터 흐름 또는 데이터 바인딩을 따릅니다.
- UI 구성 요소를 재사용할 수 있도록 개발할 수 있습니다.

3. ### What is JSX? 
#### (JSX란 무엇인가요?)
[JSX](https://reactjs.org/docs/introducing-jsx.html) 는 JS XML (ECMAScript로 XML 유사 구문 확장) 의 구문 표기법입니다. JSX 는 JS XML의 약자입니다.  
[HTML](https://ko.wikipedia.org/wiki/HTML)과 같은 문법과 함께 JS를 표현할 수 있습니다. 
예를 들면 아래의 h1 태그안에 text 는 render 함수에 의해 JS 함수로 반환됩니다. 

```jsx harmony
render() {
  return (
    <div>
      <h1> Welcome to React world!!</h1>
    </div>
  );
}
```

4. ### What is the difference between Element and Component? 
#### (element와 component의 차이점은 무엇인가요?)
[element](https://ko.wikipedia.org/wiki/HTML_%EC%9A%94%EC%86%8C)는 DOM 노드 또는 다른 [component](https://reactjs.org/docs/react-component.html)들과 관련하여 화면에 표시 할 내용을 표현하는 일반 객체입니다. elements는 다른 elements 들을 포함할 수 있습니다.
[React element](https://reactjs.org/docs/rendering-elements.html)를 만드는 비용은 저렴합니다. element 는 생성되면 변형되지 않습니다. 리액트 element 의 표현은 아래와 같습니다.

```jsx harmony
const element = React.createElement(
  'div',
  {id: 'login-btn'},
  'Login'
)
```

위에서 만들어진 element 는 아래와 같은 객체로 리턴됩니다.

```jsx harmony
{
  type: 'div',
  props: {
    children: 'Login',
    id: 'login-btn'
  }
}
```

마지막으로 아래와 같이 [**ReactDOM.render**](https://reactjs.org/docs/react-dom.html) 를 이용하여 DOM 으로 렌더링합니다.

```jsx harmony
<div id='login-btn'>Login</div>
```

반면 component 는 여러 방법으로 선언될 수 있습니다. render 메서드가 있는 class 일 수도 있습니다. 간단한 component 일 경우 function으로 정의 될 수 있습니다.
입력된 component 를 바탕으로 element 트리를 만듭니다. 마지막에 JSX 는 [createElement로 변환](https://reactjs.org/docs/jsx-in-depth.html)됩니다. 

```js
function Button ({ onLogin }) {
  return React.createElement(
    'div',
    {id: 'login-btn', onClick: onLogin},
    'Login'
  )
}
```

5. ### How to create components in ReactJS? 
#### (React에서 컴포넌트를 어떻게 생성하나요?)   
ReactJS 는 Components 생성하는 두 가지 방법이 있습니다.

1. **Functional components**
ReactJS 에서 component를 생성하는 가장 간단한 방법입니다. props 를 받을 수 있고 React elements 를 리턴할 수 있습니다. 
이런 방법을 pure 한 JS의 function 이기 때문에 functional한 생성법이라고 부릅니다.

```jsx harmony
function Greeting(props) {  
  return <h1> Hello, {props.message}</h1> 
}
```

2. **Class components**
ES6 [Class](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Classes)를 사용하여 component 를 정의할 수 있습니다. 위에서 본 functional 선언법을 아래와 같이 적용할 수 있습니다.

```jsx harmony
class Greeting extends React.Component {
  render() {
    return <h1>Hello, {this.props.message}</h1>;
  }
}
```

6. ### When to use a Class Component over a Functional Component? 
#### (언제 클래스 컴포넌트를 사용하고 언제 Functional 컴포넌트를 사용할까요?)
Component 에서 state 또는 [life cycle methods](https://reactjs.org/docs/react-component.html#the-component-lifecycle) 를 필요로 한다면 Class component를 사용하고 그렇지 않다면 Functional component를 사용할 수 있습니다.

7. ### What are Pure Components? 
####(순수 컴포넌트란 무엇인가요?)
[PureComponent](http://lucybain.com/blog/2018/react-js-pure-component)는 동일한 상태에서는 동일한 결과를 반환합니다. [shouldComponentUpdate](https://reactjs.org/docs/react-component.html#shouldcomponentupdate) 메서드를 다룰 수 있다는 점을 제외하고는 component와 동일합니다.   
props 또는 state 가 변경될 때 PureComponent 는 state 와 props 에 대해 [Shallow Compare](https://reactjs.org/docs/shallow-compare.html)을 수행합니다.  
반면 component는 현재 props와 변형될 state를 비교하지 않습니다. 그렇기 때문에 component는 shouldComponentUpdate가 호출 될 때 마다 다시 render 됩니다. (shouldComponentUpdate의 기본값은 true 이기 때문에)

8. ### What is state in ReactJS? 
#### (state란 무엇인가요?)
Component [State](https://reactjs.org/docs/faq-state.html) 는 component의 life cycle 동안 변경될 수 있는 정보를 담고 있는 객체입니다. 우리는 state를 가능한 단순하게 만들고 state 의 구성 요소의 수를 최소화하기 위해 노력해야 합니다.

message state를 가진 user component 를 생성해보겠습니다.

```jsx harmony
class User extends React.Component {
   constructor(props) {
      super(props);

      this.state = {
         message: "Welcome to React world",
      }
   }
   render() {
      return (
         <div>
            <h1>{this.state.message}</h1>
         </div>
      );
   }
}
```

![state](https://github.com/appear/reactjs-interview-questions-ko/raw/master/public/state.jpg)

9. ### What is props in ReactJS ? 
#### (props란 무엇인가요?)
[Props](https://reactjs.org/docs/components-and-props.html) 는 HTML 태그 속성과 유사한 규칙을 사용하여 React component 에 전달되는 값을 포함하는 단일 값 또는 객체입니다. 부모 component 에서 자식 component 로 전달되는 데이터 입니다.

React에서 props 의 목적은 아래와 같은 기능을 component에 제공하는 것 입니다.

1. 사용자 정의 데이터를 React component 로 전달할 수 있습니다.
2. State 변경을 Trigger 할 수 있습니다.
3. Component 의 render 메서드 안에서 this.props.reactProp 로 접근하여 사용할 수 있습니다.

예를 들어, reactProp를 받는 element 를 만들고 

```jsx harmony
<Element reactProp = "1" />
```

이 "reactProp" (또는 사용자가 찾은 정의한) props는 React를 사용하여 생성된 ccomponent에서 접근이 가능하고, React의 native props 영역에 포함된 속성이 됩니다.

```jsx harmony
props.reactProp;
```

10. ### What is the difference between state and props ? 
#### (state와 props의 차이점은 무엇인가요?)
props와 state는 모두 JavaScript 객체입니다. 두가지 다 렌더링 결과에 영향을 주는 정보를 가지고 있지만, component 와 관련된 기능면에서 차이가 있습니다.  
props는 함수 매개변수와 같이 component 요소로 전달됩니다. state 는 component 안에서 관리되고 사용할 변수 선언과 비슷합니다.


영어 원본: [Git Hub](https://github.com/sudheerj/reactjs-interview-questions#what-is-reactjs)    
블로그 번역: [Git Hub](https://github.com/appear/reactjs-interview-questions-ko)