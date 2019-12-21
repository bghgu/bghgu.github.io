---
layout: post
title: "Spring Data JPA"
description: "Spring Data JPA"
date: 2019-04-08
tags: [Spring]
comments: true
share: true
---

#### JPA(Java Persistent API)
* 자바 ORM 기술에 대한 API 표준 명세로, 자바에서 제공하는 API이다.
* 응용프로그램에서 관계형 데이터베이스의 관리를 표현하는 자바 API이다.
* JPA는 ORM을 사용하기 위한 표준 인터페이스를 모아둔 것이다.
* JPA 구성요소
    1. javax.persistance 패키지로 정의된 API 그 자체
    2. JPQL(Java Persistence Query Language)
    3. 객체/관계 메타 데이터
* 사용자가 원하는 JPA 구현체를 선택해서 사용할 수 있다.
* JPA의 대표적인 구현체로는 Hibernate, EclipseLink, DataNucleus, OpenJPA, TopLink Essentials 등이 있다. 이 구현체들을 ORM Framework라고 부른다.

#### Hibernate
* JPA 구현체 중 하나이다.
* 하이버네이트가 SQL을 직접 사용하지 않는다고 해서 JDBC API를 사용하지 않는다는 것은 아니다.
* 하이버네이트가 지원하는 메소드 내부에는 JDBC API가 동작하고 있으며, 단지 개발자가 직접 SQL을 작성하지 않을 뿐이다.
* HQL(Hibernate Query Language)라는 매우 강력한 쿼리 언어를 포함하고 있다.
* 장점
    * 객체 지향적으로 데이터를 관리할 수 있기 때문에 비즈니스 로직에 집중할 수 있으며, 객체지향 개발이 가능하다.
    * 테이블 생성, 변경, 관리가 쉽다.
    * 로직을 쿼리에 집중하기 보다는 객체자체에 집중할 수 있다.
    * 빠른 개발이 가능하다.
* 단점
    * 어렵다.(많은 내용을 정확히 알아야 한다.)
    * 잘 이해하고 사용하지 않으면 데이터 손실이 있을 수 있다.(persistence context)
    * 성능상 문제가 있을 수 있다.

#### Spring Data JPA
* Spring Framework에서 JPA를 편리하게 사용할 수 있도록 지원하는 프로젝트이다.
* CRUD를 처리하기 위한 공통 인터페이스를 제공한다.
* 인터페이스만 작성하면 Runtime시에 Spring Data JPA가 구현 객체를 동적으로 생성해 주입해준다.
* 직접 작성한 인터페이스는 메소드 이름을 분석해서 JPQL을 실행한다.
* MyBatis Mapper와 동시에 사용이 가능하다.

#### JPA/Hibernate 양방향 관계 문제 해결
* 

#### 즉시 로딩 & 지연 로딩
* 즉시 로딩 : 엔티티를 조회할 때 연관된 엔티티도 함께 조회한다.
* (fetch = FetchType.EAGER)
* 지연 로딩 : 엔티티를 조회할 때 연관된 엔티티를 실제 사용할 때 조회한다.
* (fetch = FetchType.LAZY)
* 프록시 객체는 주로 연관된 엔티티를 지연 로딩할 때 사용한다.

#### JPA 기본 패치 전략
* @ManyToOne, @OneToOne : 즉시 로딩(FetchType.EAGER)
* 연관된 엔티티가 하나면 즉시 로딩을 한다.
* @OneToMany, @ManyToMany : 지연 로딩(FetchType.LAZY)
* 연관된 엔티티가 컬렉션이면 지연 로딩을 한다.
* 컬렉션은 즉시 로딩시 비용이 많이 들고 많은 데이터가 로딩될 수 있기 때문이다.