---
layout: post
title: "Servlet"
description: "Servlet"
date: 2019-04-08
tags: [web, Servlet]
comments: true
share: true
---

#### Servlet
* Java를 사용해서 웹 페이지를 동적으로 생성하는 서버측 프로그램이다.
* 웹 기반의 요청에 대한 동적인 처리가 가능한 하나의 클래스이다.
* 웹 서버의 성능을 향상하기 위해 사용되는 자바 클래스의 일종이다.
* JSP와 비슷한 점이 있지만, JSP가 HTML에 Java 코드를 포함하고 있는 반면, Servlet은 Java 코드에 HTML을 포함하고 있다.
* 외부 요청마다 Thread로 응답한다.
* Java로 구현되기 때문에 다양한 플랫폼에서 동작한다.

#### Servlet 동작 과정
1. 웹 서버는 HTTP request를 Web(Servlet) Container에게 위임한다.
    * web.xml 설정에서 어떤 URL이 매핑되었는지 확인한다.
    * 클라이언트의 요청 URL을 보고 적절한 서블릿을 실행한다.
2. Web Container는 service() 메소드를 호출하기 전에 Servlet 객체를 메모리에 올린다.
    * Web Container는 적절한 서블릿 파일을 컴파일한다. (.class)
    * .class 파일을 메모리에 로드해 서블릿 객체를 만든다.
    * 메모리에 로드될 때 서블릿 객체를 초기화하는 init() 메소드가 실행된다.
3. Web Container는 Request가 올 때 마다 thread를 생성하여 처리한다.
    * 각 스레드는 서블릿의 단일 객체에 대한 Service() 메소드를 실행한다.

#### Servlet에서 Thread의 역할
* Thread : 운영체제로부터 시스템 자원을 할당받는 작업의 단위
* 서블릿 프로그램에서 스레드가 수행할 메소드가 지정/할당 되면
    * 스레드는 생성 후 즉시 해당 메소드만 열심히 후생한다.
    * 해당 메소드가 반환하면 스레드는 종료되고 제거된다.
    * 실제로 스레드의 역할 : 서블릿의 doGet(), doPost() 호출
* Web Container는 스레드의 생성과 제거를 담당한다.
* 스레드의 생성과 제거의 반복은 큰 오버헤드를 만든다.
* Tomcat(WAS)에서는 Thread Pool이라는 적절한 메커니즘을 사용하여 오버헤드를 줄인다.
* WAS는 Servlet의 생명주기를 담당한다.
    * 웹 브라우저 클라이언트의 요청이 들어왔을 때 서블릿 객체 생성은 WAS가 알아서 처리한다.
    * WAS위에서 서블릿이 돌아다니고 개발자는 이 서블릿을 개발해야 한다.

#### Servlet 생명 주기
1. init()
    * 1번만 수행된다.
    * 클라이언트의 요청에 따라 적절한 서블릿이 생성되고 이 서블릿이 메모리에 로드될 때 init() 메소드가 호출된다.
    * 서블릿 객체를 초기화한다.
2. service()
    * 서블릿이 수신한 모든 request에 대해 service() 메소드가 호출된다.
    * service() 메소드는 request의 타입에 따라 적절한 메소드를 호출한다.
    * 메소드가 반환하면 해당 스레드는 제거된다.
3. destory()
    * 1번만 수행된다.
    * 서블릿 객체를 메모리에서 제거한다.