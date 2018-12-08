---
title: React Interview (리액트 면접 질문) [121 ~ 130]
date: 2018-11-23 10:00:31
tags: React
---

# React Interview

원작자인 [sudheerj](https://github.com/sudheerj) 의 동의를 구하고 진행하는 번역본 입니다 :)     

처음 해보는 번역이라 어색한 문맥이 많습니다 :) ..

-------------------------------------------------------------------
| No. | Questions |
|---- | ---------
||**Core ReactJS**| |
|121| [How to programmatically trigger click event in React?](#how-to-programmatically-trigger-click-event-in-react) |
|122| [Is it possible to use async/await in plain React?](#is-it-possible-to-use-asyncawait-in-plain-react) |
|123| [What are the common folder structures for React?](#what-are-the-common-folder-structures-for-react) |
|124| [What are the popular packages for animation?](#what-are-the-popular-packages-for-animation) |
|125| [What is the benefit of styles modules?](#what-is-the-benefit-of-styles-modules) |
|126| [What are the popular React-specific linters?](#what-are-the-popular-react-specific-linters) |
|127| [How to make AJAX call and In which component lifecycle methods should I make an AJAX call?](#how-to-make-ajax-call-and-in-which-component-lifecycle-methods-should-i-make-an-ajax-call) |
|128| [What are render props?](#what-are-render-props) |
|   | **React Router** |
|129| [What is React Router?](#what-is-react-router) |
|130| [How React Router is different from history library?](#how-react-router-is-different-from-history-library) |

121. ### How to programmatically trigger click event in React?
#### (React 에서 어떻게 프로그래밍 방식으로 클릭 이벤트를 트리거 할 수 있나요?)
callback 을 통한 ref prop 를 사용하여 `HTMLInputElement` 객체에 대한 참조를 가져와 class property 로 저장하고,
나중에 저장된 참조를 사용를 이용하여 `HTMLElement.click` 메서드를 사용해 이벤트 핸들러에서 클릭 이벤트를 트리거 할 수 있습니다.  

이 작업은 두 단계로 나눌 수 있습니다.

1. render 메서드 안에서 ref 생성
 
```jsx harmony
<input ref={input => this.inputElement = input} />
```

2. 이벤트 핸들러에서 클릭 이벤트 제공:

```js
this.inputElement.click()
```

122. ###(Is it possible to use async/await in plain React?
#### (기본 React 에서 async/await 를 사용할 수 있나요?)
React 에서 async/await 을 사용하고 싶다면 Babel 과 transform-async-to-generator 플러그인이 필요합니다.

123. ### What are the common folder structures for React?
#### (React 의 일반 폴더 구조는 무엇인가요?)

여기 React project 의 파일 구조에 대한 두 가지의 사례가 있습니다.

1. 기능 또는 경로별로의 그룹:
프로젝트를 구조화하는 하나의 일반적인 방법으로는 기능이나 경로별로 그룹화된 CSS, JS, test 들을 함께 구성하는 것입니다.

```text
common/
├─ Avatar.js
├─ Avatar.css
├─ APIUtils.js
└─ APIUtils.test.js
feed/
├─ index.js
├─ Feed.js
├─ Feed.css
├─ FeedStory.js
├─ FeedStory.test.js
└─ FeedAPI.js
profile/
├─ index.js
├─ Profile.js
├─ ProfileHeader.js
├─ ProfileHeader.css
└─ ProfileAPI.js
```

2. 파일 형식의 그룹화
프로젝트를 구조화하는 다른 인기있는 방법으로는 유사한 파일들을 그룹화하는 것입니다.

```text
api/
├─ APIUtils.js
├─ APIUtils.test.js
├─ ProfileAPI.js
└─ UserAPI.js
components/
├─ Avatar.js
├─ Avatar.css
├─ Feed.js
├─ Feed.css
├─ FeedStory.js
├─ FeedStory.test.js
├─ Profile.js
├─ ProfileHeader.js
└─ ProfileHeader.css
```

124. ### What are the popular packages for animation?
#### (애니메이션을 위한 인기있는 패키지는 무엇인가요?)
React 생태계에서 인기있는 애니메이션 패키지는 React Transition Group과 React Motion 입니다.

125. ### What is the benefit of styles modules?
#### (styles modules 의 이점은 무엇인가요?)
styles modules 은 component 에서 style 의 값을 하드 코딩하는 것을 피하기 위해 추천됩니다. 
서로 다른 UI component 에서 사용될 수 있는 값들은 자체 모듈에서 받아와야 합니다.

예를 들어, 이런 style 들은 별도의 component 로 받아낼 수 있습니다.

```js
export const colors = {
  white,
  black,
  blue
}

export const space = [
  0,
  8,
  16,
  32,
  64
]
```
 
다른 component 들에서 개별적으로 가져올 수 있습니다.  

```
import { space, colors } from './styles'
```

126. ### What are the popular React-specific linters?
#### (인기있는 React linters 는 무엇인가요?)
ESLint 는 인기있는 Javascript linters 입니다. ESLint 는 코드 스타일을 분석할 수 있는 플러그인입니다.
React 에서 대부분 사용하는 npm 패키지 중 하나는 `eslint-plugin-react` 입니다.
기본적으로 몇가지의 best practices 를 확인하여 규칙을 바탕으로 iterators 의 key 에서 전체 prop type 까지 확인합니다.
다른 인기있는 플러그인은 `eslint-plugin-jsx-a11y` 이며, 접근성을 통해 문제를 해결하는데 도움을 줍니다.
JSX 는 일반적인 HTML 문법과 약간 다르게 제공하므로, `alt` text 그리고 `tabindex` 같은 문제는 플러그인으로 선택되지 않습니다.

127. ### How to make AJAX call and in which component lifecycle methods should I make an AJAX call?
#### (어떻게 AJAX 를 호출하고 어떤 lifecycle 에서 메서드를 호출해야하나요?)
AJAX 라이브러리인 Axios, jQuery AJAX, 그리고 브라우저에 내장된 fetch 를 사용할 수 있습니다.
`componentDidMount()` lifecycle 메서드에서 데이터를 불러와야합니다.
setStaet() 를 사용하여 데이터를 검색할때 component 를 update 할 수 있습니다.

예를 들어, API 에서 직원목록을 가져와 local state 를 설정합니다:

```jsx harmony
class MyComponent extends React.Component {
  constructor(props) {
    super(props)
    this.state = {
      employees: [],
      error: null
    }
  }

  componentDidMount() {
    fetch('https://api.example.com/items')
      .then(res => res.json())
      .then(
        (result) => {
          this.setState({
            employees: result.employees
          })
        },
        (error) => {
          this.setState({ error })
        }
      )
  }

  render() {
    const { error, employees } = this.state
    if (error) {
      return <div>Error: {error.message}</div>;
    } else {
      return (
        <ul>
          {employees.map(item => (
            <li key={employee.name}>
              {employee.name}-{employees.experience}
            </li>
          ))}
        </ul>
      )
    }
  }
}
```

128. ### What are render props?
#### (Render Props 란 무엇인가요?)
Render Props 는 값이 함수인 prop 를 이용하여 component 간에 코드를 공유하는 기술입니다.
아래 component 는 render prop 을 사용하여 React element 를 반환합니다.

```jsx harmony
<DataProvider render={data => (
  <h1>{`Hello ${data.target}`}</h1>
)}/>
```

React Router 와 DownShift 라이브러리는 이 패턴을 사용합니다.

## React Router

129. ### What is React Router?
#### (React Router 가 무엇인가요?)
React Router 는 React 에 구현된 강력한 routing 라이브러리 입니다.
페이지에 보여지는 내용과 URL 을 동기화된 상태로 유지해주고, 애플리케이션에 새로운 화면과 흐름을 추가할 수 있게 도와줍니다.

130. ### How React Router is different from history library?
#### (React router 와 history 라이브러리의 다른점은 무엇인가요?)
React router 는 history 라이브러리를 감싼 래퍼입니다. React router는 브라우저의 `window.history` 과 상호작용을 하고, 브라우저 및 hash history 을 다룹니다.
또 모바일 앱 개발 (React Native) 및 Node 의 unit testing 처럼 global history 가 없는 환경에 유용한 memory 히스토리를 제공합니다.
