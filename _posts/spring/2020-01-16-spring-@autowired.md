---
layout: post
title:  "Spring @Autowired & 생성자 의존성 주입"
date:   2020-01-16 15:27:00
author: bghgu
categories: Spring
tags: [Spring]
---

#### Spring 생성자 의존성 주입
* Spring 4.3+ 부터 생성자가 1개일 경우 @Autowired 없이 생성자 의존성 주입이 가능하다.
* 단일 책임의 원칙
    * 생성자의 인자가 많을 경우 코드량도 많아지고, 의존 관계도 많아져 단일 책임의 원칙에 위배된다.
    * 의존 관계, 복잡성을 쉽게 알 수 있어 리팩토링의 단초를 제공하게 된다.
* 테스트의 용이성
    * 특정 DI 컨테이너에 의존하지 않고, POJO여야 한다.
    * DI 컨테이너를 사용하지 않고도 인스턴스화 할 수 있고, 단위 테스트도 가능하며 다른 DI 프레임워크로 전환할 수 있게 된다.
* Immutability
    * 필드가 final이 가능해 객체가 변경 불가능 상태가 된다.
* 순환 의존성
* 의존성 명시