---
layout: post
title:  "Context Switch 문맥 교환"
date:   2020-01-16 17:38:00
author: bghgu
categories: OS
tags: [OS]
---

#### Context Switch 문맥 교환
* 하나의 프로세스가 CPU를 사용 중인 상태에서 다른 프로세스가 CPU를 사용하기 위해 이전의 프로세스의 상태(문맥)을 보관하고 새로운 프로세의 상태를 적재하는 작업이다.
* 한 프로세스의 문맥은 그 프로세스의 프로세스 제어 블록(PCB)에 기록된다.
* 문맥을 교환하는 동안에는 유용한 작업을 수행할 수 없으므로 문맥 교환 시간은 일종의 오버헤드라고 할 수 있다.
* CPU가 일을 바꿔가며 실행하기 위해 문맥 교환이 필요하게 되었다.
* 문맥 교환이 발생하게 되면 큰 비용이 든다.
    * cache 초기화
    * 메모리 맵핑 초기화
    * 커널은 항상 실행되어야 한다.
* 프로세스 문맥 교환이 스레드 문맥 교환보다 비용이 많이 든다.
    * 스레드는 스택 영역, PC 레지스터 영역을 제외한 모든 메모리를 공유하기 때문이다.
    * 문맥 교환 발생 시 스택, PC 레지스터 영역만 변경을 진행하면 된다.

#### 문맥 교환의 시점
* 멀티 태스킹
* 인터럽트 핸들링
* 사용자 모드와 커널 모드간 전환
* 준비 -> 실행
* 실행 -> 준비
* 실행 -> 대기

#### 문맥 교환 시나리오
1. 인터럽트나 시스템 호출로 문맥 교환 요구
2. 사용자 모드 -> 운영체제 모드 변환
3. 기존 프로세스의 현재 하드웨어 상태 정보를 PCB에 저장
4. 다음에 실행할 프로세스의 상태 정보를 PCB에서 복구한 다음 프로세스를 실행
5. 운영체제 모드 -> 사용자 모드로 변환