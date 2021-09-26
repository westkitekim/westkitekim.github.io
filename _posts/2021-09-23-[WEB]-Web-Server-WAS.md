---
title: "[WEB] Web Server와 WAS"
excerpt:

categories:
  - Web
tags:
  [Web, java, Javascript]

toc: true
toc_sticky: true

date: 2021-09-26
last_modified_at: 2021-09-26
---

##  들어가기 전에

### 1. 웹 문서란? 

- 웹에서 클라이언트가 서버에 정보를 요청하면 응답하는 컨텐츠

- 정적인 웹 문서(Static Pages)와 동적인 웹 문서(Dynamic Pages)로 구분된다.

  #### Static Pages 

  - Wer Server가 클라이언트에게 요청받은 URL경로와 일치하는 file contents를 반환한다.
  - Web Server가 이미 만들어져 있는 문서를 클라이언트에게 제공한다.  
    = 문서를 변경하지 않는 한 항상 같은 내용의 문서를 클라이언트에게 제공 
  - ex) HTML, GIF, JPG, PDF, PPT .. etc

  #### Dynamic Pages

  - 요청이 들어올 때마다 다른 웹 문서의 내용을 클라이언트로 전달한다. 
    - 두 가지 방식에 의해 처리
      1. 웹 문서에 동적인 요소를 포함하는 방식(script 방식 - JSP)
      2. 서버가 다른 어플리케이션을 통해 웹 문서를 재생성하여 제공하는 방식(Servlet: WAS 위에서 돌아가는 Java Program) 
  - ex) CGI, ASP, PHP, Servlet, JSP .. etc

### 2. Web  Application

- **동적인 기능**을 바탕으로 웹 브라우저에서 수행되고 이용할 수 있는 응용 소프트웨어(애플리케이션)

  - 간단히 말해 우리가 사용하는 모든 웹 사이트 중에서도 동적인 기능이 가능한 웹 사이트라고 할 수 있다. 

  - 여기서 동적인 기능이란 클라이언트(사용자)와의 상호작용이 가능한 것을 말한다.
    - ex) 페이스북에서 사용자가 글을 업로드하고 댓글을 작성할 수 있는 기능(서버에 의한 화면 변경이 아닌 클라이언트에 의한 화면 변경), 메일을 송수신 할 수 있는 메일링사이트 .. etc 
  - 웹 사이트와 비슷한 느낌이지만 웹 사이트는 정적인 컨텐츠를 제공한다고 볼 수 있다. 
    - ex) 정적인 정보만을 제공하는 뉴스 페이지나, 프레임워크나 API의 설명 사이트 등이 있다. 
  - 하지만 웹 사이트와 웹 애플리케이션을 정확히 나누는 것은 어렵다 - 웹 애플리케이션은 웹 사이트의 성격도 가지고 있기 때문

- 수행되는 위치에 따라서 Client Side(웹 브라우저에서 수행), Server Side(웹 서버에서 수행) 로 나뉜다. 

<br>

# Web Server  와 WAS(Web Application Server)

![WAS architecture](https://user-images.githubusercontent.com/88620416/134767557-51fc8327-2e0e-4649-8859-678647402504.png)

## Web Server 

### 	1. Web Server 란? 

- 웹에서 서버기능(제공자)을 수행하는 프로그램

- 소프트웨어와 하드웨어로 구분 

  1. 하드웨어  : Web Server 가 설치되어 있는 실제 물리적인 컴퓨터 자체
   2. 소프트웨어 : HTTP 요청에 따라 클라이언트에게 정적인 컨텐츠( .html .jpeg .css파일 ) 등를 제공하는 컴퓨터 프로그램

  - 하드웨어와 소프트웨어로 나눌 수 있지만 Web Server라고 하면 보통 통칭하여 의미하는 경우도 있다. 

### 	2. Web Server 기능

- **HTTP 프로토콜을 기반으로 동작**하여 HTTP 서버라고도 하며, **웹 클라이언트(=웹 브라우저)로부터의 요청을 서비스**하는 기능을 담당한다. 
- HTTP에 의거해 HTML 기반의  **정적인 컨텐츠를 웹 애플리케이션 단위로 서비스**한다. 
  1. *<u>클라이언트의 요청이 정적인 컨텐츠인 경우</u>*
     - 클라이언트가 요청한 웹 문서를 WAS를 거치지 않고 Web Server가 문서를 찾아서 전달한다 (정적인 컨텐츠)
  2. *<u>클라이언트의 요청이 동적인 컨텐츠인 경우</u>* (Web Server가 처리X, WAS에게 전달)
     - 서버 프로그램에 대한 요청을 WAS에 전달하고, WAS에서 수행한 결과를 클라이언트에게 전달(Response 응답) 한다. 
  3. 요청 파일이 없거나 문제 발생 시 정해진 코드 값으로 응답한다.
     - ex) 404, 405 ... 
  4. 클라이언트의 요청에 대한 기본 사용자 인증(Basic Authentication)을 처리한다.



### 	3. Web Server 의 종류 

- Apache server, Nginx.. etc

<br>

<style>#span1{background-color: #e9e97f;}</style>

## WAS(Web Application Server)

### 	1. WAS 란?

- <span id="span1">다양한 기능을 수행하는 Business Logic(DB 조회 및 제어 등) 처리가 필요한 **동적인 컨텐츠를 제공**하기 위해 만들어진 Application Server <span>

- **WAS = Web Server + Web Container**

  - Container란? 
    - Servlet과 JSP와 같은 웹 컴포넌트를 저장하는 저장소 역할, 메모리 로딩, 객체 생성 및 초기화, Servlet의 생명주기 관리 및 JSP를 Servlet으로 변환하는 기능을 수행하는 프로그램
    - Servlet Container : 클라이언트와 요청에 따라 서블릿을 수행하는 프로그램
    - JSP Container : JSP를 Servlet으로 변환 , 컴파일하는 프로그램
    - 대부분의 WAS에서는 Servlet Container 와 JSP Container를 내장하고 있기 때문에 Servlet과 JSP 구동환경을 제공한다.

  **Java EE Server**, **웹 컨테이너(Web Container)** 혹은 **서블릿 컨테이너(Servlet Container)**라고도 한다.

- 웹 서버의 기능을 구조적으로 분리하여 처리하려는 목적으로 사용

  - 웹 기반의 요청과 C/S(Client/Server) 환경 기반의 요청을 각각 분리하여 개별 처리하도록 구축한다. 
  - 웹 서버 단독으로 처리 시 서버의 처리량이 많아지기 때문에 속도 및 보안 성능에 문제가 발생한다.

### 	2. WAS 기능 

1. 프로그램 실행환경과 DBCP(DataBase Connection Pool) 접속 기능 제공
2. 여러 개의 트랜잭션(논리적인 작업 단위) 및 트래픽 관리 
3. 업무를 처리하는 비즈니스 로직 수행
4. 보안, 사용자 관리 등 강력한 기능 제공 

### 	3. WAS 종류

웹로직(WebLogic), 웹스피어(WebSphere), 제우스(Jeus), 제이보스(JBoss), 톰캣(Tomcat) ... etc

<br><br><br><br>

## References

- [https://gmlwjd9405.github.io/2018/10/27/webserver-vs-was.html](https://gmlwjd9405.github.io/2018/10/27/webserver-vs-was.html)
- https://ko.wikipedia.org/wiki/%EC%9B%B9_%EC%95%A0%ED%94%8C%EB%A6%AC%EC%BC%80%EC%9D%B4%EC%85%98
- [https://geonlee.tistory.com/90](https://geonlee.tistory.com/90)



























