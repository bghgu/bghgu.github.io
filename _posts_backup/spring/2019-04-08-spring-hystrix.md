---
layout: post
title: "Spring Hystrix"
description: "Spring Hystrix"
date: 2019-04-08
tags: [Spring]
comments: true
share: true
---

#### Spring Hystrix
* Netflix OSS의 하나
* 분산 환경(MSA)에서 장애 내성과 지연 내성을 위한 라이브러리다.
* 최소한의 부하로 운영이 가능하다.
* Circuit Breaker, DashBoard 기능이 있다.
* 내부적으로 RxJava를 사용하고 있다.
* Circuit Breaker 구현체라고도 한다.

#### Circuit Breaker
* 서비스간 의존성이 발생하는 접근 포인트를 분리시켜서 장애 전파를 막는다.
* Fallback를 제공하여 시스템 장애로부터 복구를 유연하게 한다.
* 스레드 풀 방식과 세마포어 방식이 있다.
* 동기 방식과 비동기 방식으로 구성할 수 있다.

##### Thread Pool
* 서비스 호출이 별도의 스레드에서 수행된다.
* Tomcat 스레드 풀과 서비스 호출 스레드 풀이 격리된다.
* 약간의 오버헤드가 발생한다.
* 기본값이다.

##### Semaphore
* 서비스 호출을 위한 스레드를 생성하지 않는다.
* Tomcat 스레드를 그대로 사용한다.

#### Circuit Breaker 발동 조건(기본 값)
* 20번의 메소드 실행 중, 10번 이상 실패 시 서킷 브레이커가 열린다.
* 19번의 메소드가 실행됬다면, 기본 충족수 미달로 서킷브레이커가 발동하지 않는다.

#### Circuit Breaker 해제 조건(기본 값)
* 5초 이내에 단 하나의 메소드를 다시 실행 후 성공 시 서킷 브레이커가 닫힌다. 실패할 경우 열림이 유지 된다.

#### Circuit Breaker 생명 주기
1. HystrixCommand, HystrixObservableCommand 객체 생성
2. Command 실행
3. 캐시 상태 확인
4. 회로 상태 확인
5. 사용 가능한 Thread Pool / Semaphore가 있는지 확인
6. HystrixCommand.run() / HystrixObservableCommand.construc() 실행
7. Calculate Circuit Health 확인
8. Fallback 실행
9. 응답 반환

#### threadPoolProperties 설정 값
* @HystrixCommand Annotation - threadPoolProperties
* coreSize : Thread Pool 갯수
* maximumSize : 최대 Thread Pool 갯수
* allowMaximunSizeToDriverageFromCoreSize : 최대 Queue 크기 속성 활성화
* maxQueueSize : 최대 Queue Size
* queueSizeRejectionThreshold : 거절된 메소드 Queue Size, 최대 큐 크기에 도달하지 않더라도 거부가 발생하는 queue 크기
* thread.timeoutInMilliseconds : 쓰레드 타임아웃 시간
* circuitBreaker.requestVolumeThreshold : 감시 시간 내 요청 갯수 제한

#### Thread Pool을 사용하여 의존성 격리를 구성한 이유
* Thread를 나누어 다른 Thread에 접근하기 어렵도록 종속성을 원천 차단
* Application은 수 없이 많은 back-end service를 끝없이 호출한다.
* 각 서비스는 Client library를 가지고 있다.
* Client library는 항상 바뀐다.
* Client library는 새로운 네트워크를 호출할 수도 있고, retry, parsing, caching 등의 logic을 가진다.

#### Thread Pool 사용 상 장, 단점
* 새 Client library를 추가할 때의 위험성을 낮출 수 있다. 장애는 격리된 Thread에서 발생한다.
* 애플리케이션이 Client library로 부터 보호된다.
* queueing, scheduling, Context Switching 등의 오버헤드가 발생된다.

#### Thread Pool 비용
* Hystrix는 자식 스레드에서 construct(), run()을 실행할 때, 부모 스레드에서 총 종단 시간을 측정하여 오버헤드를 측정한다.
* Netflix에서는 10억 건 이상의 Hystrix Command를 실행하며 각 API 인스턴스마다 5-20개의 Thread를 가지고 있는 Thread Pool을 40+개를 설정한다.(대부분의 Thread Pool 내의 Thread 개수는 10개)

#### Circut Breaker는 어떻게 메소드 성공, 실패를 기록하는가?
* HystrixCommandMetric는 Hystrix Command 마다 하나씩 생성된다.
* HystrixCommandMetric는 HystrixMetrics를 상속한다.
* HystrixCommandMetric 안에 스레드 safe 하기 위해 private static final ConcurrentHashMap<String, HystrixCommandMetrics> metrics 속성이 있다. 
* metrics에 다시 HystrixCommandMetric이 저장된다.
* getInstance 메소드를 통해 HystrixCommandMetric를 가져오거나 저장한다.
* 내부에 static 클래스로 HealthCounts가 있다.
    * private final long totalCount;
    * private final long errorCount;
    * private final int errorPercentage;
    * 에러 퍼센트는 새로 객체를 생성할 때 계산한다.
    * 세 개의 속성을 가지고, 해당 카운트를 변경하기 위해 plus 메소드를 호출한다.
    * plus 메소드에서는 long[] 타입의 eventTypeCounts를 파라미터로 받는다.
    * long successCount = eventTypeCounts[HystrixEventType.SUCCESS.ordinal()];
    * long failureCount = eventTypeCounts[HystrixEventType.FAILURE.ordinal()];
    * long timeoutCount = eventTypeCounts[HystrixEventType.TIMEOUT.ordinal()];
    * long threadPoolRejectedCount = eventTypeCounts[HystrixEventType.THREAD_POOL_REJECTED.ordinal()];
    * long semaphoreRejectedCount = eventTypeCounts[HystrixEventType.SEMAPHORE_REJECTED.ordinal()];
    * 여기에 이전의 long updatedTotalCount = totalCount;, long updatedErrorCount = errorCount; 더한다.
    * updatedTotalCount += (successCount + failureCount + timeoutCount + threadPoolRejectedCount + semaphoreRejectedCount);
    * 총 카운트 : 성공 카운트 + 실패 카운트 + 시간 초과 카운트 + 스레드 거절 카운트 + 세마포어 거절 카운트 + 이전의 총 카운트
    * updatedErrorCount += (failureCount + timeoutCount + threadPoolRejectedCount + semaphoreRejectedCount);
    * 에러 카운트 : 실패 카운트 + 시간 초과 카운트 + 스레드 거절 카운트 + 세마포어 거절 카운트 + 이전의 에러 카운트
    * return new HealthCounts(updatedTotalCount, updatedErrorCount);
    * 새로 객체를 생성한다.
    * 이 puls 메소드를 호출하는 곳은 어디인가??
* healthCounts의 plus 메소드는 HealthCountsStream에서 사용된다.
* HystrixCircuitBreaker 인터페이스에 makeSuccess메소드는 HystrixCommandMetrics의 resetStream을 실행한다.
* resetStream메소드의 마지막에 healthCountsStream = HealthCountsStream.getInstance(key, properties); 이 있다.
* HealthCountsStream의 getInstance를 할 때 마다 새로 객체가 생성된다.
* AbstractCommand에는 protected final HystrixCircuitBreaker circuitBreaker; 이 있다.
* AbstractCommand의 executeCommandAndObserved Observable에서 circuitBreaker의 markSuccess 메소드를 실행하는 Action1 객체를 생성한다.(RxJava 문법 인듯)
* 이 모든것은 HystrixCommand.run()메소드 실행시 동작