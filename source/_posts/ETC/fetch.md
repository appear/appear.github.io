---
title: Issue. fetch polyfill
date: 2018-12-09 12:24:32
tags: Etc
---

# Issue

모바일 대상 프로모션 페이지를 만들면서 간단한 api call 을 위해 fetch 를 사용했습니다.  
모바일 대상이었기 때문에 polyfill에 크게 신경쓰지 않았었습니다.    
오픈후 Sentry 를 확인해보니 생각보다 하위 버전의 모바일 기기가 많아 fetch 를 찾지 못하는 에러가 많았습니다.  
polyfill 를 추가하여 에러 발생을 막을 수 있었습니다.    
     
![issue](/img/etc/fetch-issue.png)
[Can I use](https://caniuse.com/#search=fetch) 에서 확인 가능합니다.

### Solution
whatwg-fetch 를 설치하고 import 해주면 됩니다. 

```js
npm install whatwg-fetch --save
import 'whatwg-fetch'
```

fetch polyfill 은 [github](https://github.com/github/fetch) 에서 확인 가능합니다.   

