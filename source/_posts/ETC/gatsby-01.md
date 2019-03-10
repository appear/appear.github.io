---
title: Gatsby + Netlify 를 이용하여 static page 배포하기
date: 2019-03-10 13:55:50
tags: Gatsby
---

# Gatsby

Gatsby 는 React.js , React-Router, [static-site-generator-webpack-plugin](https://github.com/markdalgleish/static-site-generator-webpack-plugin) 묶어 React 기반으로 static page 를 만들 수 있는 generator 입니다.

## Gatsby Project 생성

[gatsby-starter-default](https://github.com/gatsbyjs/gatsby-starter-default) 을 이용하여 프로젝트를 생성합니다.

gatsby 프로젝트 생성을 하기 위해서는 `gatsby-cli` 를 설치해야 합니다.

```bash
$ npm i gatsby-cli
$ npx gatsby new gatsby-project https://github.com/gatsbyjs/gatsby-starter-default
```

아래와 같은 프로젝트가 생성됩니다.

![gatsby](/img/etc/gatsby01.png)

다음 명령어를 실행하여 dev-server 를 실행합니다.

```bash
$ gatsby develop
```

![gatsby](/img/etc/gatsby02.png)

## Git Repository 생성

우리가 만든 gatsby 프로젝트를 올릴 repo 를 생성해야 합니다.

![gatsby](/img/etc/gatsby03.png)

만들어진 repo 에 우리가 만든 프로젝트를 올려보겠습니다.

```bash
$ git init
$ git add .
$ git commit -m "gatsby project setup"
$ git remote add origin [본인 repo url]
$ git push origin master
```

![gatsby](/img/etc/gatsby04.png)

## Netlify

공식 홈페이지에서는 Netlify는 현대의 웹 프로젝트를 배포, 지속적인 통합, 자동 HTTPS 결합 등 올인원 워크 플로라고 설명합니다.
Netlify 를 이용하게되면 우리가 만든 gatsby 프로젝트를 아주 간단하게 배포할 수 있습니다.

[Netlify](https://www.netlify.com/) 홈페이지로 이동하여 github 계정으로 로그인해주세요

New site from git 버튼을 클릭해주세요

![gatsby](/img/etc/gatsby05.png)

Continuous Deployment > github 을 선택해주세요

![gatsby](/img/etc/gatsby06.png)

Repository access > All repositories 를 선택해주세요

![gatsby](/img/etc/gatsby07.png)

우리가 만들어 놓은 `gatsby-project` 를 선택해주세요

![gatsby](/img/etc/gatsby08.png)

gatsby 를 build 하면 public 폴더 안으로 빌드된 파일들이 들어가집니다.

Deploy site 버튼을 눌러주세요

![gatsby](/img/etc/gatsby09.png)

Deploy site 버튼을 누르면 배포가 시작됩니다.

![gatsby](/img/etc/gatsby10.png)

배포가 완료되면 아래와 같이 배포된 주소를 알려줍니다. 저의 경우 https://nifty-leakey-5d0f7a.netlify.com/ 이런 주소를 할당 받았습니다.

![gatsby](/img/etc/gatsby11.png)

할당 받은 주소로 접속시 아래와 같은 화면을 볼 수 있습니다. 이 처럼 Gatsby 와 Netlify 를 이용하면 아주 간단하게 static page 를 배포를 할 수 있습니다.

![gatsby](/img/etc/gatsby12.png)
