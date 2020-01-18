---
layout: post
title:  "동기 & 비동기 & Blocking & Non Blocking"
date:   2020-01-17 10:48:00
author: bghgu
categories: ETC
tags: [ETC]
---

#### 동기 & 비동기 비유를 통한 쉬운 설명
* 해야 할 일(task)이 빨래, 설거지, 청소 세 가지가 있다고 가정한다.
* 이 일들을 동기적으로 처리한다면 빨래를 하고 설거지를 하고 청소를 한다.
* 비동기적으로 일을 처리한다면 빨래하는 업체에 빨래를 시킨다.
* 설거지 대행업체에 설거지를 시킨다.
* 청소 대행업체에 청소를 시킨다.
* 셋 중 어떤 일이 먼저 완료될지는 알 수 없다.
* 일을 모두 마친 업체는 나에게 알려주기로 했으니 나는 다른 작업을 할 수 있다.
* 이때는 백그라운드 스레드에서 해당 작업을 처리하는 경우의 비동기를 의미한다.

#### 동기 & 비동기
* 동기와 비동기의 차이는 메소드를 실행시킴과 동시에 반환 값이 기대되는 경우를 동기, 그렇지 않은 경우를 비동기
* 동시에라는 말은 값이 반환되기 전까지는 blocking 되어 있다는 것을 의미한다.
* 비동기의 경우, blocking 되지 않고 이벤트 큐에 넣거나 백그라운드 스레드에 해당 task를 위임하고 바로 다음 코드를 실행하기 때문에 기대되는 값이 바로 반환되지 않는다.

#### Synchronous 동기 (동시에 일어나는, 같은 시기)
* 동시에 일어난다.
* 요청과 그 결과가 동시에 일어나서 요청하면 바로 그 요청한 자리에서 결과를 준다.
* 시간이 얼마가 걸리든 상관 안 하고 요청한 그 자리에서 결과를 주겠다는 약속 같은 것이다.
* 요청과 결과가 한 자리에서 동시에 일어난다.
* 설계가 매우 간단하고 직관적이다.
* 결과를 줄 때까지 아무것도 못 하고 대기해야 한다.
* System Call이 끝날 때까지 기다리고 대기해야 한다.
* System Call이 끝날 때까지 기다리고 결과물을 가져온다.

#### ASynchronous 비동기 (동시에 일어나지 않은, 같은 시기가 아닌)
* 동시에 일어나지 않는다.
* 요청과 결과가 동시에 일어나지 않는다. 바로 결과를 주지 않고 나중에 처리된다.
* 동기 방식보다 설계가 복잡하다.
* 결과가 주어지는 시간이 길어져도 그 시간 동안 다른 작업을 할 수 있으므로 좀 더 효율적으로 자원을 사용할 수 있다.
* System Call이 완료되지 않아도 나중에 완료되면 그때 결과물을 가져온다.
* 주로 Callback 함수를 통해 결과물을 가져온다.

#### Blocking 블로킹
* 작업이 중단된다는 의미이다.
* 네트워크 통신에서 요청이 발생하고 완료될 때까지 모든 일을 중단한 상태로 대기해야 하는 것을 의미한다.
* 블로킹 방식의 소켓 통신은 결과가 올 때까지 다른 작업을 중단하고 하염없이 기다리게 된다.
* System Call이 끝날 때까지 프로그램은 기다려야 하고 System Call이 완료되면 그때야 Return 한다.
* Wait Queue에 들어간다.
* Scanner 입력

#### Non Blocking 논 블로킹
* 작업이 중단되지 않는다.
* 통신이 완료될 때까지 중단되는 블로킹의 반대 개념이다.
* 통신이 완료될 때까지 기다리지 않고 다른 작업을 수행할 수 있으므로 때에 따라 효율이나 반응속도가 더 뛰어나다.
* 블로킹보다 설계가 복잡하다.
* System Call이 완료되지 않아도 대기하지 않고 Return 한다.
* Wait Queue에 들어가지 않는다.

