---
layout: post
title: "Persistence(영속성)"
description: "Persistence(영속성)"
date: 2019-04-08
tags: [db]
comments: true
share: true
---

#### Persistence(영속성)
* 데이터를 생성한 프로그램이 종료되더라도 사라지지 않는 데이터의 특성을 뜻한다.
* 영속성을 갖지 않는 데이터는 단지 메모리에서만 존재하기 때문에 프로그램을 종료하면 모두 잃어버리게 된다.
* 데이터를 DB에 저장하는 3가지 방법
    1. JDBC 사용
    2. Spring JDBC(Jdbc Template)
    3. Persistence Framework(Hibernate, MyBatis)

#### Persistence Framework
* JDBC 프로그래밍의 복잡함이나 번거로움 없이 간단한 작업만으로 DB와 연동되는 시스템을 빠르게 개발할 수 있으며 안정적인 구동을 보장한다.
* Persistence Framework는 SQL Mapper와 ORM으로 나눌 수 있다.
* SQL Mapper : Mybatis
* ORM : JPA, Hibernate
* 모든 Persistence Framework는 내부적으로 JDBC API를 이용한다.

#### Persistence Layer
* 프로그램의 아키텍처에서 데이터에 영속성을 부여해주는 계층을 말한다.
* Persostence Framework를 이용한 개발