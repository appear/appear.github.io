---
title: React Interview (리액트 면접 질문) [81 ~ 90]
date: 2018-11-15 10:00:31
tags: React
---

# React Interview

원작자인 [sudheerj](https://github.com/sudheerj) 의 동의를 구하고 진행하는 번역본 입니다 :)     

처음 해보는 번역이라 어색한 문맥이 많습니다 :) ..

-------------------------------------------------------------------
| No. | Questions |
|---- | ---------
||**Core ReactJS**| |
|81 | [What is a switching component?](#what-is-a-switching-component) |
|82 | [Why we need to pass a function to setState()?](#why-we-need-to-pass-a-function-to-setstate) |
|83 | [What is strict mode in React?](#what-is-strict-mode-in-react) |
|84 | [What are React Mixins?](#what-are-react-mixins) |
|85 | [Why is isMounted() an anti-pattern and what is the proper solution?](#why-is-ismounted-an-anti-pattern-and-what-is-the-proper-solution) |
|86 | [What are the Pointer Events supported in React?](#what-are-the-pointer-events-supported-in-react) |
|87 | [Why should component names start with capital letter?](#why-should-component-names-start-with-capital-letter) |
|88 | [Are custom DOM attributes supported in React v16?](#are-custom-dom-attributes-supported-in-react-v16) |
|89 | [What is the difference between constructor and getInitialState?](#what-is-the-difference-between-constructor-and-getinitialstate) |
|90 | [Can you force a component to re-render without calling setState?](#can-you-force-a-component-to-re-render-without-calling-setstate) |

81. ### What is a switching component?
#### (switching component 란 무엇인가요?)
switching component는 여러 component 중 하나의를 렌더링 하는 component 입니다. 우리는 props 값을 매핑하기 위해 object 를 사용해야합니다. 

예를 들어, switching component 는 페이지의 props 를 기반으로 다른 component 를 보여줍니다.

```js
import HomePage from './HomePage'
import AboutPage from './AboutPage'
import ServicesPage from './ServicesPage'
import ContactPage from './ContactPage'

const PAGES = {
  home: HomePage,
  about: AboutPage,
  services: ServicesPage,
  contact: ContactPage
}

const Page = (props) => {
  const Handler = PAGES[props.page] || ContactPage

  return <Handler {...props} />
}

// The keys of the PAGES object can be used in the prop types to catch dev-time errors.
Page.propTypes = {
  page: PropTypes.oneOf(Object.keys(PAGES)).isRequired
}
```

82. ### Why we need to pass a function to setState()?
#### (왜 setState 에 함수를 전달해야하나요?)
이유는 setState 가 비동기적인 작업이기 때문입니다. 성능상의 이유로 state 의 변경은 일괄적으로 일어납니다, 그래서 setState() 의 호출 직후에 state가 변경되지 않을 수 있습니다.   
state 를 확실할 수 없기 때문에 setState()를 호출 할 때 현재의 state에 의존하면 안됩니다.  
해결책은 이전 state를 인자로 사용하는 setState()에 함수를 전달하는 것입니다.    
그렇게 하면 setState()가 가진 비동기성으로 인해 사용자가 접근할때 이전 state의 값을 가져오는 문제를 피할 수 있습니다. 

초기의 count 값이 0 이라고 가정하겠습니다. 세번의 증가연산 후의 값은 오직 1만 증가합니다.

```js
// assuming this.state.count === 0
this.setState({ count: this.state.count + 1 })
this.setState({ count: this.state.count + 1 })
this.setState({ count: this.state.count + 1 })
// this.state.count === 1, not 3
```

setState() 에 함수를 전달하면 올바르게 증가합니다.

```js
this.setState((prevState, props) => ({
  count: prevState.count + props.increment
}))
// this.state.count === 3 as expected
```

83. ### What is strict mode in React?
#### (React 에서 strict mode 는 무엇인가요?)
React.StrictMode 는 application 의 잠재적인 문제를 강조 표시하는데 유용한 component 입니다. <Fragment>와 처럼 <StrictMode> 도 추가적으로 DOM을 렌더링하지 않습니다. 
자식에 대한 추가적인 확인과 경고를 활성화합니다. 검사는 개발 모드에만 적용됩니다.  

```jsx harmony
import React from 'react'

function ExampleApplication() {
  return (
    <div>
      <Header />
      <React.StrictMode>
        <div>
          <ComponentOne />
          <ComponentTwo />
        </div>
      </React.StrictMode>
      <Footer />
    </div>
  )
}
```

위의 예제에서 strict mode 는 <ComponentOne /> 와 <ComponentTwo /> 에만 적용됩니다.    
<StrictMode> 는 다음과 같은 도움을 줍니다. 

1. 안전하지 않은 lifecycle 메서드를 식별합니다.
2. 레거시 string ref API 에 대한 경고
3. 예상치 못한 사이드 이펙트 감지 
4. legacy context API 감지


84. ### What are React Mixins?
#### (React Mixins은 무엇인가요?)
Mixins은 공통적인 기능을 가질 수 있도록 component를 분리하는 방법입니다. Mixins은 사용하지 말아야합니다. higher-order components 또는 decorators 로 대체할 수 있습니다.   
props와 state가 이전의 props 와 state 가 얕게 같을때 불필요한 재 렌더링을 방지하기 위해 component 에서 사용할 수 있습니다.

```js
const PureRenderMixin = require('react-addons-pure-render-mixin')

const Button = React.createClass({
  mixins: [PureRenderMixin],
  // ...
})
```


85. ### Why is isMounted() an anti-pattern and what is the proper solution?
#### (왜 isMounted() 가 안티패턴이고 해결책은 무엇인가요?)
isMounted() 의 사용 사례는 component의 마운트가 해제 된 후에 setState()를 호출하지 않도록 하는 것입니다. component 마운트가 해제된 후 경고가 발생되기 때문입니다. 

```js
if (this.isMounted()) {
  this.setState({...})
}
```

setState() 를 호출하기 전에 isMounted() 를 검사한다면 경고를 제거할 수 있지만, 경고의 목적을 잃어버릴 수 있습니다.
component의 마운트가 해제된 후에 reference 를 가지고 있다고 생각하기 때문에 isMounted()를 사용하는 것은 [code smell](https://ko.wikipedia.org/wiki/%EC%BD%94%EB%93%9C_%EC%8A%A4%EB%A9%9C) 입니다.

최적의 해결법은 component의 마운트가 해제된 후 setState() 가 호출될 수 있는 위치를 찾아 수정하는 것입니다. 
이러한 상황은 component가 데이터를 기다리고 데이터가 도착하기전 마운트가 해제될 때, 콜백으로 인해 많이 발생됩니다.
콜백은 마운트가 해제되기전에 componentWillUnmount 단계에서 취소되어야합니다. 

86. ### What are the Pointer Events supported in React?
#### (React 에서 지원하는 Pointer Events 는 무엇인가요?)
Pointer Events 는 모든 입력 이벤트를 다루는 통일된 방법을 제공합니다. 옛날에는 마우스와 각각 event listeners 를 다뤘지만, 
요즘에는 터치스크린, 펜이 있는 휴대전화와 같이 마우스와 상관이 없는 많이 장치가 있습니다. 
이런 이벤트들은 Pointer Events 를 지원하는 브라우저에서만 동작하는 것을 기억해야합니다.

React DOM 에서는 다음과 같은 event 를 사용할 수 있습니다.

- onPointerDown
- onPointerMove
- onPointerUp
- onPointerCancel
- onGotPointerCapture
- onLostPointerCaptur
- onPointerEnter
- onPointerLeave
- onPointerOver
- onPointerOut

87. ### Why should component names start with capital letter?
#### (왜 Component 의 이름은 대문자로 시작해야하나요?)
만약 JSX 를 사용하여 component 를 렌더링하는 경우 component 의 이름은 대문자로 시작하여야 합니다. 
그렇지 않으면 React 는 알 수 없는 태그로 인식하여 오류가 발생될 것 입니다.
이 규칙은 HTML 과 SVG 태그만 소문자로 시작할 수 있기 때문에 사용됩니다.

이름이 소문자로 시작하는 component class 를 정의할 수 있지만 가져올떄는 대문자여야 합니다.    

여기 소문자는 괜찬습니다.

```jsx harmony
class myComponent extends Component {
  render() {
    return <div />
  }
}

export default myComponent
```

다른 파일에서 가져올때는 대문자로 시작해야합니다.

```jsx harmony
import MyComponent from './MyComponent'
```

88. ### Are custom DOM attributes supported in React v16?
#### (React v16 에서 사용자 정의 DOM 속성이 지원되나요?)
네 지원됩니다. 과거 React 는 알려지지 않은 DOM 속성은 무시되었었습니다. 
React 가 인식하지 못하는 속성을 가진 JSX 를 작성한다면 React 는 건너뛸 것 입니다. 

예를 들면 다음과 같습니다.

```jsx harmony
<div mycustomattribute={'something'} />
```

React v15 에서 빈 div 를 DOM 에 렌더링합니다.

```jsx harmony
<div />
```

React v16 에서 알 수 없는 속성은 DOM 에 포함됩니다. 

```jsx harmony
<div mycustomattribute='something' />
```

이 기능은 브라우저 별로 비표준 속성을 제공하고, 새로운 DOM API 를 시도, 다른 라이브러리와 같이 사용할 때 유용합니다.

89. ### What is the difference between constructor and getInitialState?
#### (constructor 와 getInitialState의 차이점은 무엇인가요?)
ES6 클래스를 사용할 떄는 constructor에서 state를 초기화할 수 있고, React.createClass 를 사용할 때는 getInitialState() 에서 상태를 초기화해야합니다.  
 
ES6 클래스 사용:

```jsx harmony
class MyComponent extends React.Component {
  constructor(props) {
    super(props)
    this.state = { /* initial state */ }
  }
}
```
React.createClass() 사용:

```jsx harmony
const MyComponent = React.createClass({
  getInitialState() {
    return { /* initial state */ }
  }
})
```

**Note**: React.createClass() 는 이제 사용되지 않고, React v16 에서 제거됩니다. 대신 JS class 를 사용하세요 

90. ### Can you force a component to re-render without calling setState?
#### (setState 를 호출하지 않고 component 를 강제로 재 렌더링 시킬 수 있나요?)
기본적으로, component 의 state 또는 props 가 변경되면 component 는 다시 렌더링됩니다. 
render() 메소드가 다른 data 에 의존성이 있는 경우, forceUpdate() 를 호출하여 component에게 다시 렌더링이 필요하다고 알려줄 수 있습니다. 

```js
component.forceUpdate(callback)
```

forceUpdate() 의 사용은 피하고 render() 안의 this.props 와 this.state 는 읽는 용도로 사용하는 것이 좋습니다.

