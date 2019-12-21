---
layout: post
title: "URL & URI & URN"
description: "URL & URI & URN"
date: 2019-04-05
tags: [web, URL, URI, URN]
comments: true
share: true
---

#### URI(Uniform Resource Identifier, 통합 자원 식별자)
* 인터넷에 있는 자원을 나타내는 유일한 주소이다.
* 자원을 식별할 수 있는 문자열이다.
* 하위 개념으로 URL, URN이 있다.
* 127.0.0.1:8080/users?name=bghgu 은 URI이다, name=bghgu 이라는 식별자가 있다.

#### URL(Uniform Resource Identifier, 통합 자원 지시기)
* 네트워크 상에서 자원이 어디있는지를 알려주기 위한 규약이다.
* 127.0.0.1:8080/users/image.jpg

#### URN(Uniform Resource Identifier, 통합 자원 이름)
* 특정 자원에 대해 그 자원의 위치에 영향을 받지 않는 유일무이한 이름 역할을 한다.
* 자원을 여기저기로 옮기더라도 문제없이 동작한다.
* DNS