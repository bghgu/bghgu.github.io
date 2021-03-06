---
layout: post
title:  "교착 상태"
date:   2020-01-16 17:58:00
author: bghgu
categories: OS
tags: [OS]
---

#### DeadLock 교착 상태
* 두 개 이상의 작업이 서로 상대방의 작업이 끝나기만을 기다리고 있기 때문에 결과적으로 아무것도 완료되지 못하는 상태이다.

#### 교착 상태의 조건
* 네 가지 조건이 모두 충족되어야 교착 상태가 발생한다.
1. 상호 배제(Mutual Exclusion) : 자원 자체를 동시에 쓸 수 없다.
2. 점유 대기(Hold and Wait) : 자원을 붙잡은 상태에서 다른 자원을 기다리고 있다.
3. 비 선점(No Preemption) : 다른 프로세스의 자원을 빼앗을 방법이 없다.
4. 순환 대기(Circular Wait) : 대기가 꼬리를 물고 사이클이 형성되었다.

#### 교착 상태의 관리 1. 예방
* 교착 상태의 조건을 제거한다.
* 자원의 낭비가 심하다.
    1. 상호 배제 부정 : 여러 개의 프로세스가 공유 자원을 사용할 수 있도록 한다.
    2. 점유 대기 부정 : 프로세스가 실행되기 전 필요한 모든 자원을 할당한다.
    3. 비 선점 부정 : 자원을 점유하고 있는 프로세스가 다른 자원을 요구할 때 점유하고 있는 자원을 반납하고, 요구한 자원을 사용하기 위해 기다리게 한다.
    4. 순환 대기 부정 : 자원에 고유한 번호를 할당하거나, 순서대로 자원을 요구하도록 한다.

#### 교착 상태의 관리 2. 회피
* 교착 상태가 발생하면 피해나가는 방법이다.
* 프로세스가 자원을 요구할 때 시스템은 자원을 할당한 후에도 안정 상태로 남아있게 되는지를 사전에 검사하여 교착 상태를 회피하는 기법이다.
* 안정 상태에 있는 자원을 할당하고, 그렇지 않으면 다른 프로세스들이 자원을 해제할 때까지 대기한다.
* 은행원 알고리즘
    * 프로세스가 자원을 요구할 때 시스템이 자원을 할당한 후 안정 상태로 남아있게 되는지를 사전에 검사하여 교착 상태를 회피하는 방법이다.
    * 은행원 알고리즘은 다익스트라가 제안한 기법으로, 은행에서 모든 고객의 요구가 충족되도록 현금을 할당하는데서 유래한 기법이다.
    * 각 프로세스에게 자원을 할당하여 교착 상태가 발생하지 않으며 모든 프로세스가 완료될 수 있는 상태를 안정 상태, 교착 상태가 발생할 수 있는 상태를 불안정 상태라고 한다.
    * 은행원 알고리즘을 적용하기 위해서는 자원의 양과 사용자(프로세스) 수가 일정해야 한다.
    * 은행원 알고리즘은 프로세스의 모든 요구를 유한한 시간안에 할당하는 것을 보장한다.

#### 교착 상태의 관리 3. 발견
* 자원 할당 그래프를 통해 교착 상태를 탐지할 수 있다.
* 자원을 요청할 때마다 탐지 알고리즘을 실행하면 오버헤드가 발생한다.

#### 교착 상태의 관리 4. 회복
* 교착 상태를 일으킨 프로세스를 종료하거나, 할당된 자원을 해제함으로써 회복하는 것을 의미한다.
* 프로세스를 종료하는 방법
    * 교착 상태의 프로세스를 모두 중지한다.
    * 교착 상태가 제거될 때가지 한 프로세스씩 중지한다.
* 자원을 선점하는 방법
    * 교착 상태의 프로세스가 점유하고 있는 자원을 선점하여 다른 프로세스에게 할당하고 해당 프로세스를 일시 정지시키는 방법이다.
    * 우선 순위가 낮은 프로세스, 수행된 횟수가 적은 프로세스 등을 위주로 프로세스의 자원을 선점한다.
* 자원 선점시 고려사항
    1. 자원을 선점할 프로세스 선택 문제 : 최소의 피해를 줄 수 있는 프로세스를 선택한다.
    2. 자원을 선점한 프로세스의 복귀 문제 : 자원이 부족한 상태이므로 대부분 일시 중지시키고 다시 시작하는 방법을 사용한다.
    3. 기아 현상 문제 : 한 프로세스가 계속하여 자원 선점 대상이 되지 못하도록 고려해야 한다.