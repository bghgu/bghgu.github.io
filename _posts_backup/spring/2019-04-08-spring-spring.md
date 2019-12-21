---
layout: post
title: "Spring & Spring Boot"
description: "Spring & Spring Boot"
date: 2019-04-08
tags: [Spring]
comments: true
share: true
---

#### Spring
* 스스로 발전하는 프레임워크
* 스프링 개발 철한 중 하나는 "항상 프레임워크 기반의 접근 방법을 사용하라"
* 스프링 기능의 대부분은 핵심 기능을 확장해서 발전시킨 결과물이다.
* 단순함과 유연성을 중요 가치로 생각한다.
* 자바 엔터프라이즈 개발을 편하게 해주는 오픈소스 경량급 애플리케이션 프레임워크이다.
* 자바 개발을 위한 프레임워크로 종속 객체를 생성해주고, 조립해 주는 도구이다.
* 필요한 부분만 가져다 사용할 수 있도록 모듈화가 되어 있다.
* 각 모듈은 독립적으로 분리되어 있고, 재사용이 가능하다.
* IOC, DI, AOP가 핵심 기술이다.
* PSA(Portable Service Abstraction)
* POJO(Plain Old Java Object) BEAN Container

#### Spring Boot
* Spring Project중 하나
* 초기 수작업의 셋팅을 자동으로 해준다.
* 프로젝트마다 기본적으로 설정하게 되는 부분들을 이미 내부적으로 가지고 있다.
* Servlet Container를 기본 내장하고 있다. (Tomcat, Jetty)
* Pom.xml에서 의존 라이브러리의 버전을 자동으로 관리해준다.
* 자주 사용하는 프로젝트 조합을 미리 만들어 놓고 스프링을 더욱 쉽고 간단하게 사용하기 위해 만들어졌다.