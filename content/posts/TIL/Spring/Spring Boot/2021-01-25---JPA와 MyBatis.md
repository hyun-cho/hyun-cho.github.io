---
title: JPA와 MyBatis
date: "2021-01-25T07:37:39.351Z"
template: "post"
draft: false
slug: "JPA와 MyBatis"
category: "Spring Boot"
tags:
  - "Spring Data JPA"
  - "JDBC"
  - "Persitence Context"
  - "Hibernate"
  - "MyBatis"
description: "JPA와 MyBatis의 개념 및 장단점 비교"
---

# JPA와 MyBatis

## 영속성 (Persistence)

JDBC를 이해하기 위해서는 영속성이라는 개념을 먼저 이해해야 한다.  

영속성이란
- 데이터를 생성한 프로그램이 종료되더라도 사라지지 않는 데이터의 특성
- 영속성을 갖지 않는 데이터는 메모리에만 존재하기 때문에 프로그램을 종료하면 휘발된다.
- 파일 시스템, 관계형 데이터베이스 혹은 객체 데이터베이스 등을 활용해 데이터를 영구하게 저장해 영속성을 부여

영속성 계층(Persistence Layer)
- 프로그램의 아키텍처에서 데이터에 영속성을 부여해주는 계층
- JDBC를 이용해 직접 구현이 가능하지만, Persistence framework를 이용한 개발이 많이 이루어짐

![영속성 계층](https://gmlwjd9405.github.io/images/spring-framework/spring-jdbc-layer.png)
- 프레젠테이션 계층 (Presentation Layer) - UI 계층(UI layer)
- 애플리케이션 계층 (Application Layer) - 서비스 계층(Service layer)
- 비즈니스 논리 계층 (Business logic Layer) - 도메인 계층(Domain layer)
- 데이터 접근 계층 (Data access Layer) - **영속 계층(Persistence Layer)**

Persistence Framework
- 간단한 작업만으로 DB와 연동되는 시스템을 개발 가능
- SQL Mapper와 ORM 으로 구분 가능
  - JPA, Hibernate, Mybatis 등

---

## SQL Mapperdhk ORM

ORM은 데이터베이스 객체를 자바 객체로 매핑함으로써 객체간의 관계를 바탕으로 SQL을 자동생성 해주지만 SQL Mapper는 SQL을 명시  
ORM은 관계형 데이터베이스의 관계를 Object에 반영하자는 것이 목적이라면, SQL Mapper는 단순히 필드를 매핑시키는 것이 목적  

### SQL Mapper

SQL <--매핑--> Object 필드
SQL Mapper는 SQL 문장으로 직접 데이터베이스 데이터를 다룸
- SQL Mapper는 SQL을 명시
- Ex) Mybatis, JdbcTemplates


### ORM(Object-Relational Mapping), 객체-관계 매핑

데이터베이스 데이터 <--매핑--> Object 필드
- 객체를 통해 간접적으로 데이터베이스 데이터를 다룸

객체와 관계형 데이터베이스의 데이터를 자동으로 매핑(연결) 해주는 것을 말한다.
- ORM을 이요하면 SQL Query가 아닌 직관적인 코드(메서드)로 데이터를 조작가능
- 객체 간의 관계를 바탕으로 SQL을 자동으로 생성 가능

Persistence API
- JPA, Hibernate

---

## JPA(Java Persistent API)

자바 ORM 기술에 대한 API 표준 명세
- JAVA SE, JAVA 플랫폼 EE를 사용하는 응용프로그램에서 RDB의 관리를 표현하는 자바 API
- JPA는 ORM을 사용하기 위한 표준 인터페이스를 모아둔 것
- 기존 EJB에서 제공되던 엔티티 빈(Entity Bean)을 대체하는 기술

JPA 구성요소 3가지
1. `javax.persistence` 에서 제공하는 모든 것
2. JPQL (Java Persistence Query Language)
3. 객체/관계 메타데이터

사용자가 원하는 JPA 구현체를 선택 가능
- JPA의 구현체로 Hibernate, EclipseLink, DataNucleus, OpenJPA, TopLink Essentials등이 존재
- 이들을 ORM Framework라고 부름

---

## Hibernate

JPA의 구현체 중 하나  
SQL을 직접 사용하지 않는다고 해서 JDBC API를 사용하지 않는다는 것은 아님  
- 지원하는 메서드 내부에서는 JDBC API가 동작하고 있음, 단지 개발자가 SQL을 직접 작성하지 않을 뿐

HQL(Hibernate Query Language) 라는 강력한 쿼리 언어 포함
- SQL과 비슷하며, 추가적인 컨벤션 정의 가능
- 완전히 객체 지향적이며 이로써 상속, 다형성, 관계등의 객체지향 강점을 누릴 수 있다.
- 자바 클래스와 프로퍼티의 이름을 제외하고는 대소문자 구분
- 쿼리 결과로 객체를 반환하며 프로그래머에 의해 생성되고 직접적으로 접근 가능
- SQL에서는 지원하지 않는 pagenation, dynamic profiling 같은 향상된 기능 제공
- 여러 테이블을 작업할 때 명시적인 join을 요구하지 않음

장점
- 객체지향적으로 데이터를 관리, 비즈니스 로직에 집중 가능, 객체지향적 개발 가능
- 테이블 생성, 변경, 관리가 쉽다
- 로직을 쿼리에 집중하기 보단 객체 자체에 집중 가능
- 빠른 개발이 가능

단점
- 어렵다 (많은 내용이 감춰져있다.)
- 잘 이해하고 쓰지 않으면 데이터 손실이 있을 수 있다. (persistence context)
- 성능상 문제가 있을 수 있다.

---

## Mybatis

개발자가 지정한 SQL, 저장 프로시저 그리고 몇 가지 고급 매핑을 지원하는 SQL Mapper  
JDBC로 처리하는 상당 부분의 코드와 파라미터 설정 및 결과 매핑을 대신해준다.
- 기존에 JDBC를 사용할 때는 DB와 관련된 여러 복잡한 설정(Connection)등을 다루어야하나, SQL Mapper는 자바 객체를 실제 SQL문제 연결해, 빠른 개발과 편리한 테스트 환경을 얻음  

데이터베이스 레코드에 원시 타입과 Map 인터페이스, 그리고 자바 POJO를 설정해 매핑하기 위해 xml과 Annotation 사용 가능  
MyBatis는 원래 Apache Foundation의 iBatis였으나 생산성, 개발 프로세스, 커뮤니티 등의 이유로 Google Code로 이전되면서 이름이 바뀜  

장점
- SQL에 대한 모든 컨트롤을 하고자 할 때 매우 적합
- SQL쿼리들이 매우 잘 최적화되어 있을 때 유용

단점
- 어플리케이션과 데이터베이스 간의 설계에 대한 모든 조작을 하고자 할 때는 적합하지 않음
  - 서로 잘 구조화 되도록 많은 설정이 바뀌어야 하기 때문