#### Synchronous & Asynchronous 차이점
* 결과물을 가져오는 시점이 다르다.

#### Blocking & Non-Blocking 차이점
* 프로그램이 바로 실행할 수 있는 유무가 다르다.

#### 동기 & 블로킹 차이점
* Wait Queue의 유무
* 동기는 System Call의 Return을 기다리는 동안 Wait Queue에 머물 수도, 아닐 수도 있다.
* 블로킹은 System Call의 Return을 기다리는 동안 필수로 Wait Queue에 머문다.

#### 비동기 & 논 블로킹 차이점
* System Call이 즉시 반환될 때 데이터의 포함 여부
* 비동기는 요청에 처리 완료와 관계없이 응답한다. 이후 운영체제에서 응답할 준비가 되면 응답한다.
* 논 블로킹은 요청에 처리할 수 있으면 바로 응답하고, 아니면 Error를 반환한다.

#### 동기 + 블로킹
* System Call이 끝날 때까지 기다리고 결과물을 가져온다.
* System Call이 끝날 때까지 프로그램은 기다려야 하고, System Call이 완료되면 그때야 Return 한다.
* 메인 스레드는 함수를 호출하고 반환 값을 얻을 때까지 대기해야 한다.
* 함수를 실행하는 스레드는 작업이 끝날 때까지 대기하고 반환 값을 계산해서 넘겨야 한다.
* Read/Write

#### 동기 + 논 블로킹
* System Call이 끝날 때까지 기다리지 않고 결과물을 가져온다.
* System Call이 완료되지 않아도 대기하지 않고 Return 한다.
* 메인 스레드는 함수를 호출하고 반환 값을 얻을 때까지 대기해야 한다.
* 함수를 수행하는 스레드는 작업 종료 여부와 관계없이 반환 값을 넘긴다.
* 메인 스레드는 함수로부터 받은 값이 정상일 때까지 계속 함수를 실행한다.
* Read/Write(O_NONBLOCK)
* 정상 데이터가 올 때까지 System Call을 한다.

#### 비동기 + 블로킹
* System Call이 완료되지 않아도 나중에 완료되면 그 때 결과물을 가져온다.
* System Call이 끝날 때까지 프로그램은 기다려야 하고 System Call이 완료되면 그때야 Return 한다.
* I/O Multiplexing(Select/Poll)

#### 비동기 + 논 블로킹
* System Call이 완료되지 않아도 나중에 완료되면 그 때 결과물을 가져온다.
* System Call이 완료되지 않아도 대기하지 않고 Return 한다.
* AIO
* I/O 응답이 도착하면 신호나 Callback으로 I/O 전달을 완료한다.
* Node.js

#### 참고
* https://nesoy.github.io/articles/2017-01/Synchronized
* https://homoefficio.github.io/2017/02/19/Blocking-NonBlocking-Synchronous-Asynchronous/
* https://k39335.tistory.com/34
* https://victorydntmd.tistory.com/8
* https://private.tistory.com/24
* https://medium.com/@appear.ko/javascript-async-to-sync-157c57208598
* https://www.slipp.net/questions/367
* http://wiki.sys4u.co.kr/pages/viewpage.action?pageId=7767390
* https://jins-dev.tistory.com/entry/%EB%8F%99%EA%B8%B0Synchronous-%EC%9E%91%EC%97%85%EA%B3%BC-%EB%B9%84%EB%8F%99%EA%B8%B0Asynchronous-%EC%9E%91%EC%97%85-%EA%B7%B8%EB%A6%AC%EA%B3%A0-%EB%B8%94%EB%9D%BDBlocking-%EA%B3%BC-%EB%84%8C%EB%B8%94%EB%9D%BDNonBlocking-%EC%9D%98-%EA%B0%9C%EB%85%90
* https://www.slideshare.net/unitimes/sync-asyncblockingnonblockingio