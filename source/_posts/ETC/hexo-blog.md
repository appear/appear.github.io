---
title: Hexo Blog 로 블로그 만들기
date: 2018-12-09 11:01:38
tags: Etc
---

# Blog 

## 블로그 만들어보기 

### Github repo 만들기 

repo 의 이름은 [github 이름].github.io 로 만들어야합니다.   
이름만 지정해주면 됩니다. 다른 사항들은 체크하지 말아주세요 :)  

![hexo](/img/hexo/hexo01.png)

![hexo](/img/hexo/hexo03.png)

### Hexo 설치하기 
[Hexo](https://hexo.io/ko/index.html) 는 node 기반의 static generator 입니다. 작성한 md 파일을 html 파일로 변환해줍니다.

![hexo](/img/hexo/hexo02.png)

먼저 Hexo cli 를 설치해야 합니다.

```text
npm install hexo-cli -g
```

blog 를 설치할 경로로 이동하여 hexo init 을 실행하여 hexo 프로젝트를 생성해야 합니다. 프로젝트 이름은 위에서 만든 github repo 의 이름과 같으면 좋습니다.  

```text
hexo init [github 이름].github.io
```

아래와 같이 프로젝트가 생성된 것을 볼 수 있습니다. 

![hexo](/img/hexo/hexo04.png)

IDE 로 프로젝트를 열어주세요. 프로젝트로 이동한 후 모듈을 설치해 주기 위해 아래 명령어를 입력해주세요   

```text
npm install 
```

![hexo](/img/hexo/hexo05.png)

모듈이 설치 된 후 local 환경에 블로그를 띄워보겠습니다.

```text
hexo serve
```

localhost:4000 으로 접속해주세요

![hexo](/img/hexo/hexo06.png)

#### 테마 변경하기 
기본 테마가 너무 이쁘지 않기 때문에 ... [Vue 테마](https://github.com/yanm1ng/hexo-theme-vexo)로 변경해보려고 합니다.

![hexo](/img/hexo/hexo07.png)

프로젝트 폴더안에서 vexo 테마를 git 에서 pull 해야합니다. 

```text
git clone https://github.com/yanm1ng/hexo-theme-vexo.git themes/vexo
```

받은 후에는 아래와 같은 구조가 됩니다.   

```text
- 프로젝트 폴더
  - themes
    - vexo 
```

![hexo](/img/hexo/hexo08.png)

기존의 landscape 테마는 필요가 없기 때문에 삭제해주시면 됩니다. 
그 후 프로젝트 폴더의 `_config.yml` 파일을 수정해줘야 합니다. 

```text
theme: vexo
```

![hexo](/img/hexo/hexo09.png)

다시 hexo 를 실행해보면 테마가 변경된 것을 확인할 수 있습니다.

```text
hexo serve
```

![hexo](/img/hexo/hexo10.png)

vexo/_source 안에있는 about, project, tags 폴더를 우리 프로젝트의 source 안으로 복사해서 붙여넣기 해주세요 :) 

![hexo](/img/hexo/hexo18.png)

#### 글 써보기 
블로그를 만들었으니 글을 작성해보도록 하겠습니다. 아래 명령어를 통해 글 작성이 가능합니다. 

```text
hexo new post 파일이름 
```

작성한 글은 프로젝트 폴더 `/source/_posts` 폴더에 생성됩니다. md 파일의 구조는 아래와 같습니다.

```text
title: javascript  
date: 2018-12-06 21:06:57
tags:
```

작성한 글을 다음처럼 보여집니다.

#### Main
![hexo](/img/hexo/hexo16.png)

#### detail
![hexo](/img/hexo/hexo17.png)

#### 블로그 정보 변경하기
프로젝트의 `_config.yml` 파일에서 사이트에 대한 여러정보를 수정할 수 있어요 

![hexo](/img/hexo/hexo12.png)

정보를 변경하면 아래 사진처럼 정보가 바뀝니다. 

![hexo](/img/hexo/hexo13.png)

프로젝트의 `_config.yml` 은 밖에서 보여지는 페이지에 대한 정보입니다.  
`themes/vexo/_config.yml` 은 블로그 동작에 대한 설정 정보를 다룹니다.

블로그의 폴더 구조와 카테고리 분류 댓글 플러그인 추가 등등 .. 여러 설정을 이곳에서 하게됩니다.

![hexo](/img/hexo/hexo14.png)

지금 About 페이지를 들어가게되면 테마를 만드신분의 얼굴이 보여집니다. config 파일을 수정해서 사진을 변경해보겠습니다. 

![hexo](/img/hexo/hexo19.png)

```text
/vexo/_config.yml

about: 
  banner: https://img1.daumcdn.net/thumb/R720x0.q80/?scode=mtistory&fname=http%3A%2F%2Fcfile6.uf.tistory.com%2Fimage%2F99FF384F5A4CCB0133669D
  avatar: http://image.news1.kr/system/photos/2014/2/7/752089/article.jpg
  description: "안녕하세요 olaf 입니다."
  weibo_username:
  twitter_username:
  github_username:
  zhihu_username:
  douban_username:
  linkedin_username:
```

![hexo](/img/hexo/hexo20.png)

이번에는 카테고리 링크 이동을 변경해보겠습니다. 

지금은 Home, Tags, Archives, Projects, About 등이 있지만 제가 필요한것은 Home, Javascript, About 뿐입니다.   
`/vexo/_config.yml` 파일에서 path 를 설정할 수 있습니다.   

```text
menu:
 Home: /
 Javascript: /tags/#javascript/
 About: /about/
```

위 처럼 메뉴구성을 변경해주면 메뉴가 아래와 같이 변경됩니다.
 
![hexo](/img/hexo/hexo21.png)

path 를 설정한 후 위에서 생성한 md 파일의 tags 를 javascript 로 변경해주세요

```text
title: javascript
date: 2018-12-06 21:06:57
tags: javascript
```

변경 후 Javascript 탭을 클릭해보시면 태그들을 모아놓은 페이지로 이동되고 #javascript 로 포커싱이 됩니다.

```text
_posts
  JAVASCRIPT 
    자바스크립트 관련 포스트
  REACT
    리액트 관련 포스트  
```

저는 포스트들을 모아놓고 보기 위해 아래와 같은 폴더 구조를 사용합니다. _posts 안에 모두 넣어도 괜찮지만 나중에 글이 많아지면 보기 힘들어져서
카테고리별로 폴더를 모아놓습니다.

![hexo](/img/hexo/hexo23.png)

#### ETC 
이미 기본으로 세팅되어있는 중국어들과 ... 사용하지 않을 댓글 플러그인 등등 
우리가 필요하지 않은 것들이 여기 저기 사용되어져 있어 제거해보고자 합니다.

예시로 tags 페이지의 标签检索 중국어를 제거해보겠습니다.  

![hexo](/img/hexo/hexo23.png)

vexo 테마는 vexo/layout 안에 있는 ejs 를 바탕으로 md 파일과 결합되어 빌드 되어집니다.  
그래서 우리는 ejs 파일을 수정하므로써 이런 문제를 해결할 수 있습니다.

![hexo](/img/hexo/hexo24.png)

```html
 <p class="post-date">标签检索</p>
```

이 부분을 삭제하거나 원하는 텍스트로 변경하면 됩니다. 이처럼 중국어로 되어 있는 부분들을 ejs 파일에서 수정하여 처리할 수 있습니다.

아래 사진에 보이는 사용하지 않을 플러그인들은 어떻게 제거할까요 ?  이 또한 마찬가지 입니다. ejs 파일에서 해당 부분을 제거해주면 됩니다.  

![hexo](/img/hexo/hexo25.png)

직접 html 을 제거하는 방법도 있고  `_config.yml` 에서 설정을 변경하여 활성화 시키지 않는 방법도 있습니다. 

```text
# catalog
catalog: false
# donate
donate: false
# qrcode
qrcode: false
# comment
# you can set gitment, uyan or disqus
comment: gitment
```

#### Github 배포하기
마지막으로 완성된 블로그를 배포할 일만 남았습니다.   
일단 만든 프로젝트 폴더를 우리의 github 과 연결해주어야합니다.

프로젝트 폴더에서 아래 명령어를 입력해주세요 

```text
git inti 
git add .
git remote add origin https://github.com/olafjs/olafjs.github.io.git
git checkout -b build 
git commit -m "feat: setup build project"
git push origin build
```

master 로 올리지 않고 build branch 올린 이유는 md 파일과 이미지 파일들을 백업해두기 위해서입니다.  
hexo 를 이용해서 build 를 하게되면 md 파일들이 html 파일로 변경되어져 올라가게 되는데 이 과정에서 md 파일들이 손실되기 때문입니다.  
물론 우리의 로컬 컴퓨터에는 md 파일이 남겨져 있습니다.  
하지만 실수로 로컬 컴퓨터에서 md 파일들을 지워버리면 복구할 길이 없습니다. (제가 이래서 글을 한번 다 날려먹어서 ..)   
백업용 branch 를 두고 여기서 블로그 포스팅에 대한 작업을 합니다.  

build 를 하게되면 자동으로 html 로 변환된 파일들은 master branch 로 올라가게됩니다.  
우리는 master branch 에서 어떤일이 일어나든 신경쓰지 않아도됩니다.   
build branch 만 관리해주면 됩니다. 

push 를 하시고 github 페이지에서 잘 올라갔는지 확인해주세요 :) 

![hexo](/img/hexo/hexo26.png) 

백업용 파일들은 올려두었으니 블로그를 배포해보도록 하겠습니다. 

github 으로 배포를 하기 위해서는 `hexo-deployer-git` 모듈이 필요합니다.

```
npm install hexo-deployer-git --save
```

설치 후 프로젝트 폴더의 `_config.yml` 파일의 deploy 를 수정해주세요

![hexo](/img/hexo/hexo11.png)

```text
deploy:
  type: git
  repo: https://github.com/olafjs/olafjs.github.io.git
```

배포 명령어는 아래와 같습니다. 

```text
hexo clean && hexo generate && hexo deploy
```









