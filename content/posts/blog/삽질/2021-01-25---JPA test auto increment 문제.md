---
title: JPA test auto increment 문제
date: "2021-01-25T07:39:38.193Z"
template: "post"
draft: false
slug: "JPA test auto increment 문제"
category: "Spring Boot"
tags:
  - "Spring Data JPA"
  - "Test"
  - "JUnit5"
description: "JPA test auto increment 문제 발생 및 해결 과정"
---

# JPA test auto increment 문제

Test를 만들 때 삽입 > 조회 > 삭제 까지의 하나의 테스트를 통합테스트로 만들고자 했을 때 중복된 레코드를 생성, 제거하게 되면 해당 테이블의 auto increment 속성때문에 에러가 나오기 시작했다.  

다양한 해결 방법이 존재한다.
- @Before, @After를 통한 트랜잭션 처리
- Entity Manager를 통한 시작 설정 초기화

아래 참고할 만한 글을 보고 어느정도 이해한 것 같다.
- https://github.com/HomoEfficio/dev-tips/blob/master/Spring%20Data%20JPA%20%ED%85%8C%EC%8A%A4%ED%8A%B8%20%EC%8B%9C%20auto-increment%20%EB%AC%B8%EC%A0%9C.md 참조