---
layout: post
title: "Java HashMap & HashTable & ConcurrentHashMap"
description: "Java HashMap & HashTable & ConcurrentHashMap"
date: 2019-04-09
tags: [자바]
comments: true
share: true
---

#### HashTable
* put, get과 같은 주요 메소드에 `synchronized 키워드가 선언`되어 있다.
* key, value에 null을 허용하지 않는다.
* 평균 시간 복잡도 O(1)

#### HashMap
* 주요 메소드에 `synchronized 키워드가 없다`.
* key, value에 null을 입력할 수 있다.

#### ConcurrentHashMap
* HashMap을 Thread-safe 하도록 만든 클래스이다.
* key, value에 null을 허용하지 않는다.
* putIfAbsent 메소드가 있다
    * 키 값이 존재하면 기존의 값을 반환한다.
    * 없다면 입력한 값을 저장한 뒤 반환한다.

#### HashTable 저장할 위치 계산 방법
1. Division Method
    * 저정할 값을 배열의 크기로 나눈 나머지가 그 값을 저장할 위치이다.
    * 충돌이 발생하는 것을 최소화하고, 배열에 값이 골고루 위치하게 하려면 배열의 크기가 소수인 것이 좋다.
    * 계산이 간단하다.
    * 배열의 크기가 소수여야만 저장할 위치가 골고루 분포한다.
2. Multiplication Method
    * 이 방법은 0 < A < 1 인 상수 A 가 필요하다.
    * 예를 들어 A 상수 값을 0.3758 이라고 하자.
    * 배열의 크기는 32 이라고 하자.
    * 저장할 값이 14 이라면
    * 다음과 같은 절차로 저장할 위치를 계산한다.
    * (1) 저장할 값을 A와 곱한다. 14 x 0.3758 = 73.6568
    * (2) 위의 결과에서 소수점 아래 자리만 꺼낸다. 0.6568
    * (3) 위의 결과에 배열의 크기를 곱한다. 32 x 0.6568 = 21.0176
    * (4) 위의 결과에서 소수점 아래 자리를 버린다. 21
    * 실수 계산이 있어서 계산이 복잡하다.
    * 배열의 크기는 마음대로 할 수 있지만 보통 2의 배수로 정한다.

#### 충돌을 해결하는 방법
* 서로 다른 값의 저장할 위치가 동일한 경우에 충돌이 발생한다.
1. Chaining
    * 저장할 데이터를 링크드 리스트에 등록한다.
2. Open Addressing
    * 그 다음 칸에 저장을 시도한다.
    * Linear probing : 충돌이 발생한 경우에, 저장을 시도할 그 다음 칸을 계산할 때 처음 저장할 위치에서 1, 2, 3, 4, 뒤 칸을 순서대로 시도하는 방법이다. 1차 군집 문제가 발생한다.
    * Quadratic probing : 충돌이 발생한 경우에, 저장을 시도할 그 다음 칸을 계산할 때 처음 저장할 위치에서 1, 4, 9, 16, 뒤 칸을 순서대로 시도하는 방법이다. 2차 군집 문제가 발생한다.
    * Double hashing : 충돌이 발생했을 때, 건너뛰는 폭이 매번 다른 것이 좋다.
        * int startIndex = value % SIZE;
        * int step = value % 7 + 1
        * int index = (startIndex + (충돌횟수 * step)) / SIZE;