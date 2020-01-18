---
layout: post
title:  "Orchestration 오케스트레이션"
date:   2020-01-13 16:07:00
author: bghgu
categories: ETC
tags: [ETC]
---

#### 컨테이너 오케스트레이션 개요
* 컨테이너형 애플리케이션의 배포, 스케일링 및 관리의 자동화
* 컨테이너 배포 관리
* 여러개의 컨테이너를 편리하게 관리해주는 작업
* 의도에 맞게 다수 컨테이너를 배포하고 자동화 관리하는 일
* 도커 컨테이너의 갯수가 꾸준히 늘어나면 필요한 자원도 지속적으로 늘어나기 마련이다.
* 때문에 서버 또한 여러대로 늘어날 수 있는데 한대 두대 수준이 아니라 몇십 몇백대의 서버로 늘어났다면, 많은 서버들을 일일이 접근하여 명령어 날려주고 컨테이너 올리고 하다가 시간은 시간대로 흘러버릴수 있다.
* 결론적으로 이 많은 서버들과 컨테이너를 소수의 인원으로 관리하기에는 상당히 어렵고 이 문제를 효율적으로 관리하기 위해 컨테이너 오케스트레이션 툴들이 나오게 되었다.
* 컨테이너 오케스트레이션 툴의 기능에는 단순 컨테이너의 배포 뿐만이 아닌 하나의 서비스를 관리하고 유지 보수 하기 위한 많은 기능들을 포함하고 있다.
* 도커와 쿠버네티스는 사실상 컨테이너와 오케스트레이션의 약칭이다.
* 오케스트레이션 툴의 기능
    * 노드 클러스터링
    * 컨테이너 로드 밸런싱
    * 컨테이너의 배포와 복제 자동화
    * 컨테이너 장애 복구기능
    * 컨테이너 자동 확장 및 축소
    * 컨테이너 스케줄링
    * 로깅 및 모니터링
* 컨테이너 오케스트레이션 툴의 종류는 여러가지가 있지만 그 중 가장 인지도가 높고 자주 소개되는 것은 Docker Swarm, Kubernetes, Apache Mesos이다.

#### 기능 범위
* 컨테이너형 애플리케이션의 배포
* 컨테이너 그룹에 대한 로드밸런싱
* 스케일링/오토스케일링
* 컨테이너 장애 복구
* 컨테이너 그룹 간 격리/연결
* 외부로 서비스 노출
* 서비스 디스커버리
* 로그 수집 집중화/자동화
* 모니터링 집중화/자동화

#### Docker Swarm 도커 스웜
* 도커 스웜은 도커 컨테이너 플랫폼에 통합 된 컨테이너 오케스트레이션 툴이다.
* 장점
    * 도커 명령어와 도커 컴포즈를 포함한 도커의 모든 기능이 내장되어 있다.
    * 도커 이외의 별도의 툴 설치가 필요하지 않다.
    * 타 오케스트레이션 툴에 비해 복잡하지 않고 다루기 쉽다.
* 단점
    * 타 오케스트레이션 툴에 비해 기능이 단순하여 세부적인 설정이 어렵다.
    * 초대형 노드 클러스터링에는 무리가 있다.

#### Kubernetes 쿠버네티스
* 쿠버네티스는 구글에서 개발한 오픈소스화 된 프로젝트이다.
* 구글이 대규모 워크로드 운영 경험과 노하우가 축적된 프로젝트이다.
* 컨테이너 중심의 관리 환경을 제공한다.
* 장점
    * 현재 가장 인지도가 높고 기능이 많은 오케스트레이션 툴이다.
    * 내장된 기능이 많아 타사 애드온이 불필요하다.
* 단점 
    * 쿠버네티스의 구성과 개념에 대한 이해가 필요하다.
    * 학습해야 할 부분이 많고 소규모 프로젝트에서 구축하기 쉽지 않다.

#### Apache Mesos 아파치 메소스
* 아파치 메소스는 트위처, 애플, 우버, 넷플릭스 등 대형 서비스를 운영하고 있는 기업에서 다수 채택 되었으며 마이크로 서비스와 빅데이터, 실시간 분석, 엘라스틱 기능 등을 제공하고 있다.
* 장점
    * 대형 서비스를 운영중인 회사에서 많이 채택 되었고 안정성이 검증되었다.
    * 수만대의 물리적 시스템으로 확장 가능하게 설계 되어있다.
    * Zookeeper, Hadoop, Spark와 같은 응용 프로그램을 연동하여 노드 클러스터링과 자원 최적화가 가능하다.
* 단점
    * 너무 다양한 응용 프로그램의 연동으로 인하여 복잡해질 수 있다.
    * 설치 및 관리가 어렵고 컨테이너를 활용하기 위해 Marathon(마라톤) 프레임워크를 추가로 설치해야 한다.

#### 참고
* https://team-platform.tistory.com/48
* https://developer.ibm.com/kr/technologies/container-orchestration/
* https://zetawiki.com/wiki/%EC%BB%A8%ED%85%8C%EC%9D%B4%EB%84%88_%EC%98%A4%EC%BC%80%EC%8A%A4%ED%8A%B8%EB%A0%88%EC%9D%B4%EC%85%98
* https://www.hpe.com/kr/ko/what-is/container-orchestration.html
* http://blog.bizmerce.com/?p=2533
* https://www.alibabacloud.com/ko/knowledge/what-is-containerization
* http://www.itworld.co.kr/print/104179
* https://www.juniper.net/kr/kr/insights/concert-in-the-cloud/
* http://www.mantech.co.kr/container_orchestration/
* https://www.redhat.com/ko/topics/automation/what-is-orchestration
* https://cleanupthedesk.tistory.com/6
* http://www.zdnet.co.kr/view/?no=20180627151424
* http://www.ciokorea.com/news/135391
* https://conservative-vector.tistory.com/entry/%EC%BF%A0%EB%B2%84%EB%84%A4%ED%8B%B0%EC%8A%A4%EC%99%80-%EB%8F%84%EC%BB%A4%EC%9D%98-%EC%B0%A8%EC%9D%B4
* https://docs.microsoft.com/ko-kr/azure/container-instances/container-instances-orchestrator-relationship
* https://cyberx.tistory.com/175
* 