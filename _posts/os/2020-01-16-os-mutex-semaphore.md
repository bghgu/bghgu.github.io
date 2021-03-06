---
layout: post
title:  "뮤텍스 & 세마포어 & 스핀락"
date:   2020-01-16 18:22:00
author: bghgu
categories: OS
tags: [OS]
---

#### Mutex 뮤텍스
* 상호 배제(Mutual EXclusition)의 약자이다.
* 임계 영역에 무조건 접근 못 하도록 Lock을 건다.
* 하나만 접근할 수 있고 끝나면 unLock이 된다.
* 스레드의 순서를 보장하지 않는다.
* Lock : 현재의 임계 구역에 들어갈 권한을 얻어온다.
* unLock : 현재의 임계 구역을 모두 사용했음을 알린다.
* 뮤텍스 객체를 두 스레드가 동시에 사용할 수 없다.
* 동기화 대상이 오직 하나일 때 사용한다.
* 공유된 자원의 데이터를 여러 스레드가 접근하는 것을 막는 것이다.
* 뮤텍스는 소유가 가능하며, 소유주가 이에 대한 책임을 진다.
* 0, 1 상태가 2개 뿐이라 이진 세마포어라고도 한다. (Lock, unLock)
* 뮤텍스를 소유하고 있는 스레드만이 이 뮤텍스를 해제할 수 있다.
* 프로세스 범위를 가지며 프로세스가 종료될 때 자동으로 cleanUp 된다.

#### Semaphore 세마포어
* 스레드의 실행 순서를 보장할 수 있다.
* 카운트 값이 0이면 진입 불가, 0보다 크면 진입할 수 있다.
* 세마포어는 동시에 여러 개의 프로세스/스레드가 임계 구역에 접근할 수 있도록 카운트를 가지고 있는데 카운트가 1인 세마포어가 뮤텍스이다.
* 리소스의 상태를 나타내는 간단한 카운터이다.
* 동기화 대상이 오직 하나 이상일 때 사용한다.
* 세마포어는 뮤텍스가 될 수 있지만, 뮤텍스는 세마포어가 될 수 없다.
* 공유된 자원에 여러 개의 프로세스가 동시에 접근하면서 문제가 발생할 수 있다.
* 공유된 자원 속 하나의 데이터는 한 번에 하나의 프로세스만 접근할 수 있도록 두어야 하는데 이것이 세마포어이다.
* 소유할 수 없고 운영체제 또는 커널의 한 지정된 저장장치 내 값이다.
* 세마포어는 세마포어를 소유하지 않은 스레드가 세마포어를 해제할 수 있다.
* 시스템 범위에 걸쳐 있고, 파일 시스템상의 파일로 존재한다.
* 카운터 값과 같은 타이밍 오류가 일어날 수도 있다.

#### 뮤텍스 & 세마포어 정리
* 뮤텍스 : 여러 스레드가 접근하는 것을 막는 것
    * 소유가 가능, 소유주가 책임을 진다.
    * 소유한 스레드가 뮤텍스를 해제
    * 프로세스 범위를 가지며 프로세스가 종료될 때 자동으로 cleanUp 된다.
    * 동기화 대상이 오직 하나일 때
* 세마포어 : 공유된 데이터를 여러 프로세스가 접근하는 것을 막는 것
    * 리소스의 상태를 나타내는 카운터
    * 소유할 수 없다.
    * 소유하지 않은 프로세스도 세마포어 카운트 값 변경 가능
    * 시스템 범위에 걸쳐 있고 파일 시스템상의 파일 형태로 존재한다.
    * 동기화 대상이 하나 이상일 때

#### Spin Lock 스핀락
* 다른 스레드가 Lock을 소유하고 있다면 그 Lock이 반환될 때까지 계속 확인하며 기다리는 것이다.
* 조금만 기다리면 바로 쓸 수 있는데 굳이 문맥 교환으로 부하를 줄 필요가 있나 라는 개념에서 개발된 것이다.
* 임계 영역에 진입할 수 없을 때 문맥 교환을 하지 않고, 잠시 루프를 돌면서 재시도 하는 것이다.
* Lock - unLock 하는 과정이 아주 짧아서 Lock 하는 경우가 드문 경우(적절하게 임계 영역을 사용한 경우) 유용하다.
* Lock을 얻을 수 없다면 계속해서 Lock을 확인하면서 얻을 때까지 기다린다. busy waiting이다.
* 바쁘게 기다린다는 것은 무한루프를 돌면서 최대한 다른 스레드에 CPU를 양보하지 않는 것이다.
* Lock이 곧 사용 가능해질 경우 문맥 교환 스위치를 줄여 CPU의 부담을 덜어준다.
* 하지만 어떤 스레드가 Lock을 오랫동안 유지한다면 오히려 CPU 시간을 많이 소모할 가능성이 있다.
* 하나의 CPU나 하나의 코어만 있는 경우에는 유용하지 않다. 만약 다른 스레드가 Lock을 가지고 있고 그 스레드가 Lock을 풀어 주려면 싱글 CPU 시스템에서는 어차피 문맥 교환을 해야 하기 때문이다.
* 스핀락을 잘못 사용하면 CPU 사용률을 100%로 만드는 상황이 발생하므로 주의해야 한다.
* 스핀락은 기본적으로 무한 for를 돌면서 Lock을 기다리므로 하나의 스레드가 Lock을 오랫동안 가지고 있다면 다른 blocking된 스레드는 busy waiting 하므로 CPU를 쓸데없이 낭비하게 된다.
* 스핀락을 잘 사용하면 문맥 교환을 줄여 효율을 높일 수 있다.
* 무한 루프를 돌기보다는 일정 시간 Lock을 얻을 수 없다면 잠시 sleep 하는 back off 알고리즘을 사용하는 것이 훨씬 좋다.

#### Monitor 모니터
* 추상화된 데이터 형(ADT)에는 사적인 데이터(private data)를 공개 메소드(public methods)와 함께 캡슐화하여 공개 메소드가 사적인 데이터에 연산을 실행한다.
* 모니터 형도 프로그래머가 내부를 정의한 ADT이다.
* 모니터 형 내부의 변수값들은 그 변수의 형에 해당하는 한 인스턴스의 상태를 정의하는 것이다.
* 모니터 형의 내용은 다른 프로세스가 사용할 수 없다.
* 즉, 모니터 내의 정의된 프로세스, 함수들만이 그 정의되있는 모니터의 변수에 엑세스 할 수 있다는 것이다.
* 내부에 정의된 프로시저만이 프로시저 내부의 지역 변수들을 엑세스 할 수 있다.
* 모니터 구조물들은 모니터 내부에 언제나 한 개의 프로세스만이 수행될 수 있도록 보장을 해야 한다.
* 세마포어의 단점을 보완하여 나온 개념이다.
* 모니터의 단점
    * 프로세스가 자원에 대한 허락을 받지 않고 자원을 엑세스 할 경우
    * 프로세스가 자원에 대한 허락을 받은 다음 그 자원을 방출하지 않을 경우
    * 프로세스가 자원에 대한 허락을 받지 않았는데도 그 자원을 방출할 경우
    * 프로세스가 자원에 대한 허락을 받은 다음 방출하지 않은 상태에서 또 그 자원을 요청할 경우
* 위 문제는 세마포어에서도 나타나는 문제이다.
* 위 문제를 해결하려면 자원에 접근하는 연산 자체를 모니터 내부에 두어서 모니터 자체의 스케줄러에 맡겨야 한다.

#### 참고
* https://about-myeong.tistory.com/34