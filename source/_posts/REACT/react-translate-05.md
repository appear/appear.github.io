---
title: React Interview (리액트 면접 질문) [41 ~ 50]
date: 2018-11-02 16:56:20
tags: React
---

# React Interview

원작자인 [sudheerj](https://github.com/sudheerj) 의 동의를 구하고 진행하는 번역본 입니다 :)     

처음 해보는 번역이라 어색한 문맥이 많습니다 :) ..

-------------------------------------------------------------------
| No. | Questions |
|---- | ---------
||**Core ReactJS**| |
|41 | [What is reconciliation?](#what-is-reconciliation) |
|42 | [How to set state with a dynamic key name?](#how-to-set-state-with-a-dynamic-key-name) |
|43 | [What would be the common mistake of function being called every time the component renders?](#what-would-be-the-common-mistake-of-function-being-called-every-time-the-component-renders) |
|44 | [Why is it necessary to capitalize component names?](#why-is-it-necessary-to-capitalize-component-names) |
|45 | [Why React uses className over class attribute?](#why-react-uses-classname-over-class-attribute) |
|46 | [What are fragments?](#what-are-fragments) |
|47 | [Why fragments are better than container divs?](#why-fragments-are-better-than-container-divs) |
|48 | [What are portals in React?](#what-are-portals-in-react) |
|49 | [What are stateless components?](#what-are-stateless-components) |
|50 | [What are stateful components?](#what-are-stateful-components) |


41. ### What is reconciliation?
#### ([reconciliation](https://reactjs.org/docs/reconciliation.html)란 무엇인가요?)
Component 의 props 또는 state가 변경될 때 React는 새로 반환된 component를 이전에 rendered 된 component와 비교하여 DOM update가 필요한지 결정합니다. 두개의 component가 같지 않을때 React는 DOM을 update합니다. 이를 프로세스 reconciliation 라고합니다.

42. ### How to set state with a dynamic key name?
#### (동적 key name 으로 어떻게 set State를 하나요?)
만약 ES6 나 babel transpiler 사용하여 JSX 코드를 변환하는 경우 computed property names 으로 작업을 할 수 있습니다. 

```js
handleInputChange(event) {
  this.setState({ [event.target.id]: event.target.value })
}
```

43. ### What would be the common mistake of function being called every time the component renders?
#### (render가 될 때마다 호출되는 function 의 일반적인 실수는 무엇인가요?)

function을 parameter로 전달하는 동안 function 이 호출되지 않았는지 확인해야합니다. 

```jsx harmony
render() {
  // Wrong: handleClick is called instead of passed as a reference!
  return <button onClick={this.handleClick()}>{'Click Me'}</button>
}
```

괄호 없이 function 을 전달하세요

```jsx harmony
render() {
  // Correct: handleClick is passed as a reference!
  return <button onClick={this.handleClick}>{'Click Me'}</button>
}
```

44. ### Why is it necessary to capitalize component names?
#### (왜 Component 의 이름은 대문자로 표기해야하나요?)

Component 들은 DOM element가 아니기 때문에 대문자 표기가 필요합니다. component들은 constructors입니다. 또한 JSX 에서의 소문자 태그 이름은 component가 아닌 HTML 요소를 나타내기 때문입니다.

45. ### Why React uses className over class attribute?
#### (왜 React 는 class를 사용하지 않고 className 속성을 사용하나요?)
class는 Javascript 의 키워드입니다. JSX는 Javascript 의 확장입니다. 그것이 React가 class 대신 className을 사용하는 이유입니다. classNames prop 로 문자열을 전달하세요

```jsx harmony
render() {
  return <span className={'menu navigation-menu'}>{'Menu'}</span>
}
```

46. ### What are fragments?
#### (fragments는 무엇인가요 ?)
fragments는 여러 Component 들을 반환하는데 사용되는 React의 일반적인 패턴입니다. fragments를 사용하면 DOM에 node 들을 추가하지 않고 하위 목록을 그룹화 할 수 있습니다. 

```jsx harmony
render() {
  return (
    <React.Fragment>
      <ChildA />
      <ChildB />
      <ChildC />
    </React.Fragment>
  )
}
```

더 짧은 구문도 있지만, 많은 tool 들에서 지원되지 않습니다.

```jsx harmony
render() {
  return (
    <>
      <ChildA />
      <ChildB />
      <ChildC />
    </>
  )
}
```

47. ### Why fragments are better than container divs?
#### (왜 fragments 가 div 보다 좋나요 ?)
1. Fragments 는 빠르고 DOM node를 만들지 않기 때문에 메모리를 적게 사용합니다. 이것은 매우 크고 깊은 트리에서 이익을 가집니다.  
2. Flexbox 나 CSS Grid과 같은 CSS 메커니즘에는 특별한 부모 - 자식 관계를 가지고 있습니다. 중간에 div 들을 추가하게 된다면 layout을 유지하기 어려워집니다. 
3. DOM Inspector은 덜 복잡합니다.

48. ### What are portals in React?
#### (React 에서 portals은 무엇입니까 ?)
portals 은 상위 Component 의 DOM 계층 구조 외부에 존재하는 DOM 노드로 자식을 render 하는데 권장되는 방법입니다. 

```js
ReactDOM.createPortal(child, container)
```

첫 번째 인자는 렌더링 가능한 React 하위요소 (element, string, fragment) 입니다. 두 번째 인자는 DOM element 입니다.

49. ### What are stateless components?
#### (stateless 컴포넌트는 무엇인가요?)
Component의 동작과 상태가 독립적이라면 stateless component 를 구성할 수 있습니다. stateless component 는 class나 function 을 이용하여 만들 수 있습니다. 
lifecycle hook을 사용해야하는 경우가 아니라면 function component를 사용할 수 있습니다. function component를 사용한다면 많은 이점이 있습니다. 
쓰기, 이해 그리고 테스트가 빠르고 쉽습니다. 

50. ### What are stateful components?
#### (stateful components 는 무엇인가요?)
Component 의 동작이 component의 state에 의존한다면 stateful component라고 부를 수 있습니다. stateful component 는 항상 class components 이고, constructor에서 초기화합니다. 

```jsx harmony
class App extends Component {
  constructor(props) {
    super(props)
    this.state = { count: 0 }
  }

  render() {
    // ...
  }
}
```

영어 원본: [Git Hub](https://github.com/sudheerj/reactjs-interview-questions#what-is-reactjs)    
블로그 번역: [Git Hub](https://github.com/appear/reactjs-interview-questions-ko)