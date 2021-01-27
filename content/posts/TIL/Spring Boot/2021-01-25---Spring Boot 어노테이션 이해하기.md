---
title: Spring Boot 어노테이션 이해하기
date: "2021-01-25T07:42:31.798Z"
template: "post"
draft: false
slug: "Spring Boot 어노테이션 이해하기"
category: "Spring Boot"
tags:
  - "Spring Boot"
  - "Spring Data JPA"
  - "Lombok"
description: "다양한 Spring Boot의 어노테이션을 정리해보자"
---

# Spring Boot 어노테이션(Annotatiion) 이해하기

본 장에서는 스프링에서 사용되는 다양한 어노테이션에 관련해서 정리한다.  
배우는 족족 하나씩 추가하는 중이다.  

## Spring Boot Application

### **`@Transactional`**

트랜잭션 처리를 지원하는 어노테이션  
선언적 트랜잭션이라고 부른다.  

클래스, 메서드 위에 @Transactional 이 추가되면, 이 클래스에 트랜잭션 기능이 적용된 프록시 객체가 생성된다.

PlatformTransactionManager를 사용해서 트랜잭션을 시작하고, 정상 여부에 따라 Commit 또는 Rollback한다.

### **`@Commit`**

최종 결과를 커밋하기 위해서 사용. 이를 적용하지 않으면 테스트 코드의 deleteBy등의 구문은 자동적으로 Rollback 처리되어서 결과가 반연되지 않는다.

### **`@Controller`**

### **`@RestController`**

컨트롤러 접근 데이터가 오직 텍스트만 존재할 때 사용  
모든 반환값을 JSON으로 처리하며 ResponseEntity<> 객체를 사용  

### **`@GetMapping`**

컨트롤러 내부에서 Get Method를 처리한다.  
파라미터로 변수를 받을 수 있는데, 이는 `@PathVariable`로 처리한다.

### **`@Service`**

스프링에서 빈으로 처리되도록 만들어 주는 어노테이션, Service 계층을 생성해준다.  
서비스 구현체에 붙여 사용한다.

### **`@RequiredArgsConstructor`**

자바 빈을 자동으로 주입하기 위해서 사용하는 어노테이션

---

## JPA

### **`@Entity`**

엔티티 클래스는 Spring Data JPA에서는 반드시 **@Entity** 를 추가해야한다.  
해당 클래스가 엔티티를 위한 클래스이며, 해당 클래스의 인스턴스들을 JPA로 관리하는 엔티티 객체라는 것을 의미

### **`@Table`**

@Entity 어노테이션과 함께 하용하며, 데이터베이스상에서 엔티티 클래스를 어떠한 테이블로 생성할 것인지에 대한 정보를 담음
```java
@Table(name="t_memo")
```

### **`@Id`** 와 **`@GeneratedValue`**

@Entity가 붙은 클래스는 PK에 해당하는 특정 필드를 @Id로 지정해야한다.  
만약 해당 값을 지정해서 사용하는 경우가 아니면 GeneratedValue라는 어노테이션을 활용한다.

키 생성 전략
- GenerationType.AUTO(default)
  - JPA 구현체 (Spring Boot:Hibernate)가 생성 방식을 결정
- GenerationType.IDENTITY : PK 자동생성 전략
  - 오라클 - 별도의 번호를 위한 별도의 테이블 생성
  - MySQL, MariaDB - auto increment를 기본으로 사용해 레코드가 기록될 때마다 다른 번호를 가질 수 있도록 처리
- GenerationType.SEQUENCE
  - 데이터베이스의 sequence를 이요해서 키를 생성
  - @SequenceGenerator 와 함께 사용
- GenerationType.TABLE
  - 키 생성 전용 테이블을 생성해서 키 생성
  - @TableGenerator와 함께 사용

### **`@Column`**

추가적인 필드(칼럼)이 필요할 경우 사용  
다양한 속성을 지정가능
- nullable
- name
- length
- columnDefinition을 통해 기본값 지정 가능

### **`@Query`**

메서드의 이름과 상관없이 메서드에 추가한 어노테이션을 통해서 원하는 처리가 가능.  
value는 JPQL(Java Persistence Query Language)로 작성하는데 객체지향 쿼리라고 불리는 구문들이다.  

다음과 같은 작업 가능
- 필요한 데이터만 선별적으로 추출하는 기능
- 데이터베이스에 맞는 순수한 SQL(Native SQL)을 사용하는 기능
- DDL이 아닌 DML 등을 처리하는 기능(`@Modifying`과 함께 사용)

### **`@Modifying`**

@Query 어노테이션에서 DML 을 처리할 때 같이 써야하는 어노테이션  
update, delete 구문에는 항상 사용해야한다.

### **`@MappedSuperClass`**

해당 어노테이션이 적용된 클래스는 테이블로 생성되지 않는다.

