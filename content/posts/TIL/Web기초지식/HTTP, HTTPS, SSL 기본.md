---
title: HTTP, HTTPS, SSL 기본
date: "2021-02-08T01:04:51.444Z"
template: "post"
draft: false
slug: "HTTP, HTTPS, SSL 기본"
category: "Web"
tags:
    - "www"
    - "http"
    - "https"
    - "ssl"
description: "HTTP, HTTPS, SSL 기본"
---

# Web 기본 지식 (www, http 등)

### www

### web server

- Apache, Nginx 등
- 아파치, 톰캣

### HTTP

정의
- 어플리케이션 프로토콜
- 하이퍼텍스트 전송 프로토콜(HTTP)은 HTML과 같은 하이퍼미디어 문서를 전송

특징
- 클라이언트-서버 모델
  - HTTP는 클라이언트가 요청을 생성하기 위한 연결을 연다음 응답을 받을때 까지 대기
- connectionless
  - 서버가 응답을 마치면 연결이 끊긴다.
- stateless
  - 서버가 두 요청간에 어떠한 데이터(상태)도 유지하지 않음을 의미
- TCP/IP 레이어를 기반

### HTTP 쿠키

서버가 사용자의 웹 브라우저에 전송하는 데이터 조각  
브라우저는 이 데이터 조각들을 저장할 수 있고 다음 요청시에 전송할 수 있으며, 방문자의 상태를 저장하는 용도로 사용  
