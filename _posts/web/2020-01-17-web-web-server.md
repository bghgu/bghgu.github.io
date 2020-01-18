---
layout: post
title:  "Web Server"
date:   2020-01-17 19:56:00
author: bghgu
categories: WEB
tags: [WEB]
---

#### Web Server 웹 서버
* 클라이언트의 요청을 받아 html이나 오브젝트를 http 프로토콜을 이용해 전송하는 것이다.
* 정적 컨텐츠를 관리한다. (html, css, js, 이미지 등)
* Apache, Nginx
* 파일 경로 이름을 받아 경로와 일치하는 file contents를 반환한다.
* 항상 동일한 페이지를 반환한다.
* WAS를 거치지 않고 바로 자원을 제공한다.
* 동적인 컨텐츠 제공을 위한 요청 전달
* 클라이언트의 Request를 WAS에 보내고, WAS가 처리한 결과를 클라이언트에게 Response 한다.
* 클라이언트는 일반적으로 웹 브라우저를 의미한다.

#### Web Server가 필요한 이유
* 클라이언트(웹 브라우저)에 이미지 파일(정적 컨텐츠)을 보내는 과정
* 이미지 파일과 같은 정적인 파일들은 웹 문서(HTML 문서)가 클라이언트로 보내질 때 함께 가는 것이 아니다.
* 클라이언트는 HTML 문서를 먼저 받고 그에 맞게 필요한 이미지 파일들을 다시 서버로 요청하면 그때서야 이미지 파일을 받아온다.
* Web Server를 통해 정적인 파일들을 Application Server까지 가지 않고 앞단에서 빠르게 응답해줄수 있다.
* 따라서 Web Server에서는 정적 컨텐츠만 처리하도록 기능을 분배하여 서버의 부담을 줄일 수 있다.

#### WAS가 필요한 이유
* 웹 페이지는 정적 컨텐츠와 동적 컨텐츠 모두 존재한다.
* 사용자의 요청에 맞게 적절한 동적 컨텐츠를 만들어서 제공해야 한다.
* 이때, Web Server만을 이용한다면 사용자가 원하는 요청에 대한 결과값을 모두 미리 만들어놓고 서비스를 해야 한다.
* 하지만 이렇게 수행하기에는 자원이 절대적으로 부족하다.
* 따라서 WAS를 통해 요청에 맞는 데이터를 DB에서 가져와서 비즈니스 로직에 맞게 그때 그때 결과를 만들어서 제공함으로써 자원을 효율적으로 사용할 수 있다.

#### WAS가 Web Server의 기능도 모두 수행?
1. 기능을 분리하여 서버 부하 방지
    * WAS는 DB 조회나 다양한 로직을 처리하느라 바쁘기 때문에 단순한 정적 컨텐츠는 Web Server에서 빠르게 클라이언트에 제공하는 것이 좋다.
    * WAS는 기본적으로 동적 컨텐츠를 제공하기 위해 존재하는 서버이다.
    * 만약 정적 컨텐츠 요청까지 WAS가 처리한다면 정적 데이터 처리로 인해 부하가 커지게 되고, 동적 컨텐츠의 처리가 지연됨에 따라 수행 속도가 느려진다.
    * 즉, 이로 인해 페이지 노출 시간이 늘어나게 될 것이다.
2. 물리적으로 분리하여 보안 강화
    * SSL에 대한 암복호화 처리에 Web Server를 사용
3. 여러 대의 WAS를 연결 가능
    * 로드 밸런싱을 위해서 Web Server를 사용
    * 장애 극복, fail back 처리에 유리
    * 특히 대용량 웹 애플리케이션의 경우(여러 개의 서버 사용) Web Server와 WAS를 분리하여 무중단 운영을 위한 장애 극복에 쉽게 대응할 수 있다.
    * 예를 들어, 앞 단의 Web Server에서 오류가 발생한 WAS를 이용하지 못하도록 한 후 WAS를 재시작함으로써 사용자는 오류를 느끼지 못하고 이용할 수 있다.
4. 여러 웹 애플리케이션 서비스 가능
    * 예를 들어, 하나의 서버에서 PHP, Java를 함께 사용하는 경우
5. 기타
    * 접근 허용 IP 관리, 2대 이상의 서버에서의 세션 관리 등도 Web Server에서 처리하면 효율적이다.
* 즉, 자원 이용의 효율성 및 장애 극복, 배포 및 유지보수의 편의성을 위해 Web Server와 WAS를 분리한다.
* Web Server를 WAS 앞에 두고 필요한 WAS들을 Web Server에 플러그인 형태로 설정하면 더욱 효율적인 분산 처리가 가능하다.

#### Web Service Architecture
* Client -> Web Server -> DB
* Client -> WAS -> DB
* Client -> Web Server -> WAS -> DB
1. Web Server는 웹 브라우저 클라이언트로부터 HTTP 요청을 받는다.
2. Web Server는 클라이언트의 요청(Request)을 WAS에 보낸다.
3. WAS는 관련된 Servlet을 메모리에 올린다.
4. WAS는 Web.xml을 참조하여 해당 Servlet에 대한 스레드를 생성한다. (스레드 풀 이용)
5. HttpServletRequest, HttpServletResponse 객체를 생성하여 서블릿에 전달한다.
    1. 스레드는 서블릿의 service() 메소드를 호출한다.
    2. service() 메소드는 요청에 맞게 doGet(), doPost() 메소드를 호출한다.
6. doGet(), doPost() 메소드 인자에 맞게 생성된 적절한 동적 페이지를 Response 객체에 담아 WAS에 전달한다.
7. WAS는 Response 객체를 HttpResponse 형태로 바꾸어 Web Server에 전달한다.
8. 생성된 스레드를 종료하고, HttpServletRequest와 HttpServletResponse 객체를 제거한다.

#### DBMS와 MiddleWare의 개념
* DBMS(Database Management System)
    * 다수의 사용자들이 DB내의 데이터를 접근할 수 있도록 해주는 소프트웨어
    * DBMS는 보통 Server 형태로 서비스를 제공한다.
    * MySQL, MariaDB, Oracle, PostgreSQL
    * DBMS Server에 직접 접속해서 동작하는 클라이언트 프로그램의 문제점?
        * 클라이언트에 로직이 많아지고 이에 따라 클라이언트 프로그램의 크기가 커진다.
        * 로직이 변경될 때마다 매번 배포가 되어야 한다.
        * 클라이언트에 대부분의 로직이 포함되어 배포가 되기 때문에 보안에 취약하다.
        * 이를 해결하기 위해 MiddleWare가 등장했다.
* MiddleWare
    * Client - MiddleWare Server - DB Server(DBMS)
    * 동작 과정
        * 클라이언트는 단순히 요청만 중앙에 있는 MiddleWare Server에게 보낸다.
        * MiddleWare Server에서 대부분의 로직이 수행된다.
        * 이때, 데이터를 조작할 일이 있으면 DBMS에 부탁한다.
        * 로직의 결과를 Client에게 전송한다.
        * Client는 그 결과를 화면에 보여준다.
    * 즉, 비즈니스 로직을 클라이언트와 DBMS 사이의 MiddleWare Server에서 동작하도록 함으로써 클라이언트는 입력과 출력만 담당하게 된다.