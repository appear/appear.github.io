---
title: React Interview (리액트 면접 질문) [11 ~ 20]
date: 2018-10-20 15:55:20
tags: React
---

# React Interview (리액트 면접질문)

원작자인 [sudheerj](https://github.com/sudheerj) 의 동의를 구하고 진행하는 번역본 입니다 :)     

처음 해보는 번역이라 어색한 문맥이 많습니다 :) ..

-------------------------------------------------------------------
| No. | Questions |
|---- | ---------
||**Core ReactJS**| |
|11 | [Why should not we update the state directly?](#why-should-not-we-update-the-state-directly)|
|12 | [What is the purpose of callback function as an argument of setState?](#what-is-the-purpose-of-callback-function-as-an-argument-of-setstate)
|13 | [What is the difference of event handling between HTML and React?](#what-is-the-difference-of-event-handling-between-html-and-react)|
|14 | [How to bind methods or event handlers in JSX callbacks?](#how-to-bind-methods-or-event-handlers-in-jsx-callbacks)|
|15 | [How to pass a parameter to an event handler or callback?](#how-to-pass-a-parameter-to-an-event-handler-or-callback)|
|16 | [What are synthetic events in ReactJS?](#what-are-synthetic-events-in-reactjs)|
|17 | [What is inline conditional expressions?](#what-is-inline-conditional-expressions)|
|18 | [What is Key and benefit of using it in lists?](#what-is-key-and-benefit-of-using-it-in-lists)|
|19 | [What is the use of create refs?](#what-is-the-use-of-create-refs)|
|20 | [How to create refs?](#how-to-create-refs)

11. ### Why should not we update the state directly? 
#### (state를 직접 업데이트 하면 안되는 이유는 무엇인가요?)
state를 직접 업데이트 하려고 한다면 component 는 re-render 되지 않습니다.

```js
//Wrong
this.state.message =”Hello world”;
```

그 대신 setState 메서드를 사용해야합니다. setState는 component state 업데이트에 대한 예약을합니다. state 가 변경되면 component는 re-rendering 할 것 입니다.

```js
//Correct
this.setState({message: ‘Hello World’});
```

Note: 상태를 할당할 수 있는 곳은 constructor 가 유일합니다. (외부에서 다이렉트로 state를 할당하지 말라는 뜻 같습니다.)

12. ### What is the purpose of callback function as an argument of setState? 
#### (setState에서 callback의 역할은 무엇인가요?)
callback 함수는 setState가 끝난 후 그리고 component 가 re-rendering 된 후 호출됩니다. setState는 비동기적으로 동작합니다. callback 함수는 모든 작업이 마무리된 후 사용됩니다. 

```js
setState({name: 'sudheer'}, () => console.log('The name has updated and component re-rendered'));
```
 
#### Note: callback 함수를 이용하는 것보단 lifecycle 메서드를 이용하는 것이 좋습니다.

13. ### What is the difference of event handling between HTML and React? 
#### (HTML과 React의 이벤트 처리 차이점은 무엇인가요?)
아래는 HTML과 React 의 이벤트처리의 몇 가지 차이점입니다.

1. HTML 에서는 이벤트 이름이 소문자여야 합니다.

```html
<button onclick="activateLasers()">
```

반면 React는 camelCase 를 사용합니다.

```html
<button onClick={activateLasers}>
```

2. HTML 에서는 기본 이벤트 동작을 막기위해 false 를 반환할 수 있습니다.

```html
<a href="#" onclick="console.log('The link was clicked.'); return false"/>
```

반면 React는 preventDefault 메서드를 호출해야합니다.

```js
function handleClick(e) {
  e.preventDefault();
  console.log('The link was clicked.');
}
```

14. ### How to bind methods or event handlers in JSX callbacks? (Or) How to use this keyword in JSX callbacks? 
#### (This를 바인딩하는 방법은 어떤 것들이 있나요?)
3가지의 방법이 있습니다.

1. 생성자에서의 바인딩: JS 의 Class 에서 메서드는 기본적으로 바인딩 되지 않습니다. 클래스 메서드로 지정된 React의 event handlers 에서도 마찬가지 입니다.  
일반적으로 다음과같이 constructor 바인딩합니다.

```js
constructor(props) {
  super(props);
  this.handleClick = this.handleClick.bind(this);
}

handleClick() {
  // Perform some logic
}
```

2. 공통 클래스 필드 구문: 생성자에서의 바인딩 방법을 원하지 않는다면, 공용 클래스 필드 구문을 이용하여 callback 을 올바르게 바인딩할 수 있습니다.

```js
handleClick = () => { 
  console.log('this is:', this);
}

<button onClick={this.handleClick}> Click me </button>
```

3. [arrow function](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Functions/%EC%95%A0%EB%A1%9C%EC%9A%B0_%ED%8E%91%EC%85%98)을 이용한 바인딩
아래와 같이 바로 arrow function 을 이용하여 바인딩 해줄 수 있습니다.

```html
<button onClick={(e) => this.handleClick(e)}>
  Click me
</button>
```

Note: 만약 callback 이 하위 component 에 props 로 전달되면 component는 re-rendering 될 수 있습니다. 이럴 경우 성능을 고려하였을때 1번 또는 2번의 방식을 사용하는 것이 좋습니다.

15. ### How to pass a parameter to an event handler or callback? 
#### (이벤트 핸들러 또는 콜백에 매개변수를 어떻게 전달하나요?)
arrow function 으로 감싸서 event handler 에게 매개변수를 전달할 수 있습니다.

```js
<button onClick={() => this.handleClick(id)} />
```

위의 방법은 아래와 같이 method 를 사용하는 것과 같습니다

```html
<button onClick={this.handleClick.bind(this, id)} />
```
 
16. ### What are synthetic events in ReactJS? 
#### (React에서의 합성 이벤트는 무엇인가요?)
[synthetic event](https://reactjs.org/docs/events.html) 는 브라우저의 기본 이벤트를 감싼 cross-browser wrapper 입니다. API는 모든 브라우저에서 동작한다는 점을 제외하면, stopPropagation () 및 preventDefault () 를 포함해 브라우저 네이티브 이벤트와 같은 인터페이스를 가지고 있습니다.

17. ### What is inline conditional expressions? 
#### (인라인 조건식은 무엇인가요?)
조건부 표현식을 표현하기 위해 if 문 또는 삼항 연산자를 사용할 수 있습니다. 이런 표현법 외에도 중괄호로 묶은 다음 JS의 논리 연산자 (&&) 를 붙여 JSX 표현식에 포함시킬 수 있습니다.

```html
<h1>Hello!</h1>
 {messages.length > 0 &&
<h2>
  You have {messages.length} unread messages.
</h2>
```

18. ### What is Key and benefit of using it in lists? 
#### (list 에서 key를 사용했을 때의 이점은 무엇인가요?)
["Key"](https://reactjs.org/docs/lists-and-keys.html)는 목록을 만들때 포함시켜야하는 특수한 속성입니다. "Key"는 목록의 변경사항, 추가 또는 제거된 항목을 분별할 수 있도록 도와줍니다.  

예를 들어, 데이터의 키를 자주 목록의 "Key"로 사용합니다.

```jsx harmony
const todoItems = todos.map((todo) =>
  <li key={todo.id}>
    {todo.text}
  </li>
);
```
What is ReactJS? (React란 무엇인가요?)
렌더링 된 목록에 안정적인 ID가 없을 경우 마지막 수단으로 index 값을 이용할 수 있습니다.

```jsx harmony
const todoItems = todos.map((todo, index) =>
  <li key={index}>
    {todo.text}
  </li>
);
```

Note: 
1. 항목 순서가 변경 될 가능성이 있는 경우 index를 이용하는 것은 좋지 않습니다. 성능에 부정적인 영향을 미치고 component state에 문제가 발생할 수 있습니다.
2. list를 별도의 component로 뽑아 사용하는 경우 li 태그 대신 list component 요소에 key를 적용하세요

list 에 key 가 없으면 콘솔에 경고가 표시됩니다.

19. ### What is the use of create refs? 
#### (ref의 용도는 무엇인가요?)
두 가지의 접근법이 있습니다.

최근에 추가된 접근 방식입니다. [create ref](https://reactjs.org/docs/refs-and-the-dom.html)는 element 요소에 대한 참조를 반환하는데 사용할 수 있습니다. DOM의 요소나 compoennt 에 직접 접근해야 될 경우 유용할 수 있습니다.

20. ### How to create refs? 
#### (create refs를 어떻게 만드나요?) 
Ref는 React.createRef() 메서드를 사용하여 생성합니다. ref attribute 을 통해 React elements 에 첨부됩니다. component 전체에서 사용하고 싶다면 constructor에서 instance property 로 ref를 할당하면 됩니다.

```jsx harmony
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.myRef = React.createRef();
  }
  render() {
    return <div ref={this.myRef} />;
  }
}
```

2. React의 버전과 관계없이 ref callback 을 사용할 수 있습니다. 예를들어 serach bar component의 input element는 다음과 같이 접근합니다.

```jsx harmony
class SearchBar extends Component {
   constructor(props) {
      super(props);
      this.txtSearch = null;
      this.state = { term: '' };
      this.setInputSearchRef = e => {
         this.txtSearch = e;
      }
   }
   onInputChange(event) {
      this.setState({ term: this.txtSearch.value });
   }
   render() {
      return (
         <input
            value={this.state.term}
            onChange={this.onInputChange.bind(this)}
            ref={this.setInputSearchRef} />
      );
   }
}
```


영어 원본: [Git Hub](https://github.com/sudheerj/reactjs-interview-questions#what-is-reactjs)    
블로그 번역: [Git Hub](https://github.com/appear/reactjs-interview-questions-ko)