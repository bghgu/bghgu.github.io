---
layout: post
title: "Spring MVC"
description: "Spring MVC"
date: 2019-04-08
tags: [Spring]
comments: true
share: true
---

#### MVC
* Model - View - Controller
* 사용자 인터페이스와 비즈니스 로직을 분리하여 웹 개발을 하는 것을 가장 큰 장점으로 한다.
* MVC 모델 1, MVC 모델 2가 있는데 요즘 MVC 모델은 MVC 모델 2이다.
* Model : 모델은 애플리케이션의 정보, 즉 데이터를 나타낸다.
* View : 뷰는 사용자에게 보여주는 인터페이스, 즉 화면을 의미한다. 자바 웹 애플리케이션에서는 JSP를 의미하기도 한다.
* Controller : 컨트롤러는 비즈니스 로직과 모델의 상호 동작의 조정 역할을 한다. MVC2에서는 서블릿이 흐름을 제어하는 컨트롤러 역할을 수행한다.

#### Spring MVC
1. 클라이언트의 요청에 대한 최초 진입 지점은 DispatcherServlet이 담당하게 된다. 일종의 front controller이다. 이 servlet이 다음 작업을 처리하게 된다.
2. DispatcherServlet은 Spring Bean Definition에 설정되어있는 Handler Mapping 정보를 참조하여 해당 요청을 처리하기 위한 Controller를 찾는다.
3. DispatcherServlet은 선택된 Controller를 호출하여 클라이언트가 요청한 작업을 처리한다.
4. Controller는 Business Layer와 통신하여 원하는 작업을 처리한 다음 요청에 대한 성공 유무에 따라 ModelAndView 인스턴스를 반환한다. ModelAndView 클래스에는 UI Layer에서 사용할 Model 데이터와 UI Layer로 사용할 View에 대한 정보가 포함되어 있다.
5. DispatcherServlet은 ModelAndView의 View의 이름이 논리적인 View 정보이면 ViewResolver를 참조하여 이 논리적인 View 정보를 실질적으로 처리해야 할 View를 생성하게 된다.
6. DispatcherServlet은 ViewResolver를 통하여 전달된 View에게 ModelAndView를 전달하여 마지막으로 클라이언트에게 원하는 UI를 제공할 수 있도록 한다. 마지막으로 클라이언트에게 UI를 제공할 책임은 View 클래스가 담당하게 된다.

#### Spring MVC 처리 순서
1. 클라이언트가 서버에 어떤 요청(Request)을 한다면 스프링에서 제공하는 DispatcherServlet 이라는 클래스(일종의 Front Controller)가 요청을 가로챈다.
(web.xml에서 모든 url(/)에 서블릿 매핑을 하여 모든 요청을 DispatcherServlet이 가로채게 해놓았다.)
2. 요청을 가로챈 DispatcherServlet은 HandlerMapping에게 어떤 컨트롤러에게 요청을 위임하면 좋을지 물어본다.
(servlet-context.xml에서 @Controller로 등록한 것들을 스캔해서 찾아준다.)
3. 요청에 매핑된 컨트롤러가 있다면 @RequestMapping을 통하여 요청을 처리할 메소드에 도달한다.
4. 컨트롤러에서는 해당 요청을 처리할 Service를 주입(DI) 받아 비즈니스 로직을 Service에게 위임한다.
5. Service에서는 요청에 필요한 작업 대부분(코딩)을 담당하며 데이터베이스에 접근이 필요하면 DAO를 주입받아 DB처리는 DAO에게 위임한다.
6. DAO는 Mybaits/JPA 설정을 이용해서 SQL 쿼리를 날려 DB의 정보를 받아 서비스에게 다시 돌려준다.
(보통 DTO를 컨트롤러에서 부터 내려받아 쿼리의 결과를 담는다.)
7. 모든 로직을 끝낸 서비스가 결과를 컨트롤러에게 넘긴다.
8. 결과를 받은 컨트롤러는 Model객체에 결과물 어떤 view(jsp)파일을 보여줄 것인지 등의 정보를 담아 DispatcherServlet에게 보낸다.
9. DispatcherServlet은 ViewResolver에게 받은 뷰에 대한 정보를 넘긴다.
10. ViewResolver는 해당 JSP를 찾아서(응답할 View를 찾음) Dispatcher에게 알려준다.
(servlet-context.xml에서 suffix, prefix를 통해 /WEB-INF/views/index.jsp 이런 식으로 명을 해주는 것도 ViewResolver)
11. DispatcherServlet은 응답할 View에게 Render를 지시하고, View는 응답 로직을 처리한다.
12. 결과적으로 DispatcherServlet이 클라이언트에게 렌더링 된 View를 응답한다.