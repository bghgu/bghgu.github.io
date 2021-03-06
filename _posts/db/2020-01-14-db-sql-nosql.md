---
layout: post
title:  "RDBMS & NoSQL(MySQL & MongoDB)"
date:   2020-01-14 18:53:00
author: bghgu
categories: Database
tags: [Database]
---

#### SQL(관계형 데이터베이스), RDBMS
* SQL은 구조화 된 쿼리 언어(Structured Query Language)의 약자이다.
* 그러므로 데이터베이스 자체를 나타내는 것이 아니라, 특정 유형의 데이터베이스와 상호 작용하는 데 사용하는 쿼리 언어이다.
* SQL을 사용하면 관계형 데이터베이스 관리 시스템(RDBMS)에서 데이터를 저장, 수정, 삭제 및 검색 할 수 있다.
* 관계형 데이터베아스에는 두 가지 주요 특징이 있다.
* 데이터는 정해진(엄격한) 데이터 스키마(=structure)를 따라 데이터베이스 테이블에 저장된다.
* 데이터는 관계를 통해서 연결된 여러개의 테이블에 분산된다.

##### 엄격한 스키마
* 데이터는 테이블(table)에 레코드(record)로 저장되며, 각 테이블에는 명확하게 정의된 구조(structure)가 있다.
* 구조란 어떤 데이터가 테이블에 들어가고 어떤 데이터가 그렇지 않을지를 정의하는 필드(field) 집합을 가르킨다.
* 구조(structure)는 필드의 이름과 데이터 유형으로 정의된다.
* 관계형 데이터베이스에서 스키마를 준수하지 않는 레코드는 추가할 수 없다.
* 스키마를 뜯어고치지 않는 한 필드를 추가할 수 없다.

##### 관계
* SQL 기반의 데이터 베이스의 또 다른 중요한 부분은 관계이다.
* 데이터들을 여러개의 테이블에 나누어서, 데이터들의 중복을 피할 수 있다.
* 만약 사용자가 구입한 상품들을 나타내기 위해서는 , User(사용자), Products(상품), Orders(주문한 상품) 여러 테이블을 만들어야 하지만, 각각의 테이블들은 다른 테이블에 저장되지 않은 데이터 만을 가지고 있다. (중복된 데이터가 없다.)
* 이런 명확한 구조는 장점이 있다.
* 하나의 테이블에서 중복없이 하나의 데이터만을 관리하기 때문에, 다른 테이블에서 부정확한 데이터를 다룰 위험이 없다.

#### NoSQL(비 관계형 데이터베이스)
* NoSQL은 기본적으로 RDBMS(관계형 데이터베이스)와 반대되는 접근방식을 따르기 때문에 지어진 이름이다.
* 스키마 없음
* 관계 없음
* NoSQL에서는 레코드를 문서(documents)라고 부른다.
* RDBMS에서는 정해진 스키마를 따르지 않는다면 데이터를 추가 할 수 없지만, NoSQL에서는 다른 구조의 데이터를 같은 컬렉션(=RDBMS에서의 테이블)에 추가할 수 있다.
* 문서는 JSON 데이터와 비슷한 형태를 가지고 있다.
* 스키마에 대해서는 신경 쓸 필요가 없다.
* 또한 일반적으로 관련 데이터를 동일한 컬렉션에 넣는다. (관계형 데이터베이스처럼 여러 테이블에 나누어 담지 않는다.)
* 따라서 많은 Order(주문한 상품)이 있는 경우, 일반적인 정보를 모두 포함한 데이터를 Orders 컬렉션에 저장한다. (즉, 관계형 데이터베이스에서 사용했던 Users나 Products 정보 또한 Orders에 포함해서 한꺼번에 저장된다.)
* 따라서 여러 테이블 / 컬렉션에 조인(join)할 필요없이 이미 필요한 모든 것을 갖춘 문서를 작성하게 된다.
* NoSQL 데이터베이스는 조인이라는 개념이 존재하지 않는다.
* 만약 조인을 하고 싶다면, 직접 해당 외래키를 검색하여 사용할 수 있겠지만, 일반적인 방법은 아니다.
* 대신 컬렉션을 통해 데이터를 복제하여 각 컬렉션 일부분에 속하는 데이터를 정확하게 산출하도록 한다.
* 이런 방식은 데이터가 중복되기 때문에 불안정한 측면이 있다.
* 실수로 컬렉션 B에서는 데이터를 수정하지 않았는데, 컬렉션 A에서만 데이터를 업데이트 할 위험이 있다.
* 특정 데이터를 같이 사용하는 모든 컬렉션에서, 똑같은 데이터 업데이트를 수행되도록 해야 한다.
* 그럼에도 불구하고, 이러한 방식의 커다란 장점은 복잡하고 (어떤 순간에는 느린)조인을 사용할 필요가 없다는 것이다.
* 필요한 모든 데이터가 이미 하나의 컬렉션안에 저장되어 있기 때문이다.
* 특지 자주 변경되지 않는 데이터일 때 더 큰 장점이 있다.

