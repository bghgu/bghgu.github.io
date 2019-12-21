---
layout: post
title: "Spring Bean Life Cycle"
description: "Spring Bean Life Cycle"
date: 2019-04-08
tags: [Spring]
comments: true
share: true
---

#### Spring Bean Life Cycle
1. Bean 인스턴스화 및 DI
2. 스프링인지 여부 검사
3. Bean 생성 생명주기 Callback
4. Bean 소멸 생명주기 Callback

#### bean 저장 형식
* DefaultSingletonBeanRegistry 클래스에 Map<String, Object> singletonObjects = new ConcurrentHashMap<>(256); 으로 싱글톤 객체가 저장된다.