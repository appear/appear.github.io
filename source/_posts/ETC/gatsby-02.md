---
title: Gatsby 를 이용하여 Resume Page 만들기 (1)
date: 2019-04-06 13:55:50
tags: Gatsby
---

# Gatsby 를 이용하여 Resume Page 만들기 (1)

gatsby 에 대한 기본적인 예제는 [여기](https://appear.github.io/2019/03/10/ETC/gatsby-01/) 에서 확인해주세요

## 프로젝트 생성

```bash
$ npx gatsby new resume https://github.com/gatsbyjs/gatsby-starter-default
$ cd resume
& npm run develop
```

![gatsby](/img/etc/resume01.png)

## 이력서 컨셉

이력서는 아래와 같은 구조와 컨셉으로 구성해보려고합니다.

![gatsby](/img/etc/resume03.png)

## styled-components

style 제어는 `styled-component` 를 이용하겠습니다.

```bash
$ npm i --save styled-components
```

## Gatsby-transformer-json

페이지의 필요한 데이터들을 [GraphQL](https://graphql.org/learn/) 을 이용하여 다루고자 합니다.
Json 파일을 GraphQl Query 와 같이 이용하기 위해서는 `gatsby-transformer-json` plugin 이 필요합니다.

```bash
$ npm i --save gatsby-transformer-json
```

`gastby-config` 의 plugins 에 아래 설정을 추가해주세요

```js
plugins: [
  // ...
  `gatsby-transformer-json`,
  {
    resolve: `gatsby-source-filesystem`,
    options: {
      path: `${__dirname}/src/data/`
    }
  }
  // ...
];
```

## Json Data 추가

`src/data/resume/item.json` 이라는 파일을 만들어주겠습니다.

```json
{
  "work_experience": [
    {
      "title": "트리플",
      "date": "2018.07 - ing"
    }
  ]
}
```

위의 데이터를 호출하는 query 는 아래와 같습니다. Query name 은 자유롭게 정할 수 있습니다.  
저는 `ResumeItemsQuery` 라는 이름을 사용합니다.
`allResumeJson` 의 경우 `all[폴더이름]Query` 와 같은 `gatsby-transformer-json` 의 규칙을 따릅니다.

```js
query ResumeItemsQuery {
  allResumeJson {
    edges{
      node {
        work_experience {
          title,
          date
        }
      }
    }
  }
}
```

파일을 추가한 후 http://localhost:8000/___graphql 에서 query 가 정상 작동하는지 테스트해보겠습니다.

![gatsby](/img/etc/resume04.png)

나머지 데이터들을 추가하겠습니다.

```json
{
  "intro": {
    "header": "안녕하세요 프론트엔드 개발자 고석진입니다.",
    "content": "2년차를 향해 달려가고 있는 프론트 개발자입니다. 새로운 기술들을 학습하는 것을 좋아하며 틀에 박힌 것보다는 다양한 경험을 할 수 있는 환경을 좋아합니다."
  },
  "work_experience": [
    {
      "title": "트리플",
      "date": "2018.07 - ing",
      "role": "UI Developer",
      "experiences": [
        {
          "title": "Design System 개발",
          "description": "",
          "skill": ""
        }
      ]
    }
  ],
  "personal_experience": [
    {
      "title": "",
      "date": "",
      "description": ""
    }
  ],
  "contact": [
    {
      "name": "",
      "link": ""
    }
  ]
}
```

## StaticQuery

stateless component 를 감싸는 StaticQuery 를 이용하여 query 를 실행하고 결과를 component 의 props 로 전달해보겠습니다.

```js
<StaticQuery query={graphql``} render={(data) => { // ...data }}/>
```

`pages/index.js` 에 적용해보겠습니다.

```js
import { StaticQuery, graphql } from "gatsby";

const IndexPage = () => (
  <StaticQuery
    query={graphql`
      query ResumeItemsQuery {
        allResumeJson {
          edges {
            node {
              intro {
                header
                content
              }
              work_experience {
                title
                date
                role
                experiences {
                  title
                  description
                  skill
                }
              }
              personal_experience {
                title
                date
              }
              contact {
                name
                link
              }
            }
          }
        }
      }
    `}
    render={item => {
      const {
        work_experience,
        personal_experience,
        contact,
        intro
      } = item.allResumeJson.edges[0].node;
      return (
        <Layout>
          <SEO title="Home" keywords={[`gatsby`, `application`, `react`]} />
          <h1>Hi people</h1>
          <p>Welcome to your new Gatsby site.</p>
          <p>Now go build something great.</p>
          <div style={{ maxWidth: `300px`, marginBottom: `1.45rem` }}>
            <Image />
          </div>
          <Link to="/page-2/">Go to page 2</Link>
        </Layout>
      );
    }}
  />
);
```

![gatsby](/img/etc/resume05.png)

## Component 를 추가하기전에

불필요한 컴포넌트들을 제거하고 시작하겠습니다.

1. `pages/index.js` 을 아래부분을 제외하고 다 제거하겠습니다.

```js
// before
<Layout>
  <SEO title="Home" keywords={[`gatsby`, `application`, `react`]} />
  <h1>Hi people</h1>
  <p>Welcome to your new Gatsby site.</p>
  <p>Now go build something great.</p>
  <div style={{ maxWidth: `300px`, marginBottom: `1.45rem` }}>
    <Image />
  </div>
  <Link to="/page-2/">Go to page 2</Link>
</Layout>

// after
<div>
  <SEO keywords={[`gatsby`, `resume`, `react`]} />
</div>
```

2. SEO 컴포넌트에서 title 을 graphQL 의 값을 이용하도록 변경하겠습니다.

```js
// src/components/seo.js

import React from "react";
import PropTypes from "prop-types";
import Helmet from "react-helmet";
import { useStaticQuery, graphql } from "gatsby";

function SEO({ description, lang, meta, keywords }) {
  const { site } = useStaticQuery(
    graphql`
      query {
        site {
          siteMetadata {
            title
            description
            author
          }
        }
      }
    `
  );

  const metaDescription = description || site.siteMetadata.description;

  return (
    <Helmet
      htmlAttributes={{
        lang
      }}
      title={site.siteMetadata.title}
      titleTemplate={`%s | ${site.siteMetadata.title}`}
      meta={[
        {
          name: `description`,
          content: metaDescription
        },
        {
          property: `og:title`,
          content: site.siteMetadata.title
        },
        {
          property: `og:description`,
          content: metaDescription
        },
        {
          property: `og:type`,
          content: `website`
        },
        {
          name: `twitter:card`,
          content: `summary`
        },
        {
          name: `twitter:creator`,
          content: site.siteMetadata.author
        },
        {
          name: `twitter:title`,
          content: site.siteMetadata.title
        },
        {
          name: `twitter:description`,
          content: metaDescription
        }
      ]
        .concat(
          keywords.length > 0
            ? {
                name: `keywords`,
                content: keywords.join(`, `)
              }
            : []
        )
        .concat(meta)}
    />
  );
}

SEO.defaultProps = {
  lang: `en`,
  meta: [],
  keywords: [],
  description: ``
};

SEO.propTypes = {
  description: PropTypes.string,
  lang: PropTypes.string,
  meta: PropTypes.arrayOf(PropTypes.object),
  keywords: PropTypes.arrayOf(PropTypes.string)
};

export default SEO;
```

3. 404 page component 수정

```js
// src/pages/404.js

import React from "react";

const NotFoundPage = () => (
  <div>
    <h1>NOT FOUND</h1>
    <p>You just hit a route that doesn&#39;t exist... the sadness.</p>
  </div>
);

export default NotFoundPage;
```

3. 불필요한 component 들을 제거합니다.

- src/pages/page-2.js
- src/components/seo 를 제외한 모든 component 들
