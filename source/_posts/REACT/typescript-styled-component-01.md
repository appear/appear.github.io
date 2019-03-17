---
title: Typescript 에서 steyld-component props type 적용하기
date: 2019-03-17 14:28:14
tags: React
---

# 타입스크립에서 styled component 적용

styled-component 는 보통 props 를 아래와 같이 유동적으로 받습니다.

```js
const Container = styled.div`
  box-sizing: border-box;

  width: ${({ width }) => (width ? width : "100%")};

  ${({ textAlign }) =>
    textAlign &&
    css`
      text-align: ${textAlign};
    `}

  ${({ minHeight }) =>
    minHeight &&
    css`
      min-height: ${minHeight};
    `}

  ${({ maxHeight }) =>
    maxHeight &&
    css`
      max-width: ${maxHeight};
    `}
`;
```

위의 경우 width, textAlign, minHeight, maxHeight 의 타입을 알 수 없기 떄문에 type checker 에 걸립니다.
이를 해결하기 위해 다음과 같은 방법을 이용할 수 있습니다.

```js
interface IWidth {
  width: string
}

interface ITextAlign {
  textAlign: string
}

width: ${({ width }: IWidth) => (width ? width : "100%")};

${({ textAlign }: ITextAlign) =>
  textAlign &&
  css`
    text-align: ${textAlign};
  `}

```

위의 방법으로 해결할 수 있지만 interface 가 속성 갯수에 따라 증가하게 됩니다.  
아주 간단한 방법으로 개선할 수 있습니다.

```js
interface IContainer {
  width: string
  textAlign: string
  minHeight: string
  maxHeight: string
}

const Container = styled.div<IContainer>`
  box-sizing: border-box;

  width: ${({ width }) => (width ? width : "100%")};

  ${({ textAlign }) =>
    textAlign &&
    css`
      text-align: ${textAlign};
    `}

  ${({ minHeight }) =>
    minHeight &&
    css`
      min-height: ${minHeight};
    `}

  ${({ maxHeight }) =>
    maxHeight &&
    css`
      max-width: ${maxHeight};
    `}
`;


<Container> Hello Steyld Component </Container>
```

위와 같이 정의한 컴포넌트를 사용하였을때 아래의 에러를 볼 수 있습니다.

![gatsby](/img/react/ts-styled-component01.png)

위의 container component 의 props 들은 고정적으로 들어올 수 도 아닐 수도 있는 값이기 때문에 interface 에 다음과 같은 처리를 해줘야합니다.

```js
interface IContainer {
  width?: string
  textAlign?: string
  minHeight?: string
  maxHeight?: string
}
```
