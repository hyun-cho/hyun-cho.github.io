---
title: Maria 데이터베이스와 Spring Data JPA
date: "2021-01-19T23:46:37.121Z"
template: "post"
draft: false
slug: "Maria 데이터베이스와 Spinrg Data SPA"
category: "코드로-배우는-스프링부트"
tags:
  - "코드로-배우는-스프링부트"
  - "Spring"
  - "Spring Boot"
  - "MariaDB"
  - "Spring Data JPA"
description: "코드로 배우는 스프링부트 part1/chapter2 Maria 데이터베이스와 Spinrg Data SPA"
---

# Chapter 2 Maria 데이터베이스와 Spring Data JPA

## Maira DB?

MySQL과 거의 유사하지만 완전한 오픈소스
[MariaDB docker에 설치하기](./../../TIL/Database/MariaDB%20docker에%20설치하기.md)

## DB를 위한 스프링 부트 설정

스프링 부트에서는 `Auto Configure`이라는 자동 설정 기능이 있다.
- 특정한 라이브러리가 있다면 이에 관련된 설정을 자동으로 추가
- Spring Data JPA가 의존성으로 추가되었기 때문에, 이에 대한 설정이 자동으로 추가

구체적인 값은 직접 지정해줘야 한다.  
`build.gradle`에 mariadb 관련 구문을 추가하자
  - [maven 저장소](https://mvnrepository.com/artifact/org.mariadb.jdbc/mariadb-java-client/2.7.1)에서 최신 버전을 알아볼 수 있다.

```sh
compile group: 'org.mariadb.jdbc', name: 'mariadb-java-client', version: '2.7.1'
``` 

추가적인 데이터베이스 설정을 추가합니다.
- `application.properties`을 이용하거나 별도의 클래스 파일을 작성해 사용가능

```sh
# 데이터베이스 설정을 위한 부분, Driver, Databse 주소, user, password 등록
spring.datasource.driver-class-name=org.mariadb.jdbc.Driver
spring.datasource.url=jdbc:mariadb://localhost:3306/test_db
spring.datasource.username=username               #username
spring.datasource.password=password               #password
```

## Spring Data JPA 소개

JPA는 JAVA언어를 통해서 데이터베이스와 같은 영속 계층을 처리하고자 하는 스펙

### ORM과 JPA

ORM(Object Relational Mapping)은 객체지향과 관련
> 객체지향 패러다임을 관계형 패러다임으로 매핑해주는 개념

객체지향의 구조가 관계형 데이터베이스와 유사하다는 점에서 시장  
RDB에서는 테이블을 설계할 때 칼럼을 정의하고 칼럼에 맞는 데이터 타입을 지정해 데이터를 보관하는 틀을 만든다는 의미에서 클래스와 유사하다.  

객체지향과 관계형 데이터베이스는 유사한 특징을 가지고 있는데, 이런 특정에 기초해서
> 객체지향을 자동으로 관계형 데이터베이스에 맞게 처리 해주는 기법이 ORM이다.

JPA는 Java Persistence API의 약어로 ORM을 Java 언어에 맞게 사용하는 스펙이다.
ORM이 JPA의 더 상위 개념이며, JPA를 구현한 프레임워크 중 Spring에서는 Hibernate를 기본적으로 사용한다.

![image](https://user-images.githubusercontent.com/77606318/105649385-63d68880-5ef3-11eb-8078-e45b7b7a2996.png)
![image](https://user-images.githubusercontent.com/77606318/105649421-8f597300-5ef3-11eb-8e75-0254a8ebdc31.png)


## Entity Class와 JpaRepository

Spring Data JPA가 개발에 필요한 것은 단지 두 종류의 코드로 구현 가능
- JPA를 통해서 관리하게 되는 객체(Entity Object)를 위한 엔티티 클래스
- 엔티티 객체들을 처리하는 기능을 가진 Repository

이 중 Repository는 Spring Data JPA에서 제공하는 인터페이스로 설계

## Spring Boot JPA 설정

JPA 이용 시 생기는 여러가지 상황을 설정으로 적용

spring.jpa.hibernate.ddl-auto
- 프로젝트 실행 시 DDL을 생성할 것인지 정하는 설정
- 설정값은 create, update, create-drop, validate등등

spring.jpa.properties.hibernate.format_sql=true
- 실제 jpa 구현체인 hiberante가 동작하며 발생하는 sql을 포맷팅 하여 출력
- true, false

spring.jpa.show-sql
- jpa 처리 시에 발생하는 sql을 보여줄 것인지 결정

```sh
spring.jpa.hibernate.ddl-auto=update
spring.jpa.properties.hibernate.format_sql=true
spring.jpa.show-sql=true
```

## JpaRepository 인터페이스 구조

여러 종류의 인터페이스 기능을 통해 JPA 관련 작업을 별도의 코드 없이 처리할 수 있게 지원

![image](https://user-images.githubusercontent.com/77606318/105662393-f2a6cd80-5f12-11eb-937d-5580a0e1fdf2.png)

일반적인 기능만을 사용할 때는 `CrudRepository` 를 사용  
모든 JPA관련 기능을 사용하고 싶을 때는 `JpaRepository` 사용

실제로 Repository를 사용하기 위해서는 위 인터페이스를 상속하는 것으로 모든게 해결
- interface만 등록하면 (`MemoRepostiroy`를 등록하면) 스프링에서 자동적으로 빈(bean)으로 등록
- 내부적으로 인터페이스 타입에 맞는 객체를 생성해 빈으로 등록

기본적인 인터페이스
- insert
  - `save(Entity Object)`
- select
  - `findById(Key Type), getOne(Key Type)`
- update
  - `save(Entity Object)`
- delete
  - `deleteById(Key type), delete(Entity Object)`

### insert, update 모두 save()?

JPA 구현체가 메모리상에서 객체를 비교하고 (select 구문 1번), 없다면 insert 존재한다면 update를 동작시키는 방식으로 동작하기 때문

### findById(), getOne() 동작방식

데이터베이스를 먼저 이용하는지, 나중에 필요한 순간까지 미루는지에 대한 차이  

findById()
- Java.util 패키지의 `Optional` 타입으로 반환되기 때문에 한번 더 결과가 존재하는지 체크하는 형태로 작성
- 실행 순간에 이미 SQL처리는 완료되었고, 결과에 대해서는 이후에 처리

getOne()
- `@Transactional` 어노테이션이 추가로 필요
- 반환 값은 해당 객체지만, 실제 객체가 필요한 순간까지 SQL을 실행하지 않고 보류

## chapter 2.5 페이징 / 정렬 처리

페이징 처리와 정렬을 위해 오라클은 inline view를, mysql은 limit을 알아야 한다.  
JPA는 내부적으로 이런 처리를 'Dialect'라는 존재를 이용해서 처리를 findAll()이라는 메서드를 사용  
- JpaRepository 인터페이스의 상위인 PagingAndSorRepository의 메서드로 파라미터로 전달되는 Pageableㅣ라는 타입의 객체에 의해서 실행되는 쿼리를 결정
- Return type이 `Page<T>`라면 꼭 Pageable 파라미터로 이용해야 한다.
Pageable 인터페이스  

`org.springframework.data.domain.PageRequest` 라는 클래스를 사용
- `protected`로 선언되어 `new`를 선언할 수 없다.
- 객체를 생성하기 위해 static한 `of()`를 이용해 처리
- `PageRequest`생성자에서 `page, size, Sort`라는 정보를 이용해 객체 생성

static 메서드인 `of()`의 경우 형태가 여러개 존재, 이는 페이지 처리에 필요한 정렬 조건을 같이 지정하기 위해서
- `of(int page, int size)` : 0부터 시작하는 페이저 번호과 개수, 정렬이 지정되지 않음
- `of(int page, int size, Sort.Direction direction, String ...props)` : 0부터 시작하는 페이지 번호와 개수, 정렬의 방향과 정렬 기준 필드
- `of(int page, int size, Sort sort)`: 페이지 번호와 개수, 정렬 관련 정보

### 페이징 처리

항상 0부터 시작한다.  
findAll()의 반환값은 Page타입이라는점  
- 해당 목록만 가져오는데 그치지 않고, 실제 페이지 처리에 필요한 전체 데이터의 개수를 가져오는 쿼리 역시 같이 처리하기 때문  

Page<엔티티 타입> 의 다양한 메서드 사용가능
- getTotalPages() : 총 페이지 수
- getTotalElementes() : 전체 개수
- getNumber() : 현재 페이지 번호 (0부터 시작)
- getSize() : 페이지당 데이터 개수
- hasNext() : 다음 페이지 존재 여부
- isFirst() : 시작 페이지(0) 여부
- getcontent() : List<엔티티>으로 처리 가능
- get() : Stream<엔티티>으로 처리 가능

### 정렬 처리

PageRequest에는 `org.springframework.data.domain.Sort` 타입을 파라미터로 전달 가능
- 한 개 혹은 여러 개의 필드값을 이용해 정렬 방향 등을 지정가능
- 여러 개의 Sort 같은 경우 and, or등으로 연결 가능

## chapter 2.6 쿼리 메서드로

마지막 기능은 쿼리 메서드와 `JPQL(Java Persistence Query Language)`  
다양한 검색조건이나 특정 범위 객체 검색의 경우  
Spring Data JPA의 경우 이러한 처리를 위해 방법 제공  
- 쿼리 메서드 : 메서드의 이름 자체가 쿼리의 구문으로 처리되는 기능
- @Query : SQL과 유사하게 엔티티 클래스의 정보를 이용해서 쿼리를 작성하는 기능
- Querydsl 등의 동적 처리 기능

### 쿼리 메서드란

메서드의 이름 자체가 질의(query)문이 되는 흥미로운 기능   
findBy, getBy... 등으로 시작하고 자세한 종류는 `Spring Data JPA Reference`를 통해 알아볼 수 있다.  
필요한 필드 조건이나 `And, Or`등의 키워드를 통해서 질의 조건을 만들어 낸다.  

다양한 장점들
- select를 하는 작업이라면 List 타입이나 배열을 이용 할 수 있다.
- 파라미터에 Pageable 타입을 넣는 경우, 무조건 `Page<Entity>` 타입

### @Query 어노테이션

간단한 처리만 쿼리 메서드를 이용, @Query를 더 많이 사용  
메서드의 이름과 상관없이 메서드에 추가한 어노테이션을 통해 원하는 처리 가능  
JPQL이라고 부르는 객체지향 쿼리 구문 사용  
다음과 같은 작업 진행  
- 필요한 데이터만 선별적으로 추출
- 데이터베이스에 맞는 순수한 SQL(Native SQL)을 사용하는 기능
- insert, update, delete 등 DML을 처리하는 기능 (@Modifying과 함께 사용)  

객체지향 쿼리는 테이블 대신 엔티티 클래스 이용  
테이블의 칼럼 대신에 클래스에 선언된 필드를 이용해 작성  

```java
@ Query("select m from Memo m order by m.mno desc")
List<Memo> getListDesc();
```

파라미터 바인딩
  - ?1, ?2 와 1부터 시작하는 파라미터의 순서를 이용하는 방식
  - :xxx 와 같이 파라미터 이름을 활용하는 방식
  - :#{} 와 같이 자바 빈 스타일을 이용
  - `@Param` 어노테이션을 통해서 파라미터를 적는다.

```java
@Transactional
@Modifying
@Query("update Memo m set m.memoText = :memoText where m.mno = :mno")
int updateMemoText(@Param("mno") Long mno, @Parama("memoText") String memoText);

@Query("update Memo m set m.memoText = :#{#param.memoText} where m.mno = :#{#param.mno}")
int updateMemoText(@Param("mno") Long mno, @Parama("memoText") String memoText);
```

- 페이징 처리를 위해서 Pageable 파라미터를 적용

```java
@Queru(value = "select m from Memo m where m.mno > :mno", countQuery = "select count(m) from Memo m where m.mon > :mno")
Page<Memo> getListWithQuery(Long mno, Pageable Pageable);
```

- @Query의 장점 중 하나는 쿼리 메서드의 경우 엔티티 타입의 데이터만을 추출하지만 @Query를 사용하면 `Object[]`의 형태로 선별적으로 추출

```java
@query(value = "select m.mno, m.memoText, CURRENT_DATE from Memo m where m.mno > :mno", 
        countQuery = "select count(m) from Memo m where m.mno > :mno")
Page<Object[]> getListWithQueryObject(Long mno, Pageable pageable);ß
```

- Native SQL 처리
  - 데이터 베이스 고유의 SQL을 그대로 사용 가능ß
  - @Query의 nativeQuery속성을 true로 설정하면 기존의 SQL그대로 사용가능