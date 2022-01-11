---
title: "[Web] 상태정보 유지기술 - Cookie, Session"

categories:
  - Web
tags:
  [Web, java, Servlet,JSP]

toc: true
toc_sticky: true

date: 2021-10-04
last_modified_at: 2021-10-04
---

<style>
    .highlight{background-color: e9e97f;}
</style>


## 0. 상태정보 유지 기술의 필요성

- 인터넷 웹 서비스에서 클라이언트와 서버가 통신하기 위한 규약으로  HTTP 프로토콜을 사용한다.

- HTTP 프로토콜의 특징

  1. **무연결(Connectionless)** : 서버가 요청에 대한 응답을 하고나서 연결을 끊는다. 그리고 다시 요청이 들어오면 이전과는 다른 연결을 맺는데, 이 때의 연결은 이전과는 전혀 연관이 없는 새로운 연결이 되는 특성이다.

  2. **무상태(Stateless)** : 상태정보가 유지되지 않는 특성. 클라이언트의 각각의 요청마다 서로 다른 연결로 인식을 하기 때문에 요청 사이에 정보가 공유되지 않는다.(why? - 모든 정보를 유지하면 서버에 그만큼 부담이 가중된다)

     ex) 상태정보 유지기술없이 HTTP의 특성만으로 웹을 구현한다면? - 로그인을 한 다음, 클라이언트가 다른 요청을 하게 되면 로그인 상태가 사라지게 된다. 

     이를 테면 로그인을 하고 나서 메일을 확인하는 과정을 생각해보자. 메일을 확인 요청을 보내게 되면 다른 요청이기 때문에 클라이언트가 로그인했던 상태는 사라지게되고 메일을 확인할 수 없게 된다.

     - 응답(response)이 끝난 상태의 HTML

     

     ![html아무것도몰라](https://user-images.githubusercontent.com/88620416/135848540-da1ab4d7-2040-470d-9976-87e97fdd99b4.png)

  3. **요청 및 응답(Request / Response)** : 클라이언트와 서버 사이에 요청정보와 응답정보를 주고받으며 통신이 이루어 지는 방식

- 엄밀히 따지자면 HTTP의 특징 중 <u>비연결성(무연결, Connectionless)</u>, <u>무상태(Stateless)</u> 라는 특징으로 인해 클라이언트와 서버가 이전에 연결되었던 상태정보를 서버가 유지해 주지 않는다. 따라서 일정 시간 동안 상태정보를 유지해주는 기술이 필요한 것이다. 

<br>

## 1. 상태정보 유지 기술의 분류

### 1 - 1. 저장 위치에 따른 분류

#### 		(1)  클라이언트에 저장

클라이언트, 즉 웹 브라우저에 저장되는 상태정보 유지 기술

- **쿠키 Cookie** - javax.servlet.http.Cookie 객체 안에 쿠키의 기능이 구현되어 있다.

#### 		(2) 서버에 저장

서버에 저장한다는 것은 서버의 힙(Heat) 메모리 영역에 만들어진 객체에 상태정보를 저장(등록)하는 것을 의미한다. 힙 영역에 상태정보가 있는 객체가 존재하는 한, 등록된 상태정보를 유지하여 사용 가능하다.

- **세션 Session** - javax.servlet.http.HttpSession 
- **ServletContext** - javax.servlet.ServletContext
- **Request** - javax.servlet.http.HttpServletRequest

### 1 - 2. 유지 기간에 따른 분류 

#### 		(1)  웹 어플리케이션 단위

웹 어플리케이션이 실행되고 있는 동안 유지 

- ServletContext

#### 		(2) 클라이언트 단위

각각의 클라이언트(웹 브라우저)별로 구분하여 상태정보 유지	ex) 로그인

- Cookie
- Session

#### 		(3) 요청 단위 

클라이언트로부터 요청이 들어오고 응답이 나가기까지 하나의 요청에서만 상태정보 유지 

- Request

> **<span class="highlight">Cookie 와 Session은 유지하려는 정보의 위치가 클라이언트인지 서버인지에 따라 분류된다.</span>**

<br>

# 2. Cookie