#### 수직적(Vertical) & 수평적(Horizontal) 확장(Scaling)
* 두 종류의 데이터베이스를 비교할 때 살펴 봐얗ㄹ 또 하나의 중요한 개념은 확장(Scaling)이다.
* 확장은 수직적(Vertical) 확장과 수평적 확장(Horizontal) 확장으로 구별할 수 있다.
* 수직적 확장이란 단순히 데이터베이스 서버의 성능을 향상시키는 것이다. = 스케일 업
* 수평적 확장은 더 많은 서버가 추가되고 데이터베이스가 전체적으로 분산됨을 의미한다. = 스케일 아웃
* 따라서 하나의 데이터베이스에서 작동하지만 여러 호스트에서 작동한다.
* 데이터가 저장되는 방식 때문에 RDBMS에서는 일반적으로 수직적 확장만을 지원한다.
* 수평적 확장은 NoSQL 데이터베이스에서만 가능하다.
* RDBMS는 샤딩(Sharding)의 개념을 알고 있지만 특정 제한이 있으며 구현하기가 대체로 어렵다.
* NoSQL 데이터베이스는 이를 기본적으로 지원하므로 여러 서버에서 데이터베이스를 쉽게 분리 할 수 있다.

#### 요약
* RDBMS의 장점
    * 명확하게 정의된 스키마, 데이터 무결성 보장
    * 관계는 각 데이터를 중복없이 한번만 저장한다.
* RDBMS의 단점
    * 상대적으로 덜 유연하다.
    * 데이터 스키마는 사전에 계획되고 알려져야 한다. (나중에 수정하기가 번거롭거나 불가능 할 수도 있다.)
    * 관계를 맺고 있기 때문에, JOIN문이 많은 매우 복잡한 쿼리가 만들어 질 수 있다.
    * 수평적 확장이 어렵고, 대체로 수직적 확장만 가능하다.
    * 즉, 어떤 시점에서 (처리할 수 있는 처리량과 관련하여) 성장 한계에 직면하게 된다.
* NoSQL의 장점
    * 스키마가 없기 때문에 훨씬 더 유연하다.
    * 즉, 언제든지 저장된 데이터를 조정하고 새로운 필드를 추가할 수 있다.
    * 데이터는 애플리케이션이 필요로 하는 형식으로 저장된다. 이렇게 하면 데이터를 읽어오는 속도가 빨라진다.
    * 수직 및 수평 확장이 가능하므로 데이터베이스가 애플리케이션에서 발생시키는 모든 읽기/쓰기 요청을 처리할 수 있다.
* NoSQL의 단점
    * 유연성 때문에 데이터 구조 결정을 하지 못하고 미루게 될 수 있다.
    * 데이터 중복은 여러 컬렉션과 문서가 여러 개의 레코드가 변경된 경우 업데이트 해야 한다.
    * 데이터가 여러 컬렉션에 중복되어 있기 때문에, 수정을 해야 하는 경우 모든 컬렉션에서 수행해야 한다. RDBMS에서는 중복된 데이터가 없기 때문에 한번만 수행하면 된다.
* RDBMS 사용
    * 관계를 맺고 있는 데이터가 자주 변경(수정)되는 애플리케이션일 경우
    * 변경될 여지가 없고, 명확한 스키마가 사용자와 데이터에게 중요한 경우
* NoSQL 사용
    * 정확한 데이터 구조를 알 수 없거나 변경/확장 될 수 있는 경우
    * 일기(read) 처리를 자주 하지만, 데이터를 자주 변경(update)하지 않는 경우(즉, 한번의 변경으로 수십 개의 문서를 업데이트 할 필요가 없는 경우)
    * 데이터베이스를 수평으로 확장해야 하는 경우(즉, 막대한 양의 데이터를 다뤄야 하는 경우)

#### 참고
* https://siyoon210.tistory.com/130
* http://bigmatch.i-um.net/2013/12/09/mongodb%EB%A5%BC-%EC%93%B0%EB%A9%B4%EC%84%9C-%EC%95%8C%EA%B2%8C-%EB%90%9C-%EA%B2%83%EB%93%A4/
* https://www.mongodb.com/compare/mongodb-mysql
* https://sjh836.tistory.com/98
* 