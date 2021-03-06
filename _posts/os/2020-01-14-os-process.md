---
layout: post
title:  "Process 프로세스"
date:   2020-01-14 20:29:00
author: bghgu
categories: OS
tags: [OS]
---

#### Process 프로세스
* 컴퓨터에서 연속적으로 실행되고 있는 컴퓨터 프로그램이다.
* 실행 중인 프로그램이다.
* 디스크로부터 메모리에 적재되어 CPU의 할당을 받을 수 있는 것을 말한다.
* 운영체제로부터 주소 공간, 파일, 메모리 등을 할당받으며 이것들을 총칭하여 프로세스라고 한다.
* 메모리에 올라와 실행되고 있는 프로그램의 인스턴스(독립적인 개체)이다.
* 운영체제로부터 시스템 자원을 할당받는 작업의 단위이다.
* 프로세스는 함수의 매개변수, 복귀주소와 로컬 변수와 같은 임시 자료를 갖는 스택과 전역 변수들을 수록하는 데이터 부분을 포함한다.
* 프로세스는 프로세스 실행 중에 동적으로 할당되는 메모리인 힙을 포함한다.
* 프로세스끼리는 서로 완전히 독립적이다.
* 프로세스는 운영체제 관점에서의 실행 흐름을 구성한다.
* 저장소에 존재하는 프로그램을 운영체제가 실행해서 CPU가 처리할 수 있게 메인 메모리로 올라온 상태이다.
* 프로세스 기술자(Process Descriptor), 혹은 프로세스 제어 블록(PCB)이라는 자료 구조에 의해서 정의되는 공간이다.
* 각 프로세스는 자신의 주소 공간을 가진다.
    * 텍스트/코드 영역 : 프로세서가 실행하는 코드를 저장하는 영역이다.
    * 데이터 영역 : 변수들을 저장하는 영역과 프로세스가 실행 중에 사용하려고 동적으로 할당받은 메모리 공간이다.
    * 스택 영역 : 호출된 함수가 사용하는 지역 변수와 복귀 주소 등을 저장하는 공간이다.
    * 힙 영역 : 프로세스 실행 중에 동적으로 할당되는 메모리 공간이다.

#### 프로세스 생성 과정
1. PCB가 생성되며 운영체제가 실행한 프로그램의 코드를 읽어 들여 프로세스에 할당된 메모리의 텍스트 영역에 저장한다.
2. 초기화된 전역 변수 및 static 변수를 데이터 영역에 할당한다.
3. 힙과 스택은 초기 메모리 주소만 초기화한다.
4. PCB에 여러 정보가 기록되면 Ready Queue에서 CPU를 할당받기까지 대기한다.

#### PCB(Process Control Block)
* 특정 프로세스에 대한 중요한 정보를 저장하고 있는 운영체제의 자료구조이다.
* 운영체제는 프로세스를 관리하기 위해 프로세스의 생성과 동시에 고유한 PCB를 생성한다.
* 프로세스는 CPU를 할당받아 작업을 처리하다가도 프로세스 전환이 발생하면 진행하던 작업을 모두 저장하고 PCB에 저장하게 된다.
* 다시 CPU를 할당받게 되면 PCB에 저장되어있던 내용을 불러와 이전에 종료됐던 시점부터 다시 작업을 수행한다.
* PCB에 저장되는 정보
    * 프로세스 식별자(PID) : 프로세스 식별번호
    * 프로세스 상태 : new, ready, running, waiting, terminated 등의 상태를 저장
    * 프로그램 카운터 : 프로세스가 다음에 실행할 명령어의 주소
    * CPU 레지스터
    * CPU 스케줄링 정보 : 프로세스의 우선순위, 스케줄 큐에 대한 포인터 등
    * 메모리 관리 정보 : 페이지 테이블 또는 세그먼트 테이블과 같은 정보를 포함
    * 입출력 상태 정보 : 프로세스에 할당된 입출력 장치들과 열린 파일 목록
    * 어카운팅 정보 : 사용된 CPU 시간, 시간 제한, 계정번호 등

#### 프로세스 간 통신 방법
* Pipe
* 메시지 큐
* 공유 메모리
* 소켓
* 세마포어

#### 프로세스 상태 전이
* 프로세스 상태 전이는 프로세스가 시스템 내에 존재하는 동안 프로세스의 상태가 변하는 것을 의미한다.
* 프로세스의 상태는 제출, 접수, 준비, 실행, 대기(보류) 상태로 나눌 수 있으며, 이 중 주요 세 가지 상태는 준비, 실행, 대기 상태이다.
* 제출(Submit)
    * 작업을 처리하기 위해 사용자가 작업을 시스템에 제출한 상태
* 접수(Hold)
    * 제출된 작업이 스풀 공간인 디스크의 할당 위치에 저장된 상태
* 준비(Ready)
    * 프로세스가 프로세서를 할당받기 위해 기다리고 있는 상태
    * 프로세스는 준비상태 큐(스케줄링 큐)에서 실행을 준비하고 있다.
    * 접수 상태에서 준비 상태로의 전이는 Job 스케줄러에 의해 수행된다.
* 실행(Run)
    * 준비상태 큐에 있는 프로세스가 프로세서를 할당받아 실행되는 상태
    * 프로세스 수행이 완료되기 전에 프로세스에게 주어진 프로세서 할당 시간이 종료(Timer Run Out)되면 프로세스는 준비 상태로 전이된다.
    * 실행 중인 프로세스에 입출력 처리가 필요하면 실행중인 프로세스는 대기 상태로 전이된다.
    * 준비 상태에서 실행 상태로의 전이는 CPU(프로세스) 스케줄러에 의해 수행된다.
* 대기(Wait), 보류, 블록(Block)
    * 프로세스에 입출력 처리가 필요하면 현재 실행 중인 프로세스가 중단되고, 입출력 처리가 완료될 때까지 대기하고 있는 상태
* 종료(Terminated, Exit)
    * 프로세스의 실행이 끝나고 프로세스 할당이 해제된 상태