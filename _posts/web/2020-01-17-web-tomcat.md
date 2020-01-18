---
layout: post
title:  "Tomcat"
date:   2020-01-17 17:41:00
author: bghgu
categories: WEB
tags: [WEB]
---

#### Tomcat 톰캣
* Apache 소프트웨어 재단에서 개발한 Servlet Container(Web Container)만 있는 Web Application Server이다.
* Web Server와 연동하여 실행할 수 있는 자바 환경을 제공한다.
* Servlet이나 JSP를 실행하기 위한 Servlet Container을 제공한다.
* JSP를 포함하고 있는 Servlet 컨테이너이다.
* Servlet 컨테이너는 사용자 입장에서 Servlet을 유지하고 호출하여 실행하는 셀이다.
* 정적 콘텐츠를 로딩하는데 웹 서버보다 수행 속도가 느리다.
* 이를 해결하기 위해서 아파치와 연동한다.
* 아파치는 html 같은 정적인 페이지를 로드하는 데에 사용되는 웹 서버이다.
* 아파치가 실행되면 아파치는 html 파일은 자신이 수행하고 jsp 파일은 톰캣으로 넘겨서 톰캣이 수행하게 만든다.
* 톰캣 특성상 java 언어만 해석이 가능하기 때문에 톰캣에 자체 내장되어 있는 http 서버를 사용하더라도 php 언어, 다른 언어로 작성된 서버 페이지는 실행이 불가능하다.
* 따라서 php와 jsp 모두를 사용하고 싶다면 아파치에서 php를 호출하고 톰캣에서 jsp를 호출하도록 구성하여 상호 보완적 동작을 수행하도록 구성할 수도 있다.
* Tomcat 8.5 기준
    * MaxThreadPool : 200
    * minSpareThreads : 25
    * MaxQueueSize : Integer.MAX_VALUE

#### Apache 아파치
* 소프트웨어 단체 이름이다.
* 아파치 서버라고 부르는 것은 아파치 제단에서 후원하는 오픈소스 프로젝트 커뮤니티에서 만든 http 웹 서버를 지칭하는 말이다.
* 아파치 http 서버는 http 요청을 처리하는 웹 서버인 것이다.

#### 3가지 기능
1. Stand-alone Servlet Containers
    * 내장된 웹 서버의 기능을 사용하는 것이다.
    * 기능면에서 JavaWebServer의 부분인 Servlet 컨테이너와 Java 근간 웹 서버를 사용한다.
2. In-process Servlet Containers
    * Servlet 컨테이너는 웹 서버 플러그인과 Java 컨테이너 구현
    * 웹 서버 플러그인은 웹 서버의 주소 공간 내의 JVM을 열고 그 안에서 Java 컨테이너가 실행되도록 한다.
    * 다중 스레드의 단일 프로세스 서버에 적당하고 퍼포먼스도 좋지만 확장성에 한계가 있다.
3. Out-of-process Servlet Containers
    * 웹 서버 플러그인과 웹 서버의 외부 JVM에서 실행하는 Java 컨테이너 구현
    * 웹 서버 플러그인과 Java 컨테이너 JVM은 몇몇 IPC(보통은 TCP/IP 소켓)를 사용해서 통신
    * Out-of-process 엔진의 반응 시간은 in-process 만큼 좋지 않지만 out-of-process 엔진은 확장성과 안전성 면은 In-process 보다 좋다.

#### Tomcat 디렉터리 구조
* tomcat : Tomcat 루트 폴더
    * bin : 톰캣 서버의 동작을 제어할 수 있는 스크립트 및 실행 파일들이 포함되어 있다.
    * conf : 톰캣의 기본적인 설정 파일들이 포함되어 있다.
    * lib : 아파치와 같은 다른 웹 서버와 톰캣을 연결해주는 바이너리 모듈들이 포함되어 있다.
    * webapps : 톰캣이 제공하는 웹 애플리케이션의 기본 위치이다.
    * logs : 서버의 로그 파일이 저장되는 디렉터리이다.
    * work : JSP 컨테이너와 다른 파일들을 생성하는 임시 디렉터리이다.
    * temp : 임시 저장 폴더

* webapps : 루트 폴더
    * app1 : 웹 애플리케이션 루트 폴더
        * WEB-INF : 웹 애플리케이션 설정 및 참조 클래스 파일
            * classes : java class 파일
            * lib : jar 라이브러리
            * web.xml
        * META-INF
            * context.xml
        * jsp/html 파일
    * app2
    * app3