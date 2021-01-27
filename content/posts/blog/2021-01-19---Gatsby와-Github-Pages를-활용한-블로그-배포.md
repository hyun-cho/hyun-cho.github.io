---
title: Gatsby와 Github Pages를 활용한 블로그 배포
date: "2021-01-19T23:46:37.121Z"
template: "post"
draft: false
slug: "blog/Gatsby와-Github-Pages를-활용한-블로그-배포"
category: "Gatsby"
tags:
  - "Gatsby"
  - "Github Pages"
description: "개츠비를 활용한 블로그 배포"
---

## 회사 첫 업무로 블로그 생성하기

취업 준비 할 때 Github Pages를 활용하여 포트폴리오를 만든 경험이 있기 때문에, 가지고 있던 정말 작은 지식은,
> 정적 페이지를 깃허브에 등록하고, Github Pages에 브랜치를 등록하면 자동으로 배포가 된다!  

라는 것이었다.  

근데 정적페이지에서 html을 수정하는 방식은 한계가 있고, 기존에 널리 사용되던 블로그 배포방식인 Jekyll themes는 몇 가지 문제가 있었다.
- 블로그에 여러 기능을 추가해야한다. (facebook, 댓글 등)
- Jekyll을 사용하기에는 나는 Ruby언어를 모른다.

따라서 문제점을 해결해보기위해 구글링하다가 React기반의 정적페이지 생성기인 Gatsby JS에 대해 알게 되었다.

## Gatsby JS

Gatsby JS 공식 홈페이지에는 다음과 같이 소개되어있다.

> Gatsby is a React-based open-source framework for creating websites and apps. It's great whether you're building a portfolio site or blog, or a high-traffic e-commerce store or company homepage.

요약하자면 **리액트를 기반으로하는 웹사이트/앱을 만드는 프레임워크** 라고 얘기할 수 있겠다.  
리액트 컴포넌트를 기반으로 뷰를 만들고, GraphQL을 통해서 정적 컨텐츠를 가져오는 방식의 정적 사이트 생성기이다.  
데이터 소스는 마크다운 파일일 수도, CMS인 워드프레스 등 다양한 플러그인을 활용해서 사용할 수 있다.

GraphQL은 나도 공부가 많이 필요한 영역이라, 일단 글을 쓸 수 있게만 만들어놓고 차근차근 공부하면서 업그레이드 할 생각이다.

개발은 다음과 같은 과정을 통해서 이루어 질 예정이다.

- Gatsby를 통해 페이지를 개발 한 뒤
- 정적페이지를 빌드해서 Github Pages에 배포

위 두가지 과정을 걸쳐서 블로그를 개발해보자.

## Gatsby 설정

Macbook Pro에서 개발하고있고 OS X 를 사용하고 있으므로 더욱 쉽게 블로그를 사용할 수 있다.

[Gatsby Quick-Start](https://www.gatsbyjs.org/docs/quick-start)를 보고 cli를 설치하자.

```bash
npm install -g gatsby-cli
```

Gatsby를 설치한 뒤 여러가지 스타터 중 하나를 선택해서 시작할 예정인데,   
나는 참조한 블로그에서 사용한 minimal-blog를 사용하여 개발하려고 한다.

~~다른 gatsby 블로그 테마를 가져와도 된다.~~

```bash
gatsby new minimal-blog LekoArts/gatsby-starter-minimal-blog
    # gatsby new <프로젝트이름> <스타터url>
```

여기까지 `minimal-blog`라는 이름의 폴더가 만들어지고, `gatsby develop`을 입력하면 로컬호스트에 dev환경이 만들어진다.
> [http://localhost:8000/](http://localhost:8000/)

참조한 블로그에서는 Gatsby에서 사용하는 쉐도윙이라는 기법으로 커스터마이징을 바로 시작했지만,
처음 사용해보는 나에게는 큰 장벽..이어서 나중에 적용해보도록 하고 이어서 배포하는 방법을 소개하려고한다.

## 배포 설정

배포 방식에는 두가지 방식이 있다.
- Github Repository를 netlify.com을 사용해서 간단하게 배포하기
- gatsby gh-pages를 사용해서 배포하기

전자의 경우에는 많은 자료가 있으니 찾아서 해보면 좋을 것 같고, 더 간편하다고 소개한다.  
대신 홈페이지가 netlify.app으로 나온다는점?  
나는 새로운 걸 너무 많이써서 github 내에서 해결할 수 있는 후자로 배포하기로 했다.

### gh-pages를 활용한 배포

`gh-pages`는 빌드와 동시에 깃허브에 정적파일을 배포하는 형식이다.
기존의 gatsby 프로젝트는 public안에 index.html이 있고, README.md가 둘다 존재해 개발 브랜치를 기준으로 배포하게되면 README.md 만 보이게 된다.
이를 정적페이지로 빌드해서 배포하면 index.html로 화면을 보여주게 된다. ~~정확하지 않을 수 있습니다.~~

gatsby에 gh-pages를 설치하자.

```bash
npm i gh-pages -D
```

설치가 끝났으면 배포 설정을 해줘야한다.

일단 생성한 gatsby 프로젝트를 개발용 브랜치로 바꾸도록하자.

```bash
git branch -m develop
```

gatsby 프로젝트 설정파일인 `package.json`의 `script`부분에 다음부분을 추가한다.

```json
"scripts": {
    "deploy": "gatsby build && gh-pages -d public -b master"
  },
```

해당 내용은 개발용 브랜치에서 `npm run deploy`를 하면 gh-pages가 실행되서, master branch에 빌드가 완료된 배포 직전의 정보가 push 되는 내용이다.

여기까지 실행을 했다면, `minimal-blog`가 빌드된 정적 페이지가 `master` branch에 저장되었을 것이다.  
이제 github repository로 돌아가 Github Pages를 master branch로 연결만 해주면 정상적으로 배포 된다.

이제 gatsby develop 브랜치로 돌아와서 하나씩 수정하면서 커스터마이징하면 된다.  

아래 블로그 링크를 보고 하나 씩 따라한 내용이니 참고하면 좋을 것 같다.  
쉐도윙에 대한 내용도 하단부에 언급되어 있으니 관심 있으신분은 따라해보면 좋을 것 같다.
[블로그 링크](https://juneyr.dev/jekyll-to-gatsby-%EB%B8%94%EB%A1%9C%EA%B7%B8-%F0%9F%91%A9%E2%80%8D%F0%9F%94%A7)

## 후기

배포한 홈페이지는 [hyun-cho.github.io](hyun-cho.github.io)에서 확인할 수 있다.

내가 배포한 방식을 간략하게 (생략한 부분이 정말 많다..) 적었는데 중간과정을 다 날려버린것 같아서 뭔가... 글이 아쉽다.  
좀 더 글쓰기 연습을 해야 할 것 같은 느낌