### **`@CreatedDate`**과 **`@LastModifiedDate`**

JPA 내부에서 엔티티 객체가 생성/변경되는 것을 가지하는 역할은 AuditingEntityListener로 이루어진다.  
해당 기능을 사용하기 위해 `@EnableJpaAuditing` 설정은 Application 클래스에 추가해 주어야 한다.  
엔티티의 생성시간을 처리하고, 최종 수정 시간을 자동으로 처리하는 용도  
- 속성으로 insertable, updatable 등 지정 가능  

### **@`ManyToOne`**

데이터베이스 구조로 한 테이블이 다른 테이블을 FK를 이요한 참조를 이용하게 되면 사용  
JPA에서 FK쪽을 먼저 고려해 Many쪽에 사용  

fetch 타입에는 (fetch = FetchType,~~)를 적용
- LAZY : 지연 로딩
- EAGAR : 즉시 로딩

### **`@OneToMany`**

@OneToMany 어노테이션은 영화와 포스터의 1:N의 관계로 지정하기 위해서 사용한다.  
1인 영화쪽에서 N인 포스터에 대한 참조를 표현한다.

```java
public class Movie extends BaseEntity {
    @OneToMany(fetch = fetchType.LAZY)
    private List<Poster> poster = new ArrayList<>();
}
```

fetchType은 기본적으로 LAZY긴 하지만 명시적으로 처리해 준다.  

@OneToMany를 위와같이 쓰는 이유는 기본적으로 M:N의 관계를 구성하기 위해서 사용하기 때문  
@OneToMany의 경우 @JoinTable 등을 이용해 별도의 테이블을 지정하거나 mappedBy 속성을 이용해서 하위 엔티티를 이용하는 설정을 추가할 수도 있다.  

cascade 속성을 사용해서 현재 엔티티 객체의 상태를 하위 엔티티 객체들에게 전파 하고 처리할 수 있다.  
- ALL : 모든 상태 변화 전파
- PERSIST : 상위 엔티티 객체가 영속 컨텍스트에 저장되면 하위 엔티티들도 같이 저장
- MERGE : 현재 겍체와 영속 컨텍스트 내 객체와 병합되면 하위 엔티티들도 같이 반영
- REMOVE : 엔티티 객체 삭제 시점에 하위 엔티티들에 전파
- DETACT : 상위 엔티티가 영속 컨텍스트에 분리되면 하위 엔티티들도 같이 분리

orphanRemoval 속성은 '함조가 없는 하위 엔티티 객체는 삭제할 것인가?'에 대한 설정


### **`@EntityGraph`**

@EntityGraph의 경우, 엔티티의 특정 속성을 같이 로딩하도록 표시하는 어노테이션  
JPA를 이용할 경우, 연관관계의 FETCH 속성은 거의 LAZY로 하는게 일반적  
@EntityGraph는 이러한 상황에서 특정 기능을 수행할 때만 EAGER로딩을 하도록 설정  
다음과 같은 속성과 type을 가진다.  
- **attributePaths** : 로딩 설정을 변경하고 싶은 속성의 이름을 배열로 명시
- **type** : @EntityGraph를 어떤 방식으로 적용할 것인지 설정
- FATCH 속성값은 attributePaths에 명시한 속성은 EAGER로 처리하고, 나머지는 LAZY로 처리
- LOAD 속성값은 attributePaths에 명시한 속성은 EAGER로 처리하고, 나머지는 엔티티 클래스에 명시되거나 기본 방식으로 처리

---

## Lombok

롬복 어노테이션의 경우 싫어하는 사람이 많다..고 알고있지만 스프링부트를 처음 배우는 입장에서 이것 만큼 쉬운게 없다.

### **`@Getter, @Setter`**

클래스 내부 변수의 Getter, Setter함수를 자동으로 생성해준다.

### **`@ToString()`**

해당 객체의 toString() 메서드를 자동으로 생성  

만약 해당 객체가 다른 객체(엔티티)를 참조하고 있는 경우 복잡한 쿼리문이 발생할 여지가 있다.  
이를 방지하기 위해서 exclude 옵션을 사용한다.
- 해당 속성값으로 지정된 변수는 toString()메서드에서 제외하기 때문

### **`@Builder`**

빌더패턴을 사용하여 클래스를 생성할 수 있게 도와준다.  
@Builder 어노테이션을 사용하기 위해선 `@AllArgsConstructor`와 `@NoArgsConstructor`를 함께 사용해야한다.

### **`@Log4j2`**

스프링 부트가 로그 라이블러ㅣ 중에 Log4j2를 기본으로 사용하기 때문에, 별도의 설정 없이 적용 가능

### **`@Data`**

Getter/Setter, toString(), equals(), hashCode()를 자동으로 생성하는 어노테이션