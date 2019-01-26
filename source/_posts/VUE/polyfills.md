---
title: Vue Cli에서 polyfill 적용하기
date: 2019-01-26 17:55:26
tags: Vue
---

# Polyfill 이란

polyfill 이란 특정 기능이 지원되지 않는 브라우저 (IE 라던가 .. IE 라던가 .. 구현 디바이스 라던가..)를 위해 지원하는 코드를 말합니다.

Vue 또는 React 를 사용하고 있는 환경이라면 일반적으로 ES6 또는 ES7 ~ 8 과 Babel 을 사용하여 제공된 Preset 을 통하여 Transfile 로 변환된 코드로 서비스를 하게되는데  
이 과정에서 대표적으로 구형 브라우저(IE8 등)와 구형 디바이스의 웹뷰등에서 예상치 못한 에러를 겪을 수 있습니다.

`array.iterator` 가 없다던가 `promise` 가 없다던가 등등 vue-cli 에 설정되어있는 default polyfill 만으로는 한계가 옵니다.

## polyfill 적용

vue-cli 프로젝트는 default 로 `@vue/babel-preset-app` 을 사용하고 [corejs](https://github.com/zloirock/core-js) 를 포함하고 있습니다.  
사용자가 사용한 코드를 바탕으로 polyfill 를 구성하게 되는데, 이는 최소한의 번들 파일을 만들기 위함입니다.  
하지만 위처럼 기본적인 방법으로 포함되지 않은 polyfill 들은 따로 추가해야합니다.

`.babelrc` 의 @vue/app 에 명시적으로 사용할 polyfill 를 작성해주면 됩니다.  
저는 아래정도의 polyfill 추가가 필요했습니다.

```
{
  "presets": [
    ["@vue/app", {
      "polyfills": [
        "es6.symbol",
        "es6.array.from",
        "es6.array.iterator",
        "es6.promise",
        "es6.object.assign"
      ]
    }]
  ]
}
```

polyfill 스펙은 [corejs](https://github.com/zloirock/core-js) 를 참고해주세요

자세한 내용은 vue-cli 의 공식 [가이드 문서](https://cli.vuejs.org/guide/browser-compatibility.html#modern-mode)를 참고해주세요 :)
