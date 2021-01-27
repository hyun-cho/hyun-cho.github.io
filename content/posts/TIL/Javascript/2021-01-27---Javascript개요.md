---
title: Javascript개요
date: "2021-01-27T02:30:44.175Z"
template: "post"
draft: false
slug: "Javascript개요"
category: "Javascript"
tags:
  - "Javascript"
  - "ECMAscript"
description: "Javscript 개요"
---

# Javascript의 표준화

크로스 브라우징 이슈(MS의 익스플로러..)로 인한 표준화된 언어의 개발이 필요해짐

|버전|출시 연도|특징|
|----|:--:|----|
|ES1|1997|초판|
|ES2|1998|ISO/IEC 16262 국제 표준과 동일한 규격 사용|
|ES3|1999|정규 표현식, try... catch|
|ES5|2009|HTML5와 함께 출현 JSON, strict mod, 접근자 프로퍼티, 어트리뷰트 제어, 향상된 배열 조작기능(forEach, map, filter, reduce, some, every|
|ES6(ECMAscript 2015)|2015|let/const, 클래스, 화살표함수, 템플릿 리터럴, 디스트럭처링 할당, 스프레드 문법, rest 파라미터, 심벌, 프로미스, Map/Set, 이터러블, for...of, 제너레이터, Proxy, 모듈 import/export|
|ES7(ECMAscript 2016)|2016|지수(**) 연산자, Array.prototype.includes, String.prototype.includes|
|ES8(ECMAscript 2017)|2017|async/await, Objeect 정적 메서드(Object.values, Obejct.entries, Object.getOwnPropertyDescriptors|
|ES9(ECMAscript 2018)|2018|Obejct rest/spread 프로퍼티, Promise.prototype.finally, async.generator, for await... of|
|ES10(ECMAscript 2019)|2019|Objeect.fromEntries, Array.prototype.flat, Array.prototype.flatMap, optional catch binding|
|ES11(ECMAscript 2020)|2020|String.prototype.matchAll, BigInt, globalThis, Promise.allSettled, null 병합 연산자, 옵셔널 체이닝 연산자, for ... in enumertion order|
|ES12(ECMAscript 2021|2021|Optional Chaining(?.), Nulish coalescing operation(??), replaceAll, Private visibility modifier, WeakRefs, Finalizers, Promise.any|

<br><br><br><br>


# Javascript의 역사

## Ajax

JS와 비동기 방식으로 데이터를 교환할 수 있는 Ajax가 XMLHttpRequest라는 이름으로 등장했다.  

## jQuery

DOM을 더욱 쉽게 제어할 수 있는 수단으로 나왔다.  쉽고 직관적인 jQuery를 더 선호하는 개발자가 양산되기도 했다.  

## V8 자바스크립트 엔진

비동기 처리로 인한 더 성능 좋은 엔진이 필요해졌고, V8 엔진이 이를 대체했다, 과거 웹 서버에서 수행되던 로직들이 대거 클라이언트(브라우저)로 이동했고, 이는 웹 애플리케이션 개발에서 FE가 주목받게 되는 계기가 되었다.  

## Node.js

2009년 V8 JS 엔진으로 빌드된 JS 런타임 환경이다.  
Node.js는 브라우저의 자바스크립트를 다른 환경에서도 동작할 수 있도록 자바스크립트 엔진을 브라우저에서 독립시킨 실행 환경.  
주로 서버사이드 애플리케이션 개발에 주로 사용된다.  

비동기I/O를 지원하면 단일 스레드 이벤트 루프 기반으로 동작  
이로인해 요청(Request)처리 성능이 좋고 SPA처리에 적합하다.  

## SPA 프레임워크

데스크톱 어플리케이션과 비슷한 성능과 UX를 제공하는 것이 필수가 되었고, 이를 만족시키기 위한 CBD(Component Based Development) 방법론을 기반으로 하는 SPA(Single Page Application)이 대중화 되며 Angular, Vue.js, React, Svelte등 다양한 프레임워크/라이브러리가 많아졌다.

## Javascript와 ECMAScript

Javascript는 일반적으로 뼈대를 이루는 ECMAScript와 브라우저가 별도로 지원하는 클라이언트 사이드 Web API, 즉 DOM, BOM, Canvas, XMLHttpRequest, fetch, requestAnimationFrame, SVG, Web Storage, Web Component, Web Worker를 모두 포함하는 개념
