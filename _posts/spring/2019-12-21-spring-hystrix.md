---
layout: post
title:  "Spring Hystrix"
date:   2019-12-21 17:12:00
author: bghgu
categories: Spring
tags: [Spring]
---

#### Spring Hystrix
* Netflix OSS 중 하나다.
* MSA에서 장애 내성과 지연 내성을 위한 라이브러리이다.
* 최소한의 부하로 운영이 가능하다.
* Circuit Breaker, DashBoard 기능이 있다.
* 내부적으로 RxJava를 사용하고 있다.
    * RxJava : 리액티브 프로그래밍을 자바에서 구현한 프로그래밍 라이브러리이다.
    * 리액티브란 외부에서 자극이 오고 그에 대해 반응 한다는 뜻이다.
* Circuit Breaker 구현체 라고도 한다.
* 서비스를 호출하는 쪽에 적용한다.

#### Circuit Breaker는 기본적으로 @Repository에 붙여준다고 생각한다.
* Circuit Breaker는 외부 API 장애시 timeout까지 대기하게 되면서 본 서비스까지 덩달아 느려지는 것을 방지하기 위한 장치이다.
* Circuit Breaker는 어떤 API가 제대로 응답을 주지 못하는 상황일 때 굳이 매번 요청을 반복하지 않고, 아예 일정 시간 동안 요청을 하지 않고 default 응답을 반환해주도록 한다.
* 그래서 Circuit Breaker는 외부 API에 대한 호출이 발생하는 @Repository, 즉 Persistence Layer에 붙여주는게 일반적이다.

#### Hystrix에 적용된 세 가지 패턴
* Circuit Breaker 패턴 - 차단
    * 평소에는 정상적으로 동작하다가 오류 발생시 더 이상 동작하지 않게 한다.
* Bulkhead 패턴 - 격리
    * 하나가 장애가 발생하더라도 나머지는 정상적으로 작동하도록 애플리케이션의 요소를 여러 풀에 격리한다.
* Fallback 패턴 - 대체
    * 장애가 발생할 경우 대신 실행할 서비스를 실행한다.

#### Circuit Breaker 서킷 브레이커
* 서비스간 의존성이 발생하는 접근 포인트를 분리시켜서 장애 전파를 막는다.
* Fallback 메소드를 제공하여 시스템 장애로부터 복구를 유연하게 한다.
* 스레드 풀 방식과 세마포어 방식이 있다.
* 동기 방식과 비동기 방식이 있다.

#### Thread Pool 방식
* 서비스 호출이 별도의 스레드에서 수행된다.
* 톰캣 스레드 풀과 서비스 스레드 풀이 격리된다.
* 약간의 오버헤드가 발생한다.
* 기본값이다.

#### Thread Pool을 사용하지 않아도 되는 경우
1. 네트워크 Connection/Read timeout, retry 옵션을 사용하여 매우 빨리 실패할 경우
2. 클라이언트가 정상 작동한다는 신뢰가 있을 경우
3. 그냥 스레드 풀을 사용

#### Semaphore 방식
* 서비스 호출을 위한 스레드를 생성하지 않는다.
* 톰캣 스레드를 그대로 사용한다.
* 세마포어(카운트)를 사용하여 종속성에 대한 동시 호출 수를 제한할 수 있다.
* 따라서 스레드를 사용하지 않고 부하를 분산하지만, timeout과 격리가 느슨해지는 단점이 있다.
* 백엔드 서버를 신뢰할 수 있다면 사용해도 괜찮다.

#### 서킷 브레이커 발동 조건
* 20번의 메소드 실행 중, 10번 이상 실패 시 서킷 브레이커가 열린다.
* 19번의 메소드가 실행됐다면, 기본 충족 수 미달로 서킷 브레이커가 발동하지 않는다.

#### 해제 조건
* 5초 이내에 단 하나의 메소드를 다시 실행 시킨 후 성공 시 서킷 브레이커가 닫힌다.
* 실패할 경우 열림이 유지 된다. 이것이 반 열림(Half Open) 상태이다.
* 정상 상태 : Close
* 오류 상태 : Open
* 반 열림 상태 : Half Open

#### Flow Chart
1. HystrixCommand 객체 생성
2. Command 실행
3. 캐시 상태 확인
4. 회로 상태 확인
5. 사용 가능한 스레드 풀/큐/세마포어가 있는지 확인
6. HystrixCommand.run() 실행
7. 회로 상태 연산
8. Fallback 실행
9. 응답 반환

#### Thread Pool을 사용하여 의존성 격리를 구성한 이유
* 스레드를 나누어 다른 스레드에서 접근하기 어렵도록 종속성을 원천 차단한다.
* 애플리케이션은 수없이 많은 팀의, 수없이 많은 백엔드 서비스를 수없이 많이 호출한다.
* 각 서비스는 Client Library를 가지고 있고 항상 바뀐다.
* Client Library는 새로운 네트워크를 호출할 수도 잇고, retry, parsing, caching 등의 로직을 가지며 blockbox로 취급된다.

#### Thread Pool 사용시 장, 단점
* 애플리케이션이 Client Library로 부터 보호된다.
* 새 Client Library을 추가할 때의 Lisk를 낮출 수 있다.
* 장애는 격리된 스레드에서 발생한다.
* Queueing, Scheduling, Context Switch 등의 오버헤드가 발생한다.

#### 스레드 비용
* 10억건 이상의 Hystrix Command를 실행하며, 각 API 인스턴스 마다 5-20개의 스레드를 가지고 있는 스레드 풀을 40+개 설정한다. (대부분의 스레드 풀 내의 스레드 개수는 10개)

#### ThreadLocal, 설정
* 기본적으로 @HystrixCommand는 다른 스레드로 동작하기 때문에, ThreadLocal이나 스프링에서 지원해주는 @RequestScope, @SessionScope Bean에 접근할 수 없다.
* 필요한 경우 execution.isolation.strategy : semaphore로 변경하여 현재 스레드에서 연산을 실행하게 할 수 있다.
* Spring Security를 사용하는 경우 hystrix.shareSecurityContext = true로 설정해서 SecurityContext를 공유할 수 있다.
* 스레드 동작 방식의 경우에는 세마포어 카운트 만큼 요청을 수행할 수 있다.
* Execution.isolation.semaphore.maxConcurrentRequests

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