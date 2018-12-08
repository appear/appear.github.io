---
title: React Interview (리액트 면접 질문) [21 ~ 30]
date: 2018-10-20 15:56:20
tags: React
---

# React Interview

원작자인 [sudheerj](https://github.com/sudheerj) 의 동의를 구하고 진행하는 번역본 입니다 :)     

처음 해보는 번역이라 어색한 문맥이 많습니다 :) ..

-------------------------------------------------------------------
| No. | Questions |
|---- | ---------
||**Core ReactJS**| |
|21 | [What are forward refs?](#what-are-forward-refs) |
|22 | [Which is preferred option with in callback refs and findDOMNode()?](#which-is-preferred-option-with-in-callback-refs-and-finddomnode) |
|23 | [Why are String Refs legacy?](#why-are-string-refs-legacy) |
|24 | [What is Virtual DOM?](#what-is-virtual-dom) |
|25 | [How Virtual DOM works?](#how-virtual-dom-works) |
|26 | [What is the difference between Shadow DOM and Virtual DOM?](#what-is-the-difference-between-shadow-dom-and-virtual-dom) |
|27 | [What is React Fiber?](#what-is-react-fiber) |
|28 | [What is the main goal of React Fiber?](#what-is-the-main-goal-of-react-fiber) |
|29 | [What are controlled components?](#what-are-controlled-components) |
|30 | [What are uncontrolled components?](#what-are-uncontrolled-components) |


21. ### What are forward refs?
#### (forward refs 란 무엇인가요?)

Ref Forwarding 은 ref를 받아 child component 에게 전달하는 기능을합니다.

```jsx harmony
const ButtonElement = React.forwardRef((props, ref) => (
  <button ref={ref} className="CustomButton">
    {props.children}
  </button>
));

// Create ref to the DOM button:
const ref = React.createRef();
<ButtonElement ref={ref}>{'Forward Ref'}</ButtonElement>
```

22. ### Which is preferred option with in callback refs and findDOMNode()?
#### (callback refs 와 findDOMNode 중에 어떤것을 더 선호하나요?)

findDOMNode API 보다 callback refs를 사용하는 것이 좋습니다. [findDOMNode](https://reactjs.org/docs/react-dom.html#finddomnode)는 향후 개선사항에서 React의 특성을 막기 떄문입니다.

findDOMNode 를 사용하는 legacy 한 방식입니다.

```jsx harmony
class MyComponent extends Component {
  componentDidMount() {
    findDOMNode(this).scrollIntoView()
  }

  render() {
    return <div />
  }
}
```

권장하는 방법입니다.

```jsx harmony
class MyComponent extends Component {
  componentDidMount() {
    this.node.scrollIntoView()
  }

  render() {
    return <div ref={node => this.node = node} />
  }
}
```

23. ### Why are String Refs legacy?
(왜 String Refs 는 legacy가 되었나요?)

만약 예전 React에서 ref 를 다뤄봤다면, ref={'textInput'} 과 같은 ref 의 속성이 string 이고 DOM Node가 refs.textInput 으로 접근하는 API 방식에 익숙할 것 입니다.  
String refs 는 아래와 같은 문제들이 있기 떄문에 legacy 를 고려하였습니다. React v16 에서 String refs는 제거되었습니다.

1. String refs 는 실행중인 component 요소를 추적하도록 강제화합니다. 또 react module 을 stateful 하게 만듭니다. bundle시 react module 들이 중복될 때
 이상한 오류를 발생시킵니다.
2. 라이브러리를 추가하여 String Refs를 Child component 에 전달하면, 사용자는 다른 ref를 추가할 수 없게됩니다. Callback refs를 이용한다면 이런 문제를 해결할 수 있습니다.
3. Flow 와 같은 static 분석에서는 동작하지 않습니다. Flow 는 string refs를 this.refs 같은 형태로 표시하도록 만드는 마법을 추측할 수 없습니다. callback ref는 String refs 보다 Flow 와 친밀합니다. 
4. 대부분의 사람들이 "render callback" 패턴으로 동작하기를 기대하지만 그렇게 동작하지 않습니다.

```jsx harmony
class MyComponent extends Component {
  renderRow = (index) => {
    // 이것은 동작하지 않습니다. Ref는 MyComponent가 아닌 DataTable 에 연결될 것입니다. 
    return <input ref={'input-' + index} />;

    // 이것은 동작합니다. Callback refs는 굉장합니다.
    return <input ref={input => this['input-' + index] = input} />;
  }

  render() {
    return <DataTable data={this.props.data} renderRow={this.renderRow} />
  }
}
```

24. ### What is Virtual DOM?
#### (가상돔이란 무엇인가요?)
가상돔이란 in-memory 즉 Real DOM의 메모리상에서의 표현입니다. UI의 표현은 메모리에 유지되고 Render함수와 화면을 표시하는 사이에 Real DOM 과 동기화됩니다. 이런 단계를 reconciliation 라고 부릅니다.

25. ### How Virtual DOM works?
#### [(가상돔은 어떻게 동작하나요?)](https://www.youtube.com/watch?v=BYbgopx44vo)
가상돔은 세 가지의 간단한 단계로 동작합니다.

1. 데이터가 변경될 때 전체 UI 는 가상돔안에서 re-rendered 됩니다.   

![vdom](https://github.com/appear/reactjs-interview-questions-ko/raw/master/public/vdom1.png)

2. 그런 다음 변경되기전 DOM 과 새로운 변경된 DOM 의 변경점을 계산합니다.   

![vdom](https://github.com/appear/reactjs-interview-questions-ko/raw/master/public/vdom2.png)

3. 계산이 완료되면 실제 DOM 에서 계산된 변경점들만 업데이트됩니다.     

![vdom](https://github.com/appear/reactjs-interview-questions-ko/raw/master/public/vdom3.png)

26. ### What is the difference between Shadow DOM and Virtual DOM?
#### (Shadow DOM과 Virtual DOM의 차이점은 무엇인가요?)
Shadow DOM은 web component의 scope 및 CSS scope 지정을 위해 설계된 web browser 기술입니다. Virtual DOM 은 browser API를 기반으로 JS 라이브러리에서 구현되는 개념입니다.

27. ### What is React Fiber?
#### (React Fiber란 무엇인가요?)
React Fiber란 React v16 에서 핵심 알고리즘을 재구현 한것입니다. React Fiber 의 목표는 애니메이션, 레이아웃, 제스처, 작업을 일시정지하고, 중단 또는 재사용, 여러 유형의 업데이트 우선순위 조절, 동시성등 여러 기본사항에 대한 성능을 높이는 것 입니다.

28. ### What is the main goal of React Fiber?
#### (React Fiber의 주요 목표는 무엇인가요?)
React Fiber 의 목표는 애니메이션, 레이아웃, 제스처등의 성능을 높이는 것 입니다. 주요 목적은 incremental rendering 입니다.

**incremental rendering:** 렌더링 작업을 청크로 쪼개고 여러 프레임으로 분산 시키는 기능입니다.

29. ### What are controlled components?
#### (controlled components 란 무엇인가요?)
입력 요소를 제어하는 component를 controlled components 라고 부릅니다. 모든 상태 변경에는 연관된 handler funciton 이 있습니다.

예를 들어, 이름을 대문자로 쓰려면 아래 handleChange을 이용할 수 있습니다.

```jsx harmony
handleChange(event) {
  this.setState({value: event.target.value.toUpperCase()})
}
```

30. ### What are uncontrolled components?
#### (uncontrolled components 란 무엇인가요?)
uncontrolled components란 내부적으로 자기 자신의 state를 가지고 있는 component입니다. 현재 필요한 값을 찾기 위해 ref를 사용하여 DOM query를 할 수 있습니다. 이것은 전통적인 HTML 과 비슷합니다.

아래의 UserProfile Component 에서의 이름 입력은 ref를 사용하여 접근합니다.

```jsx harmony
class UserProfile extends React.Component {
  constructor(props) {
    super(props)
    this.handleSubmit = this.handleSubmit.bind(this)
    this.input = React.createRef()
  }

  handleSubmit(event) {
    alert('A name was submitted: ' + this.input.current.value)
    event.preventDefault()
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          {'Name:'}
          <input type="text" ref={this.input} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```
  
대부분의 경우 forms 을 제어할 때는 controlled component 를 사용하는 것을 추천합니다.



영어 원본: [Git Hub](https://github.com/sudheerj/reactjs-interview-questions#what-is-reactjs)    
블로그 번역: [Git Hub](https://github.com/appear/reactjs-interview-questions-ko)