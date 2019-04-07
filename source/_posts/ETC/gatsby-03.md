---
title: Gatsby 를 이용하여 Resume Page 만들기 (2)
date: 2019-04-06 13:55:50
tags: Gatsby
---

# Gatsby 를 이용하여 Resume Page 만들기 (2)

1 에서는 Gatsby 의 기본적인 세팅과 설정 및 불필요한 파일들 제거를 했으니 이제는 우리가 필요한 component 들을 추가해야 될 차례입니다.

## Global Reset style

우리가 의도한 style 가 다르게 나올 수 있으니 방지하고자 reset style 을 추가한 후 해보겠습니다.

```js
// src/components/reset-style.js

import { createGlobalStyle } from "styled-components";

export default createGlobalStyle` 
  html,
  body,
  div,
  span,
  applet,
  object,
  iframe,
  h1,
  h2,
  h3,
  h4,
  h5,
  h6,
  p,
  blockquote,
  pre,
  a,
  abbr,
  acronym,
  address,
  big,
  cite,
  code,
  dfn,
  del,
  em,
  img,
  ins,
  kbd,
  q,
  s,
  samp,
  small,
  strike,
  strong,
  sub,
  sup,
  tt,
  var,
  b,
  u,
  i,
  center,
  dl,
  dt,
  dd,
  ol,
  ul,
  li,
  fieldset,
  form,
  label,
  legend,
  table,
  caption,
  tbody,
  tfoot,
  thead,
  tr,
  th,
  td,
  article,
  aside,
  canvas,
  details,
  embed,
  figure,
  figcaption,
  footer,
  header,
  hgroup,
  main,
  menu,
  nav,
  output,
  ruby,
  section,
  summary,
  time,
  mark,
  audio,
  video {
    margin: 0;
    padding: 0;
    border: 0;
    font-size: 100%;
    font: inherit;
    vertical-align: baseline;
  }
  /* HTML5 display-role reset for older browsers */
  article,
  aside,
  details,
  figcaption,
  figure,
  footer,
  header,
  hgroup,
  main,
  menu,
  nav,
  section {
    display: block;
  }
  /* HTML5 hidden-attribute fix for newer browsers */
  *[hidden] {
    display: none;
  }
  body {
    line-height: 1;
  }
  ol,
  ul {
    list-style: none;
  }
  blockquote,
  q {
    quotes: none;
  }
  blockquote:before,
  blockquote:after,
  q:before,
  q:after {
    content: '';
    content: none;
  }
  table {
    border-collapse: collapse;
    border-spacing: 0;
  }
`;
```

```js
// src/pages/index.js

import ResetStyle from "../components/reset-style";

// render
<div>
  <ResetStyle />
  <SEO keywords={[`gatsby`, `resume`, `react`]} />
</div>;
```

## Common Component

모든 Component 에서 공통적으로 사용될 component 를 추가합니다.

Container 는 component 를 감싸는 용도로 사용합니다.

```js
// src/components/container.js

import React from "react";
import styled, { css } from "styled-components";

const Container = styled.div`
  ${({ flex }) =>
    flex &&
    css`
      display: flex;
    `}
    
  ${({ flexFlow }) =>
    flexFlow &&
    css`
      flex-flow: ${flexFlow};
    `}  

  ${({ ratio }) =>
    ratio &&
    css`
      flex: ${ratio};
    `} 

  ${({ margin }) =>
    margin &&
    css`
      padding-top: ${margin.top}px;
      padding-left: ${margin.left}px;
      padding-bottom: ${margin.bottom}px;
      padding-right: ${margin.right}px;
    `}


  ${({ padding }) =>
    padding &&
    css`
      padding-top: ${padding.top}px;
      padding-left: ${padding.left}px;
      padding-bottom: ${padding.bottom}px;
      padding-right: ${padding.right}px;
    `}
`;

export default Container;
```

Text 는 text 를 표현할 때 사용합니다. size 와 padding, margin 을 조절 할 수 있습니다.

```js
// src/components/text.js

import React from "react";
import styled, { css } from "styled-components";

const SIZES = {
  mini: "12px",
  tiny: "13px",
  small: "14px",
  medium: "17px",
  large: "20px",
  big: "24px",
  huge: "28px",
  massive: "32px"
};

const Text = styled.div`
  font-size: ${({ size }) => SIZES[size || "small"]};

  ${({ margin }) =>
    margin &&
    css`
      padding-top: ${margin.top}px;
      padding-left: ${margin.left}px;
      padding-bottom: ${margin.bottom}px;
      padding-right: ${margin.right}px;
    `}


  ${({ padding }) =>
    padding &&
    css`
      padding-top: ${padding.top}px;
      padding-left: ${padding.left}px;
      padding-bottom: ${padding.bottom}px;
      padding-right: ${padding.right}px;
    `}

  ${({ bold }) =>
    bold &&
    css`
      font-weight: bold;
    `}
`;

export default Text;
```

## Main Container

전체 Layout 을 잡아줄 Container Component 를 추가하겠습니다.

```js
// src/pages/index.js

import styled from "styled-components";

const Container = styled.div`
  width: 100%;
  max-width: 720px;
  margin: 0 auto;
`;

// render
<Container>
  <ResetStyle />
  <SEO keywords={[`gatsby`, `resume`, `react`]} />
</Container>;
```

## Intro Component

Intro 는 최상단에 표시되는 component 입니다. 소개글을 표현합니다.
데이터를 추가하고 component 를 추가해보겠습니다.

```json
// src/data/resume/item.json

 "intro": {
    "header": "안녕하세요 프론트엔드 개발자 고석진입니다.",
    "content": "2년차를 향해 달려가고 있는 프론트 개발자입니다. 새로운 기술들을 학습하는 것을 좋아하며 틀에 박힌 것보다는 다양한 경험을 할 수 있는 환경을 좋아합니다."
  },
```

intro component 를 추가합니다.

```js
// src/components/intro.js

import React from "react";
import styled from "styled-components";

const IntroFrame = styled.div`
  padding: 40px 20px;
`;

const Title = styled.div`
  font-size: 34px;
  font-weight: bold;
  color: rgb(51, 51, 51);
  margin-bottom: 20px;
`;

const Content = styled.div`
  font-size: 20px;
  color: rgba(51, 51, 51, 0.8);
  line-height: 1.27;
`;

function Intro({ header, content }) {
  return (
    <IntroFrame>
      <Title>{header}</Title>
      <Content>{content}</Content>
    </IntroFrame>
  );
}

export default Intro;
```

index page 에서 header, content props 를 넘겨주겠습니다.

```js
// src/pages/index.js

import Intro from "../components/intro"

const {
  // ...
  intro,
} = item.allResumeJson.edges[0].node

// render
<Container>
  <ResetStyle />
  <SEO keywords={[`gatsby`, `resume`, `react`]} />
  <Intro {...intro} />
</Container>;
```

아래와 같이 내용이 표시되는 것을 볼 수 있습니다

![gatsby](/img/etc/resume06.png)

## Work Experience Component

회사 경력을 표현할 수 있는 Component 를 추가해보겠습니다.
