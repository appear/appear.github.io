---
title: 5분만에 CLI 만들어보기
date: 2019-03-17 15:15:05
tags: Node
---

# 5분만에 Cli 만들어보기

cli 란 `command line interface` 의 약자입니다.
React 또는 Vue 같은 친구들을 접해보셨다면 아래와 같은 명령어를 입력하여 프로젝트를 생성해보셨을거에요

```bash
$ create-react-app my-app
$ vue create my-app
```

이렇게 command 상에서 작업을 처리하는 방식을 cli 라고합니다.

## Npm Finder

아래의 명렁어를 입력하면 npm 사이트의 해당 모듈 페이지를 띄워주려고합니다. 매번 찾아들어가는게 귀찮았거든여 ..

```bash
$ n-finder react
```

## Setup - commander

모든 작업은 node.js 기반으로 이뤄지기 때문에 [node](https://nodejs.org/ko/) 가 설치되어 있어야 합니다.

```bash
npm init -y
```

command 에서 명령어를 실행하고 다양한 option 과 값들을 가져오기 위해선 `commander` 라는 모듈이 필요합니다.

```bash
npm install --save commander
```

commander 를 이용하여 `index.js` 를 구성해보겠습니다.

```js
#!/usr/bin/env node

const program = require("commander");
const pkg = require("./package.json");

program
  .version(pkg.version)
  .arguments("<module-name>")
  .action(module => console.log(`module: ${module}`))
  .parse(process.argv);
```

아래와 같이 index.js 를 실행하며 `react` 라는 값을 넘겨주면 `module: react` 라는 값이 찍히는 것을 볼 수 있습니다.

> #!/usr/bin/env node 는 꼭 추가해주세요 나중에 배포하고 global 로 설치하여 사용할때 없으면 에러납니다.

```bash
node index.js react
```

`commander` 의 함수를 간단하게 설명드리자면

- version: `node index.js -V` 와 같이 해당 모듈의 version 을 출력할 수 있도록 합니다.
- arguments: option 없이 호출될 값 입니다.
- action: arguments 값을 입력받은 후의 동작을 정의합니다.
- parse: 어디에서 값을 입력받는지 나타냅니다.

## Setup - open

`commander` 를 이용하여 값을 받았으니 이 값을 이용하여 npm 사이트에서 해당 모듈 페이지를 `open` 하기만 하면됩니다. 브라우저를 열기 위해 `open` 모듈을 사용하겠습니다.

```bash
$ npm i --save open
```

NPM 페이지에서 모듈을 찾아가기 위한 BASE 주소는 `https://www.npmjs.com/package` 입니다.  
모듈을 찾기 위해서는 BASE URL + `/모듈이름` 즉, `https://www.npmjs.com/package/모듈이름` 입니다.

open 은 실행시 두가지의 인자를 받습니다. 첫번째는 open 할 주소와 두번째 인자는 브라우저(크롬, 파이어폭스 등...) 입니다

```js
open(url, browser);
```

저는 크롬을 이용하여 페이지를 띄워보겠습니다. `chrome` 이 아닌 `Google chrome` 으로 적어주셔야합니다.

```js
const program = require("commander");
const pkg = require("./package.json");
const open = require("open");

const BASE_NPM_URL = "https://www.npmjs.com/package";

program
  .version(pkg.version)
  .arguments("<module-name>")
  .action(module => open(`${BASE_NPM_URL}/${module}`, "Google chrome"))
  .parse(process.argv);
```

이제 실행해보시면 검색한 모듈의 페이지가 뜨는것을 볼 수 있습니다.

```bash
node index.js react
```

![npmfinder](/img/node/n-finder.gif)

## Deploy

이제 우리가 만든 cli 를 npm 에 올려야합니다.

우리가 만든 프로젝트를 git 에 올리고 시작하겠습니다. (git repository 를 만드는법은 여기서 따로 설명하지 않겠습니다)

올라간 후의 프로젝트의 구성은 [여기](https://github.com/appear/npm-finder) 를 확인해주세요

github 에 프로젝트가 올라가 있다는 가정하에 설명하겠습니다.

`.npmignore` 를 추가하여 배포에 제외될 파일을 작성해줍니다. 만약 `.npmignore` 가 없고 `.gitignore` 가 있다면 gitignore 의 규칙을 따릅니다.

```text
.DS_Store
.git*
.idea
.vscode
```

그리고 npm 에 배포하기 위해서는 user 를 등록해야합니다. npm 아이디가 없으시다면 가입해주세요 :)

```bash
$ npm adduser
```

![npmuser](/img/node/npm-user-01.png)

마지막으로 package.json 에 정보를 채워주시면 됩니다.

우리는 global cli 로 사용할거기 때문에 `bin` 을 꼭 추가해주셔야합니다. 그래야지 `nfinder react` 처럼 사용이 가능합니다.

```json
{
  "name": "npm-finder",
  "version": "1.0.0",
  "description": "This is CLI to find the module page in npm",
  "main": "index.js",
  "scripts": {},
  "bin": {
    "nfinder": "index.js"
  },
  "repository": {
    "type": "git",
    "url": "git+https://github.com/appear/npm-finder.git"
  },
  "author": {
    "name": "olaf",
    "email": "appear.ko@gmail.com",
    "url": "https://appear.github.io/"
  },
  "license": "MIT",
  "bugs": {
    "url": "https://github.com/appear/npm-finder/issues"
  },
  "homepage": "https://github.com/appear/npm-finder#readme",
  "dependencies": {
    "commander": "^2.19.0",
    "open": "0.0.5"
  },
  "keywords": ["npm", "finder", "cli"]
}
```

이제 배포 할 일만 남았습니다. `npm publish` 라는 명령어로 배포가 가능합니다.

```bash
$ npm publish
```

![npmuser](/img/node/npm-deploy.png)

## 마무리

배포후에는 npm 사이트에서 확인이 가능합니다
NPM_finder 의 경우 [여기](https://www.npmjs.com/package/npm-finder) 에서 확인이 가능합니다.

```bash
$ npm i -g npm-finder
$ nfinder react
```

간단한게 cli 를 만들어보았습니다. 필요한 용도에 따라 코드를 조금씩만 바꾸면 다양하게 사용이 가능합니다.