----

> **쿠키란,** 
>
> **서버가 클라이언트에 저장하는 정보로서 클라이언트 쪽에 필요한 정보를 저장해놓고 필요할 때 추출하는 기술이다.**

쿠키는 인터넷을 이용할 때 언제나 자연스럽게 이용하고 있다.(물론 세션도..)

쿠키의 예시들은 다음과 같다. 

- '오늘 하루 그만보기'와 같은 팝업창
- 이전에 검색했던 키워드를 나중에 사용시 자동완성해주는 기능
- 로그인을 하고 난 후 사용자가 로그아웃할 때까지 로그인 상태 유지(+세션과 함께)
- 쇼핑몰에서 장바구니에 담았던 상품 정보 유지
- 이전에 방문했던 웹 서버에 재방문 했을 때 몇 번째 방문인지 출력하는 상황 



### 2 - 1. Cookie의 특성

-  서버를 통해 **name과 value로 구성된 사용자(User) 상태 정보**가 **클라이언트(웹 브라우저) 에 저장**된다.
  - 클라이언트 측에 저장되면, 사용자(=클라이언트)가 데이터를 직접 확인할 수 있게 되기 때문에  <u>보안상 문제</u>가 발생할 수 있다는 단점이 발생한다. 따라서 보안상 중요한 정보를 저장할 때는 뒤에 기술하게 될 Session에 저장한다.  
- 저장할 수 있는 용량의 제한 - 4kb
- 데이터 타입 : 문자열만 가능 및 공백 불가 - Cookie 객체에 들어가는 name과 value에 해당
- Cookie 유효 시간을 별도로 지정하지 않으면 브라우저 실행 시에만 유효(브라우저가 종료되면 쿠키도 사라진다)

### 2 - 2. Cookie 생성과정 

클라이언트로부터 요청이 들어오면 다음과 같이 생성한다

1. 쿠키 생성 : new Cookie(String name, String value)

   - Cookie를 생성하려면 Servlet 파일에서  java.servlet.http.Cookie 객체를 생성해야한다.
   - 클라이언트에서 서버에게 요청을 하면 서버측에서 쿠키를 생성한다. 
     그 다음 서버가  response를 할 때 만든 쿠키를 담아서 보내기 때문에 클라이언트 측에 쿠키가 저장된다.

2. 쿠키 유효 시간 설정 : setMaxAge(int expiry)

   - 쿠키의 유효 시간을 설정할 때 사용하는 메서드다. 인자값에 들어가는 정수는 유효 시간의 초(second)를 의미한다. ex) setMaxAge(60 * 60 * 24 ) - 하루동안 쿠키 유지

3. 쿠키 경로 설정 : setPath(String url)

   - 특정 경로의 요청에서만 쿠키를 전달하고자 할 때 setPath() 메서드로 경로를 지정한다.
   - 지정된 경로와 그 하위 경로의 요청에 대해서만 클라이언트로부터 쿠키가 전송된다.

4. 쿠키 도메인 설정 : setDomain(String domain)

   - 쿠키는 기본적으로 전송된 서버에서만 읽어 들어 사용할 수 있지만, 여러 대의 서버가 연결되어 서비스하는 경우가 있다면? 

     그 때 setDomain 메서드를 이용해서  A서버에서 클라이언트로 전송된 쿠키를 다른 B서버에서 읽어서 사용한다. 

5.  쿠키 전송 : addCookie(Cookie cookie)

   - 서버에서 쿠키를 생성하고 클라이언트로 보낼 때는 HttpServletResponse 객체의 addCookie() 메서드를 사용한다. addCookie() 메서드의 인자값으로 쿠키 객체를 설정하여 클라이언트에게 쿠키를 전송한다
   - 이렇게 클라이언트에 저장된 쿠키는 이후에 서버에 요청을 할 때 자동으로 요청 정보의 헤더 안에 포함되어 서버에게 전달된다. 

<br>

