---
layout: post
title:  "JAVA String"
date:   2020-01-12 20:25:00
author: bghgu
categories: JAVA
tags: [JAVA]
---

#### String
* Immutable
* 문자열이 변하면 기존 객체가 변하는 것이 아니라 새로운 객체가 생성된다.
* 계속해서 객체를 만드는 것은 오버헤드가 발생하므로 성능이 떨어진다.
* Immutable이기 때문에 단순하게 조회연산에서는 타 클래스보다 빠르다.
* 연산에 내부적으로 char 배열을 사용한다.
* 멀티 스레드 환경에서는 동기화를 신경 쓸 필요가 없다.
* 문자열 연산이 적고, 조회가 많을 때, 멀티 스레드 환경에서 적합하다.
* JDK 1.5 이상부터 + 연산하더라도 내부적으로 StringBuilder로 컴파일한다.

#### StringBuilder
* mutable
* 문자열 연산이 자주 있을 때 사용한다.
* Thread-safe 하지 않기 때문에, 싱글 스레드, 스레드를 신경 쓰지 않는 환경에 적합하다.

#### StringBuffer
* mutable
* 문자열 연산이 자주 있을 때 사용한다.
* Thread-safe 하다. (synchronized 키워드가 있다.)