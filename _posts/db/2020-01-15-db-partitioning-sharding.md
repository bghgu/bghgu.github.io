---
layout: post
title:  "Partitioning & Sharding"
date:   2020-01-15 11:35:00
author: bghgu
categories: Database
tags: [Database]
---

#### Partitioning 파티셔닝
* 큰 테이블이나 인덱스를 관리하기 쉬운 단위로 분리하는 방법을 의미한다.
* 가용성 : 물리적인 파티셔닝으로 인해 전체 데이터베이스의 훼손 가능성은 줄어들고 데이터 가용이 향상된다.
* 관리 용이성 : 큰 테이블들을 제거하여 관리를 쉽게 해준다.
* 성능 : 주로 대용량 쓰기 환경에 효율적이다.
* 많은 삽입이 있는 시스템에서 삽입 작업들을 분리된 파티션들로 분산시켜 경합을 줄인다.
* 파티셔닝 범위
    * 범위 파티셔닝
    * 리스트 파티셔닝
    * 해쉬 파티셔닝

#### 수직 분할
* 정규화 하는 과정도 이와 비슷하다고 볼 수 있지만 이미 정규화된 데이터를 분리하는 과정이다.
* 자주 사용되는 컬럼들을 분리시켜 성능을 향상시킬 수 있다.
* 도메인에 따라 쉽게 분리할 수 있다.
* 대부분 애플리케이션 단에 CRUD를 구현한다.

#### 수평 분할 - Sharding 샤딩
* 걑은 테이블 스키마를 가진 데이터를 다수의 데이터베이스에 분산하여 저장하는 방법을 의미한다.
* 데이터베이스 레벨에서도 가능하다. 애플리케이션 레벨에서도 가능하다.
* 샤딩을 적용한다는 것은 프로그래밍, 운영적인 복잡도는 더 높아지는 단점이 있다.
* 가능하다면 샤딩을 피하거나 지연시킬 수 있는 방법을 찾아야 한다.
    * 스케일 업 - 장비 성능 향상
    * 읽기 부하가 크다면 캐시나 데이터베이스의 레플리케이션을 적용하는 것도 하나의 방법이다.
    * 테이블의 일부 컬럼만 자주 사용된다면 수직 분할도 하나의 방법이다.
* 샤딩에 필요한 원리
    * 분산된 데이터베이스에서 어떻게 데이터를 읽을 것인가
    * 분산된 데이터베이스에서 데이터를 어떻게 잘 분산시켜서 저장할 것인가
        * 분산되지 않고 데이터가 한쪽으로 몰리게 되면 자연스럽게 핫스팟이 되어 성능이 느려지게 된다.
        * 균일하게 분산하는 것이 중요한 목표이다.
    * 샤드 키를 어떻게 정의하느냐에 따라 데이터를 효율적으로 분산시키는 것이 결정된다.

#### 샤딩 시 고려사항
* 데이터 재분배 : 샤딩을 진행한 DB의 물리적인 용량 한계와 성능 한계가 왔을 경우 적절하게 shard 수를 scale-up 작업을 늘를 수 있도록 설계해야 한다. (확장 고려)
* 샤딩으로부터 데이터 조인 : 샤딩된 데이터베이스간에 조인이 불가능하기 때문에 어느 정도의 데이터 중복은 감안해야 한다.
* 파티셔닝 잘 구현하기 : 샤딩의 기준이 되는 샤드키를 잘 정하거나 hash의 경우 함수를 잘 선택해야 한다.
* 샤드된 DBMS들의 트랜잭션 문제 : XA와 같은 Global Transaction을 사용하면 샤딩된 데이터베이스 간에 트랜잭션이 가능하나 성능 저하의 문제가 있다.
* Global Unique Key : 샤딩의 경우 DBMS에서 제공하는 AI를 사용하면 key가 중복 될 가능성이 있기 때문에 애플리케이션 레벨에서 key 생성을 담당해야 한다.
* 데이터 축소 : 테이블 단위를 가능한 작게 만든다.

#### 참고
* https://coding-start.tistory.com/275
* https://kslee7746.tistory.com/entry/MongoDB-Sharding%EC%83%A4%EB%94%A9
* https://givemesource.tistory.com/87