쿠키를 사용하려면 **쿠키 생성(new Cookie(String name, String value)**과 **쿠키 전송(addCookie(Cookie cookie))** 과정은 **필수**임을 잊지말자.

아래는 Servlet파일에서 구현되는 Cookie 생성 코드이다. 

```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    ....
    String time = new Date().toString().replace(" ", "-");
    //오늘의 날짜 데이터가 담겨있는 쿠키 생성, 쿠키의 이름은 time
	Cookie cookie = new Cookie("time",time); 
	cookie.setMaxAge(60);//60초 동안 유지
	response.addCookie(cookie);//서버가 응답시에 쿠키 객체를 담아 전송한다
    ...
    ..
}
```

<br>

###  2 - 3. Cookie 추출 

- 클라이언트가 접속하면 서버측에서 클라이언트의 쿠키를 확인하고 특정 쿠키의 value를 얻어와서  출력한다

1. 쿠키 추출 : Cookie[] getCookies() 	

   - HttpServletRequest 객체의  getCookies() 메서드를 통해 쿠키들을 배열로 받을 수 있다. 

2. 쿠키 검색 : String getName()

   - 쿠키의 이름을 추출하는 메서드 

   - getCookies() 메서드는 서버가 전송했던 쿠키들을 한꺼번에 배열로 반환하기 때문에 원하는 쿠키 작업이 필요하다. 이러한 작업을 처리하는 메서드 중에서 getName() 은 쿠키의 이름을 반환한다.

3. 쿠키 값 추출 : String getValue()

   - 쿠키의 값을 추출하는 메서드 

> *특정 쿠키의 값을 확인할 때는 Cookie 객체의 getName()과 getValue() 메서드를 이용한다*

<br>

Cookie 추출 코드 예시

```java
protected void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
    Cookie[] cookies = request.getCookies();
		if(cookies != null) {//쿠키의 존재 유무 확인
			boolean flag = false;
			for(int i = 0; i < cookies.length; i++) {
				if(cookies[i].getName().equals("time")) {
					out.println("client로부터 받아온 time cookie value" + cookies[i].getValue());
					flag = true;
					break;
				}
			}
			//시스템상 다른 쿠키가 존재할 수 있기 때문에 
			//time 쿠키의 존재 유무를 판단하기 위한 코드
			if(flag = false)
				out.println("time 쿠키가 존재하지 않습니다");
		}else {
			out.println("쿠키가 존재하지 않습니다");
		}
```

<br>

# 3. Session

-----

> **세션이란,** 
>
> - **클라이언트가 서버에 정보를 요청할 때 생성되는 "상태정보"를 말한다.**
> - **일정 시간 동안 같은 사용자(웹 브라우저 단위)로부터 들어온 요청을 하나의 상태로 보고 그 상태를 유지하는 기술을 말한다.**
> - 즉, 웹 브라우저를 통해 서버에 접속한 이후부터 브라우저를 종료할 때까지 유지되는 상태이며 웹 브라우저 단위로 같은 세션이라고 할 수 있다. 

###  3 - 1. Session 의 특성 

- 사용자 상태 정보를 클라이언트에 저장된다.
- 저장 용량 및 데이터 타입의 제한이 없다.
- 세션 유지 기간 
  1. 지정한 유효시간(tomcat의 경우 기본 30분 설정) 내에 새로운 요청이 없을 때 세션 만료(ex. 정부24 사이트의 로그인 만료 팝업창 - 제한 시간인 30분 전에 요청을 하면 다시 연장된다)
     - WAS 에 세션 유지 시간이 별도로 설정되어 있다. 유지 시간 변경시에는 web.xml 코드 중 아래의 `<session-config>`태그의 `<session-timeout>` 태그 내용을 변경한다.
  2. 브라우저 종료 시 까지
  3. 로그아웃 실행 시 까지

- SessionID 

  - HttpSession 인터페이스를 통해 객체가 생성될 때, 요청을 보내온 시간 및 클라이언트의 정보 등을 조합한 세션 ID가 부여된다. 
  - 이 세션 ID는 클라이언트 측에 쿠키 기술로 저장된다. 
    따라서 Session 객체는 서버에 생성되고, 클라이언트에는 세션 ID 가 쿠키로 저장되어 각 클라이언트에 대해 생성되는 Session 객체를 클라이언트마다 개별적으로 관리할 수 있다. 
- 세션은 내부적으로 Cookie를 사용한다.(ex.로그인)

###  3 - 2. HttpSession 관련 주요 메서드

1. HttpServletRequest (우선 수행)
  - 세션 트래킹 : HttpServletRequest 의 메서드를 사용하여 HttpSession 객체를 얻어낸 후 HttpSession 객체가 가지고 있는 메서드를 이용하여 클라이언트 단위의 정보를 유지하는 작업을 한다
  - <span class="highlight">getSession() </span>: 기존 세션이 존재하면 기존 세션을 반환, 없으면 새로운 HttpSession 객체를 생성한다.
  - getSession(boolean) : 기존 세션이 존재하면 기존 세션을 반환, 없으면 null 을 반환한다. 
2. HttpSession
  - setAttribute(String name, Object value) : 세션에 String 타입의 name 과 Object 타입의 value 값을 할당해서 저장한다. 
  - getAttribute(String name) : 세션에 저장된 attribute 정보를 name으로 검색해서 value 를 반환한다. 
  - getAttributeNames() : HttpSession 객체에 등록되어 있는 모든 정보의 이름들을 Enumeration 타입으로 반환한다.
  - isNew() : 서버에서 새로운 HttpSession 객체를 생성했다면 true를 반환하고, 기존 세션이 있다면 false를 반환한다.
  - invalidate() : 세션을 무효화 시킨다. 무효화 시킨다는 것은 세션ID(jsessionid)를 없애는 것이 아니라 연결만 끊는 것이다. 단지 사용할 수 없게 하는 것이다. 

###  3 - 3. Session 동작 방식 

- 로그인 시에 Session 동작 방식 예시 

  1. 클라이언트(웹브라우저)가 서버에 요청한다.(첫 요청에서는 SessionID가 존재하지 않는다.)
  2. getSession()을 통해 세션을 생성한다.
  3. session.setAttribute(String name, Object value) 로 세션 객체에 데이터를 할당한다.
  4. 서버에서 쿠키를 확인하고 SessionID 가 없다면 새로 발급하여 클라이언트에게 jsessionid cookie를 전달한다.
  5. 서버측에는 세션 생성지 jsessionid 에 mapping 된 세션 객체가 존재하는 상태이다.

  ----

- 로그인 후 재접속 시 동작 방식 흐름(클라이언트의 재요청)

  5. 클라이언트는 서버에서 전달받았던 SessionID를 요청시마다 헤더 쿠키에 넣어서 요청한다.
  6. 클라이언트가 처음 요청했을 때 getSession()을 통해 세션 객체를 이미 생성했으므로 
     getSession(false) 메서드를 이용하여 존재 유무를 판단한다.(물론 세션 객체가 없다면 세션 객체 생성된다. 인자값이 false 이기 때문에 세션 객체가 없을 때 새로 생성되는 것이 아니라 null 반환)
     - 이 때 요청한 클라이언트가 WAS가 발급한 jsessionid 쿠키 정보가 있는지 확인하여 연결된 세션 객체를 반환하고 없다면 null을 반환한다.

<br>

# Cookie 와 Session 의 차이점

---

|           |                      Cookie                       |                        Session                        |
| :-------: | :-----------------------------------------------: | :---------------------------------------------------: |
| 저장위치  |                    클라이언트                     |                         서버                          |
|   보안    |            보안 취약(클라이언트 저장)             |   SessionID만 저장하기 때문에 상대적을 보안성 높음    |
| 유지 기간 | 만료시간 기준(브라우저 종료해도 남아있을 수 있음) |   브라우저가 종료되면 만료시간 여부와 관계없이 종료   |
|   속도    |         저장위치로 인해 서버 요청 시 빠름         | 요청 시마다 서버에서 처리해야 하기 때문에 비교적 느림 |



<br>

## References

- [https://doooyeon.github.io/2018/09/10/cookie-and-session.html](https://doooyeon.github.io/2018/09/10/cookie-and-session.html)
- [https://cjh5414.github.io/cookie-and-session/](https://cjh5414.github.io/cookie-and-session/)









