---
layout: post
title: "Tomcat"
description: "Tomcat"
date: 2019-04-08
tags: [web, WAS, Tomcat]
comments: true
share: true
---

#### Tomcat
* Apache 소프트웨어 재단에서 개발한 Servlet Container(Web Container)만 있는 Web Application Server(WAS)이다.
* Web Server와 연동하여 실행할 수 있는 자바 환경을 제공한다.
* Servlet이나 JSP를 실행하기 위한 Servlet Container을 제공한다.
* 정적 컨텐츠를 로딩하는데 웹 서버보다 수행 속도가 느리다.
* Tomcat 8.5 기준
    * MaxThreadPool : 200
    * minSpareThreads : 25
    * MaxQueueSize : Integer.MAX_VALUE