---
layout: post
title: "Spring Mybatis"
description: "Spring Mybatis"
date: 2019-04-08
tags: [Spring]
comments: true
share: true
---

#### Spring Mybatis
* 개발자가 지정한 SQL 저장 프로시저, 그리고 몇가지 고급 매핑을 지원하는 Persistent 프레임워크다.
* JDBC로 처리하는 상당 부분의 코드와 파라미터 설정 및 결과 매핑을 대신해준다.
* 데이터베이스 결과에 원시 타입과 Map 인터페이스, 그리고 POJO를 설정해서 매핑하기 위해 XML과 어노테이션을 사용할 수 있다.
* SqlSessionFactory을 사용한다. 실제 SQL을 호출해주는 역할을 한다.
* Java 소스에서 SQL을 분리해준다.
* JDBC로 처리하는 상당 부분의 코드와 파라미터 설정 및 결과 매핑을 대신해준다.

#### SqlSession
* Mybatis에서는 SqlSession을 생성하기 위해 SqlSessionFactory를 사용한다.
* 세션을 한번 생성하면 매핑 구문을 실행하거나 커밋, 롤백을 하기 위해 세션을 사용할 수 있다.
* 마지막으로 더 이상 필요하지 않은 상태가 되면 세션을 닫는다.
* 마이바티스 스프링 연동 모듈을 사용하면 SqlSessionFactory를 직접 사용할 필요가 없다.
* 스프링 트랜잭션 설정에 따라 자동으로 커밋, 롤백을 수행하고 닫혀지는, 쓰레드에 안전한 SqlSession 객체가 스프링 빈에 주입될 수 있기 때문이다.

#### SqlSessionTemplate
* 마이바티스 스프링 연동모듈의 핵심이다.
* SqlSession을 구현하고 코드에서 SqlSession을 대체하는 역할을 한다.
* 스레드에 안전하고 여러개의 DAO나 매퍼에서 공유할 수 있다.