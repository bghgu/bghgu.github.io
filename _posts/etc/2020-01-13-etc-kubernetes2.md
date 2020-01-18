---
layout: post
title:  "Kubernetes"
date:   2020-01-13 12:42:00
author: bghgu
categories: ETC
tags: [ETC, Kubernetes]
---

#### What is Kubernetes?
* kubernetes(이하 k8s)는 docker-swarm, marathon과 같은 container orchestration 툴이다.
* host 머신 한대라면 docker run이나 docker-compose 등 무엇으로 컨테이너를 실행해도 상관 없다.
* 그러나 사용자가 많아지면 하나의 host에서 모든 컨테이너를 실행할 수 없다.
* 그라나 여러대의 host에서 컨테이너를 실행하려면 inter-host container 네트워킹과, host machine의 리소스를 고려한 container 분배 등을 고려해야 한다.
* 이런 문제들을 해결해주는 것이 바로 container orchestration 툴이다.

#### 쿠버네티스가 하는 일
* 여러 호스트(= node in k8s)를 묶어 클러스터를 구성한다.
* 컨테이너를 적절한 위치에 배포한다. (auto-placement)
* 컨테이너가 죽으면 자동으로 복구한다. (auto-restart)
* 필요에 따라 컨테이너를 매끄럽게 추가(scaling), 복제(replication), 업데이트(rolling update), 롤백(rollback) 할 수 있다.

#### 쿠버네티스 아키텍처(abbreviated)
* 하나의 쿠버네티스 클러스터는 하나의 마스터와 여러개의 노드로 구성되어 있다.
* 개발자는 kubectl을 이용해서 마스터에 명령을 내리고, 노드를 관리하는 반면 사용자(endpoint user)는 노드에 접속해 서비스를 이용한다.
* 마스터에는
    * 작업을 위한 api server
    * state를 관리하기 위한 분산 저장소 (default는 etcd)
    * scheduler
    * controller manager 등이 있다.
* node(= minion) 에는
    * 마스터와 통신하는 kubelet
    * 외부의 요청을 처리하는 kube-proxy
    * 컨테이너 리소스 모니터링을 위한 cAdviser 등이 있다.

#### Pod(팟)
* 하나 또는 여러개의 컨테이너 묶음을 pod(팟)이라 부른다.
* 도커에서 컨테이너끼리 통신하려면 같은 네트워크 위에 있도록 구성해야 하는 반면 (compose 도 동일), 하나의 pod 내에 있는 컨테이너 끼리는 그럴 필요가 없다.
* 같은 IP와 port space를 가지기 때문에 로컬 호스트로 통신이 가능하며
* volume을 공유한다.
* 만약 어느 컨테이너가 죽고 재시작되어도 팟이 살아있는 한 shared volume은 유지된다.

#### 참고
* https://www.popit.kr/kubernetes-introduction/