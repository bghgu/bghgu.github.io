---
layout: post
title:  "데이터베이스 무결성"
date:   2020-01-12 21:20:00
author: bghgu
categories: Database
tags: [Database]
---

#### 개체 무결성
* 릴레이션에서 기본키를 구성하는 속성은 널이거나 중복값을 가질 수 없다.

#### 참조 무결성
* 외래키 값은 널이거나 참조 릴레이션의 기본키 값과 동일해야 한다.
* 즉, 릴레이션은 참조할 수 없는 외래키 값을 가질 수 없다.

#### 도메인 무결성
* 특정 속성의 값이 그 속성이 정의된 도메인에 속한 값이어야 한다.

#### 고유 무결성
* 특정 속성에 대해 고유한 값을 가지도록 조건이 주어진 경우 그 속성값은 모두 고유한 값을 가진다. 같으면 안되는 것이다.

#### NULL 무결성
* 특정 속성값에 NULL이 올 수 없다는 조건이 주어진 경우 그 속성값은 NULL이 될 수 없다는 제약조건이다.

#### 키 무결성
* 한 릴레이션에는 최소한 하나의 키가 존재해야하는 제약조건이다.

#### 무결성을 유지하려는 이유는?
* 무결성이 유지가 되어야 DB에 저장된 데이터 값과 거기에 해당하는 현실 세계의 실제값이 일치하는지 신뢰할 수 있기 때문이다.