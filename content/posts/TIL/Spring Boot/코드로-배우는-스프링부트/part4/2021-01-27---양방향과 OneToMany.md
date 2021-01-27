---
title: 양방향과 OneToMany
date: "2021-01-27T00:43:41.804Z"
template: "post"
draft: false
slug: "양방향과 OneToMany"
category: "코드로-배우는-스프링부트"
tags:
  - "코드로-배우는-스프링부트"
  - "Spring"
  - "Spring Boot"
  - "Spring Data JPA"
description: "코드로 배우는 스프링부트 양방향과 OneToMany"
---

# 양방향과 @OneToMany

영화1 : 포스터M 관계에 있어서  
포스터는 FK로 영화의 PK를 참조한다.  
양방향 참조의 경우 항상 하나의 객체쪽에서 필요한 요구사항이다.  
- 여기서 양방향 참조가 일어나게 되면 영화를 조회활 때 포스터들을 같이 조회할 수 있을 것이다.

데이터베이스 관점에서는 항상 FK를 가지는 포스터가 관계를 가지지만, 객체지향적 해석으로 접근한다면 PK를 가지는 쪽이 관계를 정의할 이유가 될 수도 있습니다.  


## 중간정리

- 데이터베이스상에는 PK/FK를 이용한 단방향 밖에 업다.
- 객체지쟣ㅇ에서 필요에 의해 데이터베이스와 반대방향으로 설계할 수 있다.

DB 설계와 반대로 참조하게 되는 경우

- PK를 가진 영화 객체가 모든 포스터 객체를 관리하는 단방향 참좀
- 영화는 포스터를 관리하고, 포스터는 영화에 대한 참조를 가지는 양방향 참조

## @OneToMany

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

## mappedBy 속성

mappedBy 속성을 이용해서 실제 데이터베이스에서 자신은 연관관계의 주인(owner)가 아니라는 것을 명시(FK를 가지고 있지 않고, 내 쪽이 PK다.)