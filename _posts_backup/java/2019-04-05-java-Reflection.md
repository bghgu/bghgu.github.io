---
layout: post
title: "Java Reflection"
description: "Java Reflection"
date: 2019-04-05
tags: [자바]
comments: true
share: true
---

#### Reflection
* 객체를 통해 클래스의 정보를 분석해 내는 프로그램 기법
* 자바는 동적으로 객체를 생성하는 기술이 없다. 그래서 동적으로 인스턴스를 생성하는 Reflection으로 그 역할을 대신하게 된다.
* Reflection은 구체적인 클래스 타입을 알지 못해도, 그 클래스의 메소드, 타입, 변수들을 접근할 수 있도록 해주는 자바 API이다.
* 자바 클래스 파일은 바이트 코드로 컴파일되어 Static 영역에 위치하게 된다. 때문에 클래스 이름만 알고 있다면 언제든 이 영역을 뒤져서 클래스에 대한 정보를 가져올 수 있다.