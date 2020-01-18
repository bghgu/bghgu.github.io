---
layout: post
title:  "Spring Dispathcer Servlet"
date:   2020-01-16 15:44:00
author: bghgu
categories: Spring
tags: [Spring]
---

#### Spring Dispatcher Servlet 디스패처 서블릿
* Servlet Container에서 HTTP 프로토콜을 통해 들어오는 모든 요청을 프로젠테이션 계층의 제일 앞에 둬서 중앙집중식으로 처리해주는 프론트 컨트롤러(Front Controller)이다.
* 클라이언트로부터 어떠한 요청이 오면 Tomcat(톰캣)과 같은 서블릿컨테이너가 요청을 받는데, 이때 제일 앞에서 서버로 들어오는 모든 요청을 처리하는 프론트 컨트롤러를 스프링에서 정의하였고, 이를 디스패처 서블릿이라고 한다.
* 그래서 공통처리 작업을 디스패처 서블릿이 처리한 후, 적절한 세부 컨트롤러로 작업을 위임해준다.
* 물론 디스패처 서블릿이 처리하는 url 패턴을 지정해주어야 하는데 일반적으로는 /*.do와 같이 /로 시작하며, .do로 끝나는 url 패턴에 대해서 처리하라고 지정해준다.
* Spring MVC Framework의 유일한 Front Controller인 Dispatcher Servlet은 Spring MVC의 핵심 요소이다.

#### Front Controller
* Front Controller는 주로 서블릿 컨테이너의 제일 앞에서 서버로 들어오는 클라이언트의 모든 요청을 받아서 처리해주는 컨트롤러인데, MVC 구조에서 함께 사용되는 패턴이다.

#### Dispatcher Servlet의 장점
* Spring MVC는 Dispatcher Servlet이 등장함에 따라 web.xml의 역할을 상당히 축소시켜주었다.
* 기존에는 모든 서블릿에 대해 URL 매핑을 활용하기 위해서 web.xml에 모두 등록해주어야 했지만, dispatcher-serlvet이 해당 애플리케이션으로 들어오는 모든 요청을 핸들링해주면서 작업을 상당히 편리하게 할 수 있게 되었다.
* 그리고 이 서블릿을 이용한다면 @MVC 역시 사용할 수 있게되어 좋다.
* Dispatcher Servlet이 요청을 Controller로 넘겨주는 방식은 효율적으로 보인다.
* 하지만 모든 요청을 처리하다보니 이미지나 HTML 파일을 불러오는 요청마저 전부 Controller로 넘겨버린다.
* 게다가 JSP 파일 안의 JavaScript나 CSS 파일들에 대한 요청들 까지도 모두 디스패처 서블릿이 가로채는 까닭에 자원을 불러오지 못하는 상황도 발생하곤 했다.
* 이에 대한 해결책은 두가지가 있다.
1. 클라이언트의 요청을 2가지로 분리하여 구분하는 것이다.
    * /apps의 URL로 접근하면 디스패처 서블릿이 담당한다.
    * /resources의 URL로 접근하면 디스패처 서블릿이 컨트롤할 수 없으므로 담당하지 않는다.
* 이러한 방식은 괜찮지만 상당히 코드가 지저분해지며, 모든 요청에 대해서 저런 UR을 붙여주기 때문에 직관적인 설계가 될 수 없다.
2. 모든 요청을 컨트롤러에 등록하는 것인데, 무식한 방법이다.
* 스프링은 이러한 문제들을 해결함과 동시에 편리한 방법을 제공해주느데, 그것은 바로 <mvc:resources />를 이용한 방법이다.
* 이것은 만약 디스패처 서블릿에서 해당 요청에 대한 컨트롤러를 찾을 수 없는 경우에, 2차적으로 설정된 경로에서 요청을 탐색하여 자원을 찾아내는 것이다.
* 이렇게 영역을 분리하면 효율적인 리소스 관리를 지원할 뿐 아니라 추후에 확장을 용이하게 해준다는 장점이 있다.

#### DispatcherServlet에서의 웹 요청 흐름
1. doService 메소드에서부터 웹 요청의 처리가 시작된다. 디스패처 서블릿에서 사용되는 몇몇 정보를 request 객체에 담는 작업을 한 후 doDispatch 메소드를 호출한다.
2. 아래 3 ~ 13번 작업이 doDispatch 메소드안에 잇다. Controller, View 등의 컴포넌트들을 이용한 실제적인 웹요청처리가 이루어진다.
3. getHandler 메소드는 RequestMapping 객체를 이용해서 요청에 해당하는 Controller를 얻게 된다.
4. 요청에 해당하는 Handler를 찾았다면 Handler를 HandlerExecutionChain 객체에 담아 리턴하는데, 이때 HandlerExecutuinChain은 요청에 해당하는 Interceptor들이 있다면 함께 담아 리턴한다.
5. 실행될 인터셉터들이 있다면 인터셉터의 preHandle 메소드를 차례로 실행한다.
6. Controller의 인스턴스는 HandlerExecutionChain의 getHandler 메소드를 이용해서 얻는다.
7. HandlerMapping과 마찬가지로 여러개의 HandlerAdaptor를 설정할 수 있는데, getHandlerAdaptor 메소드는 Controller에 적절한 HandlerAdaptor 하나를 리턴한다.
8. 선택된 HandlerAdaptor의 handle 메소드가 실행되는데, 실제 실행은 파라미터로 넘겨 받은 Controller를 실행한다.
9. 계층형 Controller인 경우는 handleRequest 메소드가 실행된다. @Controller인 경우는 HandlerAdaptor(AnnotationMethodHandlerAdapter)가 HandlerMethodInvoker를 이용해 실행할 Controller의 메소드를 invoke()한다.
10. 인터섭터의 postHandle 메소드가 실행된다.
11. resolveViewName 메소드는 논리적 뷰 이름을 가지고 해당 View 객체를 반환한다.
12. Model 객체의 데이터를 보여주기 위해 해당 View 객체의 render 메소드가 수행된다.

#### 참고
* https://mangkyu.tistory.com/18
* https://hermeslog.tistory.com/156