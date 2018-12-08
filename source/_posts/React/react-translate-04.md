---
title: React Interview (리액트 면접 질문) [31 ~ 40]
date: 2018-11-02 15:56:20
tags: React
---

# React Interview

원작자인 [sudheerj](https://github.com/sudheerj) 의 동의를 구하고 진행하는 번역본 입니다 :)     

처음 해보는 번역이라 어색한 문맥이 많습니다 :) ..

-------------------------------------------------------------------
| No. | Questions |
|---- | ---------
||**Core ReactJS**| |
|31 | [What is the difference between createElement and cloneElement?](#what-is-the-difference-between-createelement-and-cloneelement)|
|32 | [What is Lifting State Up in ReactJS?](#what-is-lifting-state-up-in-reactjs)|
|33 | [What are the different phases of ReactJS component lifecycle?](#what-are-the-different-phases-of-reactjs-component-lifecycle)|
|34 | [What are the lifecycle methods of ReactJS?](#what-are-the-lifecycle-methods-of-reactjs)|
|35 | [What are Higher-Order components?](#what-are-higher-order-components)|
|36 | [How to create props proxy for HOC component?](#how-to-create-props-proxy-for-hoc-component)|
|37 | [What is context?](#what-is-context)|
|38 | [What is children prop?](#what-is-children-prop)|
|39 | [How to write comments in ReactJS?](#how-to-write-comments-in-reactjs)|
|40 | [What is the purpose of using super constructor with props argument?](#what-is-the-purpose-of-using-super-constructor-with-props-argument)|

31. ### What is the difference between createElement and cloneElement?
#### (createElement와 cloneElement의 차이점은 무엇인가요?)
UI 의 object representation으로 사용될 react element를 만들기 위해 JSX element 는 React.createElement() 함수로 변환됩니다. cloneElement는 element 를 복사하고 새로운 props 를 전달하는데 사용됩니다.

32. ### What is Lifting State Up in React?
#### (React 에서 Lifting State Up 은 무엇인가요?)
여러 component 들이 동일한 변경 데이터를 공유해야하는 경우 가까운 부모 component 로 state를 올리는 것이 좋습니다. 즉, 두개의 자식 component가 부모의 있는 동일한 데이터를 공유할 때 두개의 자식 component 들은 부모로 state를 올리는 대신 local state를 유지해야합니다.

33. ### What are the different phases of component lifecycle?
#### (component 라이프사이클 단계는 무엇이 다른가요?)
라이프 사이클에는 4가지 단계가 있습니다.

1. Initialization: component 는 초기 state 및 props 를 세팅을 준비합니다.
2. Mounting: component 가 브라우저 DOM에 mount 될 준비가 되었습니다. 이 단계에서는 componentWillMount, componentDidMount 를 사용할 수 있습니다.
3. Updating: 새로운 props를 보내거나 state를 업데이트하여 component 를 두가지 방법으로 update 할 수 있습니다. 이 단계에서는 shouldComponentUpdate (), componentWillUpdate () 및 componentDidUpdate 를 사용할 수 있습니다.
4. Unmounting: 브라우저 DOM에서 component 가 필요하지 않을때 mount를 해제시킵니다. 이 단계에서는 componentWillUnmount 를 사용할 수 있습니다.

![lifecycle](https://raw.githubusercontent.com/appear/appear.github.io/master/img/phases.png)

34. ### What are the lifecycle methods of React?
#### (React의 lifecycle methods는 무엇인가요?)
- componentWillMount: rendering 전에 실행됩니다.
- componentDidMount: 첫 rendering 후에 실행됩니다. 이 단계에서 모든 AJAX 요청, DOM 또는 State의 update, event listener 가 설정되어야합니다.
- componentWillReceiveProps: 특정 props update가 state 변화를 트리거할 때 실행됩니다.
- shouldComponentUpdate: 컴포넌트의 업데이트 여부를 결정합니다. 기본적으로 return 값은 true 입니다. state 또는 props 가 업데이트 된 후 component 를 render 할 필요가 없을 경우 false를 return 할 수 있습니다. component가 새로운 props를 받으면서 생기는 rerender 을 막을 수 있습니다. 성능을 향상시키는데 좋습니다.
- componentWillUpdate: shouldComponentUpdate에서 true가 반환되었을 때 state & props 의 변화를 확인하고 re-rendering 되기전에 실행합니다.
- componentDidUpdate: 주로 props 나 state 변경에 대한 response 로 DOM을 업데이트할 때 사용됩니다.
- componentWillUnmount: 네트워크 요청을 취소하거나, component와 관련된 모든 event listeners 를 제거하는데 사용됩니다.

35. ### What are Higher-Order components?
#### (Higher-Order components 는 무엇인가요?)
Higher-Order component(HOC)는 component를 받아서 새로운 component를 return 하는 함수입니다. HOC는 React의 특성에서 파생된 패턴입니다. 
동적으로 제공되는 하위 component들을 그대로 사용하지만 입력받은 component를 수정하거나 복사하지 않기 때문에 "순수 components" 라고 부릅니다.

```js
const EnhancedComponent = higherOrderComponent(WrappedComponent);
```

HOC는 아래와 같은 많은 use cases 에 사용할 수 있습니다.

- Code reuse, logic and bootstrap abstraction
- Render High jacking
- State abstraction and manipulation
- Props manipulation

36. ### How to create props proxy for HOC component?
#### (HOC component 위한 props proxy 만드는법?)
다음과 같이 component에 전달된 props를 props proxy로 추가 / 편집 할 수 있습니다. 

```jsx harmony
function HOC(WrappedComponent) {
  return class Test extends Component {
    render() {
      const newProps = {
        title: 'New Header',
        footer: false,
        showFeatureX: false,
        showFeatureY: true
      };

      return <WrappedComponent {...this.props} {...newProps} />
    }
  }
}
```

37. ### What is context?
#### (context가 뭔가요?)
Context 는 모든 레벨에 수동으로 props를 전달하지 않고 component tree를 통해 데이터를 전달할 수 있도록 제공해줍니다.
예를 들어, 인증된 유저, locale 설정, UI 테마는 많은 application 들에서 접근해야합니다.  

```jsx harmony
const {Provider, Consumer} = React.createContext(defaultValue);
```

38. ### What is children prop?
#### (children prop란 무엇인가요?)
Children prop는 component를 data로 다른 component로 전달할 수 있도록 해주는 prop(this.prop.children)입니다.  
당신이 사용하는 prop 처럼 사용할 수 있습니다. React API에는 이 props와 함께 사용할 수 있는  React.Children.map, React.Children.forEach, React.Children.count, React.Children.only, React.Children.toArray등 여러가지 방법이 있습니다.

children prop의 간단한 사용법은 아래와 같습니다.

```jsx harmony
var MyDiv = React.createClass({
  render: function() {
    return <div>{this.props.children}</div>;
  }
});

ReactDOM.render(
  <MyDiv>
    <span>Hello</span>
    <span>World</span>
  </MyDiv>,
  node
);
```

39. ### How to write comments in ReactJS?
#### (React에서 주석은 어떻게 쓰나요?)
ReactJS / JSX 에서의 주석은 JavaScript와 유사합니다. 한 줄 및 여러 줄 주석들은 모두 중괄호로 감쌉니다. 

#### Single-line comments:

```jsx harmony
<div>
  {/* Single-line comments */}    // In vanilla JavaScript, the single-line comments are represented by double slash(//)
  Welcome {user}, Let's play React
</div>
```

#### Multi-line comments:

```jsx harmony
<div>
  {/* Multi-line comments for more than
   one line */}
  Welcome {user}, Let's play React
</div>
```

40. ### What is the purpose of using super constructor with props argument?
#### (props argument와 함께 super constructor를 사용하는 목적은 무엇인가요?)
자식 클래스의 constructor 는 super() 메서드가 호출되기 전까지 this 참조 사용할 수 없습니다. es6 의 sub-classes 에서도 동일하게 적용됩니다.
super()에 props를 파라미터로 전달하는 이유는 child constructors에서 this.props로 접근하기 위해서입니다. 

#### Passing props:

```jsx harmony
class MyComponent extends React.Component {
    constructor(props) {
        super(props);

        console.log(this.props);  // Prints { name: 'sudheer',age: 30 }
    }
}
```

#### Not passing props:

```jsx harmony
class MyComponent extends React.Component {
    constructor(props) {
        super();

        console.log(this.props); // Prints undefined

        // But Props parameter is still available
        console.log(props); // Prints { name: 'sudheer',age: 30 }
    }

    render() {
        // No difference outside constructor
        console.log(this.props) // Prints { name: 'sudheer',age: 30 }
    }
}
```

위의 code snippets은 this.props의 동작이 constructor 만 다른 것을 보여줍니다. 생성자 밖에서의 동작은 동일합니다. 

영어 원본: [Git Hub](https://github.com/sudheerj/reactjs-interview-questions#what-is-reactjs)    
블로그 번역: [Git Hub](https://github.com/appear/reactjs-interview-questions-ko)