---
title: React Interview (리액트 면접 질문) [51 ~ 60]
date: 2018-11-03 16:56:20
tags: React
---

# React Interview

원작자인 [sudheerj](https://github.com/sudheerj) 의 동의를 구하고 진행하는 번역본 입니다 :)     

처음 해보는 번역이라 어색한 문맥이 많습니다 :) ..

-------------------------------------------------------------------
| No. | Questions |
|---- | ---------
||**Core ReactJS**| |
|51 | [How to apply validation on props in React?](#how-to-apply-validation-on-props-in-react) |
|52 | [What are the advantages of React?](#what-are-the-advantages-of-react) |
|53 | [What are the limitations of React?](#what-are-the-limitations-of-react) |
|54 | [What are error boundaries in React v16](#what-are-error-boundaries-in-react-v16) |
|55 | [How error boundaries handled in React v15?](#how-error-boundaries-handled-in-react-v15) |
|56 | [What are the recommended ways for static type checking?](#what-are-the-recommended-ways-for-static-type-checking) |
|57 | [What is the use of react-dom package?](#what-is-the-use-of-react-dom-package) |
|58 | [What is the purpose of render method of react-dom?](#what-is-the-purpose-of-render-method-of-react-dom) |
|59 | [What is ReactDOMServer?](#what-is-reactdomserver) |
|60 | [How to use InnerHtml in React?](#how-to-use-innerhtml-in-react) |


51. ### How to apply validation on props in React?
#### (React에서 Props의 유효성검사를 적용하나요 ?)
Application 이 개발 모드에서 실행될 때 React는 자동으로 component에 설정된 모든 props 들의 유효성을 체크합니다. 타입이 올바르지 않다면 React는 console에 경고 메세지를 생성할 것 입니다. 
성능에 영향이 가기 때문에 production 모드에서는 사용하지 않습니다. 필수 props는 isRequired로 정의됩니다.

정의 된 props 타입

- PropTypes.number
- PropTypes.string
- PropTypes.array
- PropTypes.object
- PropTypes.func
- PropTypes.node
- PropTypes.element
- PropTypes.bool
- PropTypes.symbol
- PropTypes.any

User component 에 대한 PropTypes 를 아래와 같이 정의할 수 있습니다.

```jsx harmony
import React from 'react'
import PropTypes from 'prop-types'

class User extends React.Component {
  static propTypes = {
    name: PropTypes.string.isRequired,
    age: PropTypes.number.isRequired
  }

  render() {
    return (
      <>
        <h1>{`Welcome, ${this.props.name}`}</h1>
        <h2>{`Age, ${this.props.age}`}</h2>
      </>
    )
  }
}
```

Note: React v15.5 에서 PropType은 React.PropTypes 에서 prop-types 라이브러리로 옮겨졌습니다.

52. ### What are the advantages of React?
#### (React의 장점은 무엇인가요?)
1. 가상 DOM을 이용하여 application 의 성능을 향상시킵니다.
2. JSX는 코드를 읽고 쓰는 것을 쉽게 만들어줍니다.
3. 클라이언트와 서버측 (SSR) 모두 render 됩니다.
4. 오직 view library 이기 때문에 다른 프레임워크들 (Angular, Backbone)과 손쉽게 통합 할 수 있습니다.
5. Jest 와 같은 도구를 사용하여 단위 및 통합테스트를 쉽게 작성할 수 있습니다.

53. ### What are the limitations of React?
#### (React의 한계는 무엇인가요?)
1. React 전체 framework 가 아닌 view를 위한 library 일 뿐입니다.
2. 웹 개발을 처음 접하는 초심자들에게는 학습 곡선이 있습니다.
3. 기존의 MVC framework 에 React 를 합치기 위해서는 몇 가지 추가 구성이 필요합니다.
4. 인라인 템플릿 및 JSX를 사용하면 코드가 복잡해집니다.
5. 과도한 엔지니어링 또는 boilerplate 로 이어지는 작은 components 들이 너무 많습니다.

54. ### What are error boundaries in React v16?
#### (React v16 에서 error boundaries는 무엇인가요?)
Error boundaries 는 하위 component tree 에서 Javascript error 를 catch 하고, 에러를 기록하고, 오류가 발생한 component tree가 아닌 대체 UI를 표현해 주는 component 입니다.

class component 는 componentDidCatch 라는 새로운 lifecycle 메서드를 정의하면 Error boundaries 됩니다

```jsx harmony
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props)
    this.state = { hasError: false }
  }

  componentDidCatch(error, info) {
    // Display fallback UI
    this.setState({ hasError: true })
    // You can also log the error to an error reporting service
    logErrorToMyService(error, info)
  }

  render() {
    if (this.state.hasError) {
      // You can render any custom fallback UI
      return <h1>{'Something went wrong.'}</h1>
    }
    return this.props.children
  }
}
```

그 다음 일반 component 로 사용합니다.

```jsx harmony
<ErrorBoundary>
  <MyWidget />
</ErrorBoundary>
```

55. ### How error boundaries handled in React v15?
#### (React v15 에서는 어떻게 error boundaries 처리하나요?)
React v15 에서는 unstable_handleError 메서드를 사용하여 error boundaries 에 대한 기본적인 지원을 했습니다. React v16에서 componentDidCatch로 이름이 변경되었습니다.

56. ### What are the recommended ways for static type checking?
#### (권장되는 static 한 타입 체크방법은 무엇인가요?)
일반적으로 우리는 React application 에서 타입 검사를 하기 위해 PropTypes library (React.PropTypes를 React v15.5 부터는 prop-types package 로 옮겼습니다)를 사용합니다.
대규모의 코드 베이스에서는 compile time 에서 타입 체크를 하고 자동 완성 기능을 제공해주는 Flow 나 TypeScript 와 같은 static type checkers 를 이용하는 것이 좋습니다. 

57. ### What is the use of react-dom package?
#### (react-dom 패키지의 사용법은 무엇인가요?)
React-dom 패키지는 App의 최상위 레벨에서 사용할 수 있는 DOM-specific 메서드를 제공합니다. 대부분의 Component 들은 이 모듈을 사용할 필요가 없습니다.
패키지의 일부 메서드는 다음과 같습니다. 

- render()
- hydrate()
- unmountComponentAtNode()
- findDOMNode()
- createPortal()

58. ### What is the purpose of render method of react-dom?
#### (React dom의 render 메서드의 목적은 무엇인가요?)

render 메서드는 제공된 컨테이너의 DOM에 React element를 render 하고 Component에 대한 참조를 반환하는데 사용됩니다. 
React element가 이전에 렌더링 되었다면 update 를 수행하고 최근의 변경사항을 반영하기 위해 필요에 따라 DOM을 변경합니다.

```jsx harmony
ReactDOM.render(element, container[, callback])
```

만약 optional callback 이 제공된다면, callback은 component 가 렌더링되거나 업데이트 된 후에 실행됩니다.

59. ### What is ReactDOMServer?
#### (ReactDOMServer 란 무엇인가요?)
ReactDOMServer 객체를 사용하면 component를 static markup(일반적으로 노드 서버에서 사용됩니다.) 으로 렌더링 할 수 있습니다.
이 객체는 주로 서버사이드 렌더링(SSR) 에서 사용됩니다. 다음의 메서드는 서버 및 브라우저 환경 모두 사용할 수 있습니다.

- renderToString()
- renderToStaticMarkup()

예를들어 Express, Hapi, Koa 같은 노드 기반의 웹서버를 실행할때 renderToString을 호출하여 root component 를 문자열로 렌더링 한 다음 response를 보낼 수 있습니다

```jsx harmony
// using Express
import { renderToString } from 'react-dom/server'
import MyPage from './MyPage'

app.get('/', (req, res) => {
  res.write('<!DOCTYPE html><html><head><title>My Page</title></head><body>')
  res.write('<div id="content">')
  res.write(renderToString(<MyPage/>))
  res.write('</div></body></html>')
  res.end()
})
```

60. ### How to use innerHTML in React?
#### (React에서 innerHTML을 어떻게 사용하나요?)
dangerouslySetInnerHTML 속성은 브라우저 DOM 에서 innerHTML 을 사용하기 위한 React 의 대체 속성입니다.
innerHTML과 마찬가지로 XXS (Cross-Site Scripting) 공격을 고려했을때 브라우저 DOM 에서 이 속성을 사용하는 것은 위험합니다.
__html 객체를 키로 HTML 텍스트를 값으로 전달하면됩니다. 

예제에서 MyComponent는 HTML 마크업을 설정하기 위해 dangerouslySetInnerHTML 속성을 사용하고 있습니다.

```jsx harmony
function createMarkup() {
  return { __html: 'First &middot; Second' }
}

function MyComponent() {
  return <div dangerouslySetInnerHTML={createMarkup()} />
}
```

영어 원본: [Git Hub](https://github.com/sudheerj/reactjs-interview-questions#what-is-reactjs)    
블로그 번역: [Git Hub](https://github.com/appear/reactjs-interview-questions-ko)