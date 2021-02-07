---
title: 스프링 MVC와 Thymeleaf
date: "2021-01-19T23:46:37.121Z"
template: "post"
draft: false
slug: "스프링 MVC와 Thymeleaf"
category: "코드로-배우는-스프링부트"
tags:
  - "코드로-배우는-스프링부트"
  - "Spring"
  - "Spring Boot"
  - "MariaDB"
  - "Thymeleaf"
  - "MVC"
description: "코드로 배우는 스프링부트 part1/chapter3 스프링 MVC와 Thymeleaf"
---

## Thymeleaf 정리

- 반복문 처리
- `th:each`라는 속성 사용

```html
th:each="변수: ${목록}"
```

- 반복문의 상태(state) 객체
  - 부가적으로 상요할 수 있는 상태(state) 객체가 존재를순번이나 인덱스 번호, 홀짝 등을 지정 가능


- 제어문 처리
  - `th:if ~ unless` 등을 이용, 삼항연산자 또한 가능
  - `th:if, th:unless`, 다른 언어는 `if ~ else` 구문이지만, Thymeleaf는 단독으로 처리함

- inline 속성
  - 주로 javaScript 처리에서 유용.

  ```java
  @GetMapping({"/exInline"})
    public String exInline(RedirectAttributes redirectAttributes) {
        log.info("exInline.....");

        SampleDTO dto = SampleDTO.builder()
                .sno(100L)
                .first("Frist....100")
                .last("Last....100")
                .regTime(LocalDateTime.now())
                .build();
        redirectAttributes.addFlashAttribute("result", "success");
        redirectAttributes.addFlashAttribute("dto", dto);

        return "redirect:/sample/ex3";
    }

    @GetMapping("/ex3")
    public void ex3() {
        log.info("ex3");
    }
  ```

  - exInline()은 내부적으로 RedirectAttributes를 이용하여 '/ex3'으로 result와 dto라는 이름의 데이터를 심어서 전달.
  - dto는 SampleDTO 객체
  - javascript 생성부분을 보면
    - 문자열은 자동으로 " "이 추가되어 문자열이 됨
    - 객체는 자동으로 JSON gudtlrdmfh qusghks

- `th:block` 기능
  - 별도의 태그가 필요 없기 때문에 제약이 없다.
  - 실제 화면에서는 html로 처리 되지 않는다.
  - 루프 등을 별도로 처리하는 용도로 많이 사용

- 링크 처리
  - `@{}`를 사용
  ```html
    <li th:each="dto : ${list}">
        <a th:href="@{/sample/exView(sno=${dto.sno})}">[[${dto}]]</a>
    </li>

    생성 후
    <a href="/sample/exView?sno=1">
  ```
  - sno를 path로 사용하고 싶다면 다음과 같이 사용

  ```html
    <li th:each="dto : ${list}">
        <a th:href="@{/sample/exView/{sno}(sno=${dto.sno})}">[[${dto}]]</a>
    </li>

    생성 후
    <a href="/sample/exView/3">
  ```


## Thymeleaf의 기본 객체와 LocalDateTime

- 다양한 기본 객체 지원
  - 문자 숫자 파라미터 request, response, session 등
  - `#numbers`, `#dates`등을 설정없이 사용 가능
  - 숫자 포맷팅을 아래와 같이 사용가능

```html
<ul>
    <li th:each="dto : ${list}">
        [[${#numbers.formatInteger(dto.sno, 5)}]]
    </li>
</ul>
```

- Java 8+ 버전에 LocalDate 타입이나 LocalDateTime에 대해서 복잡하게 처리해야된다.
- `thymeleaf-extras-java8time`을 사용하면 더 편리하게 사용가능
- 실제에서는 `#temporals`라는 객체를 사용해서 format()으로 처리

## Thymeleaf의 레이아웃

- 2가지 형태로 사용가능
  - JSP의 include와 같이 특정 부분을 외부 혹은 내부에서 가져와서 포함하는 형태
  - 특정한 부분을 파라미터로 전달해서 내용에 포함하는 형태

### include 방식의 처리

- `th:insert`나 `th:replace`기능 - 특정한 부분을 다른 내용으로 변경
  - reaplce의 경우 기존의 내용응 완전히 대체
  - insert 방식은 기존 내용의 바깥쪽 태그는 그대로 유지하면서 추가되는 방식
 
- `::` 뒤에는 fragment의 이름을 지정하거나 CSS의 #id와 같은 선택자를 이용할 수 있다.
  - 만약 `::`를 생략하면 파일 전체내용을 가져온다.

- 파라미터 방식의 처리
  - 특정한 태그를 파라미터 처럼 전달해서 다른 th:fragment에서 사용 할 수 있다.

  ```html
    <body>
        <div th:fragment="target(first, second)">
            <style>
                .c1 {
                    background-color: red;
                }
                .c2 {
                    background-color: blue;
                }
            </style>

            <div class="c1">
                <th:block th:replace="${first}"></th:block>
            </div>

            <div class="c2">
                <th:block th:replace="${second}"></th:block>
            </div>
        </div>
    </body>
  ```
  
  - 선언된 target 부분에는 first와 second라는 파라미터를 받을 수 있도록 구성
  - 또한 내부적으로 th:block으로 이를 표현

  ```html
    <th:block th:replace="~{/fragment/fragment3:: target(~{this:: #ulFirst}, ~{this::#ulSecond} )}">
        <ul id="ulFirst">
            <li>AAA</li>
            <li>BBB</li>
            <li>CCC</li>
        </ul>

        <ul id="ulSecond">
            <li>111</li>
            <li>222</li>
            <li>333</li>
        </ul>
    </th:block>
  ```

  - 화면 구성 기능 대신 target을 사용할 때 파라미터를 2개 사용
    - this: #ulFirst - this는 현재 페이지를 의미, 생략 가능 / #ulFirst는 CSS의 id 선택자

### layout 템플릿 만들기

- layout 폴더에 layout을 만들고, content 영역에 fragment를 삽입

### 부트스트랩 템플릿 적용하기

- 부트스트랩 템플릿을 가져와서 레이아웃 설정해보기
