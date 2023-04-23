# 쿠키
- 기본적으로 HTTP 프로토콜 환경은 한번 요청을 한 후 한번 응답을 하고 연결이 종료됩니다.
- 그래서 서버는 클라이언트가 누구인지 매번 확인을 해야 하는데 이를 보완하기 위해 쿠키와 세션을 사용합니다.
- 세션과 달리 상태 정보를 웹서버가 아닌 클라이언트에 저장합니다.
- 쿠키는 클라이언트의 정보를 웹 브라우저에 저장하므로 이후에 웹 서버로 전송되는 요청에는 쿠키가 가지고 있는 정보가 포함됩니다.
- 웹 브라우저가 접속했던 웹 사이트에 관한 정보와 개인 정보가 기록되기 때문에 보안에 문제가 있습니다.

## 쿠키의 동작과정

- **쿠키 생성단계** : 쿠키를 사용하려면 먼저 쿠키를 생성해야 합니다. 쿠키는 주로 웹 서버 측에서 생성합니다. 생성된 쿠키는 응답 데이터와 함께 저장되어 웹 브라우저에 전송됩니다.
- **쿠키 저장단계** : 웹브라우저는 응답 데이터에 포함된 쿠키를 쿠키 저장소에 보관합니다. 쿠키는 종류에 따라 메모리나 파일로 저장됩니다.
- **쿠키 전송단계** : 웹브라우저는 한 번 저장된 쿠키를 요청이 있을 때마다 웹 서버에 전송합니다. 웹 서버는 웹 브라우저가 전송한 쿠키를 사용하여 필요한 작업을 수행할 수 있습니다.

> 일단 웹 브라우저에 쿠키가 저장되면 웹 브라우저는 쿠키가 삭제되기 전까지 웹 서버에 쿠키를 전송합니다

![image](https://user-images.githubusercontent.com/54658614/233833480-66799c56-efb5-4535-9aa8-6bb85aa5f836.png)

### 쿠키의 구성요소
- 이름 : 각각의 쿠키를 구별하는 데 사용되는 이름
- 값 : 쿠키의 이름과 관련된 값
- 유효시간 : 쿠키의 유지시간
- 도메인 : 쿠키를 전송할 도메인
- 경로 : 쿠키를 전송할 요청 경로


#### Cookie 클래스의 메서드 종류

|메서드|반환 유형|설명|
|-----|----|----------|
|getComment()|String|쿠키에 대한 설명을 반환합니다.|
|getDomain()|String|쿠키에 대한 유효한 도메인 정보를 반환합니다.|
|getMaxAge()|int|쿠키의 사용 가능 기간에 대한 정보를 반환합니다.|
|getName()|String|쿠키의 이름을 반환합니다.|
|getPath()|String|쿠키의 유효한 디렉토리 정보를 반환합니다.|
|getSecure()|boolean|쿠키의 보안 설정을 반환합니다.|
|getValue()|String|쿠키에 설정된 값을 반환합니다.|
|getVersion()|int|쿠키의 버전을 반환합니다.|
|setComment(String)|void|쿠키에 대한 설명을 설정합니다.|
|setDomain(String)|void|쿠키에 유효한 도메인을 설정합니다.|
|setMaxAge(int)|void|쿠키의 유효기간을 설정합니다.|
|setPath(String)|void|쿠키가 적용되는 경로를 지정합니다.|
|setSecure(boolean)|void|쿠키의 보안을 설정합니다.|
|setValue(String)|void|쿠키의 값을 설정합니다.|
|setVersion(int)|void|쿠키의 버전을 설정합니다.|

#### 쿠키와 세션의 차이

|구분|쿠키|세션|
|----|------|-------|
|사용 클래스|Cookie 클래스|HttpSessin 인터페이스|
|저장 형식|텍스트 형식|Object 형|
|저장 장소|클라이언트|서버(세션 아이디만 클라이언트에 저장)|
|종료 시점|쿠키 저장 시 설정(설정하지 않을 경우 브라우저 종료 시 소멸)|정확한 시점을 알 수 없음|
|리소스|클라이언트의 리소스 사용|서버의 리소스 사용|
|보안|클라이언트에 저장되므로 사용자의 변경이 가능하여 보안에 취약|서버에 저장되어 있어 상대적으로 안정적|

## 쿠키 생성

- 쿠키를 사용하려면 먼저 Cookie 클래스를 사용하여 쿠키를 생성
- 쿠키를 생성하는 데이는 Cookie() 생성자를 사용합니다.
- 쿠키를 생성한 후에는 반드시 response 내장 객체의 addCookie() 메서드로 쿠키를 설정해야 합니다.

```java
Cookie Cookie(String name, String value)
```



## Ex_날짜_Cookie_Session 프로젝트 생성하기

## SetCookieAction 서블릿 생성하기
```
package action;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class SetCookieAction
 */
@WebServlet("/cookie.do")
public class SetCookieAction extends HttpServlet {
	private static final long serialVersionUID = 1L;

	/**
	 * @see HttpServlet#service(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		
		Cookie cookie = new Cookie("param1", "asdf");
		response.addCookie(cookie);
		
		response.sendRedirect("ex01_Cookie.jsp");
	}
}

```

### ex01_Cookie.jsp 생성하기
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
       
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	쿠키저장완료<br>
	쿠키이름 : ${cookie.param1.name}<br>
	쿠키내용 : ${cookie.param1.value}<br>
</body>
</html>
```
![image](https://user-images.githubusercontent.com/54658614/233833376-6ae14eb3-c78a-40b5-bacc-299c84df3fa6.png)



![image](https://user-images.githubusercontent.com/54658614/233546625-a0a67b82-7b25-4f61-9ff8-b8667a7f8a08.png)

응답헤더에 쿠키가 잘 저장이 되어있는걸 볼 수 있다.

쿠키는 개인의 컴퓨터에 저장이된다.

![image](https://user-images.githubusercontent.com/54658614/233546899-d581dc18-3677-40ad-9e07-91caef204449.png)

쿠키는 브라우저 자체에 저장이 된다.

쿠키는 브라우저에 저장이 되고 요청시마다 전송이 되기 때문에 보안에 취약하다는 문제가 있다.<br>
그래서 텍스트를 그대로 저장하지는 않고 암호화를 통해서 저장한다<br>

아래는 네이버 로그인 후 블로그에 접속했을 때 까지의 쿠키에 대한 정보이다.

![image](https://user-images.githubusercontent.com/54658614/233548583-18d97aa9-e171-4884-97f8-3a704ad4039c.png)

Domain : 현재 응답한 서버의 도메인 (도메인만 내 쿠키의 Value에 접근할 수 있다고 보면 된다.)<br>
Path : 쿠키의 경로<br>
Expires : 만료시간을 정할 수 있다.( ex)일주일동안 팝업창 띄우지 않기)<br>
HttpOnly : 원래 쿠키는 JS와 같은 프론트단에서도 접근할 수 있다. 그러면 해커의 공격에 더 취약해지기 때문에<br> &nbsp;&nbsp;&nbsp;&nbsp; HttpOnly를 체크하면 Servlet과 같은 소스에서만 접근할 수 있도록 해준다.

쿠키의 이름과 들어있는 내용들을 조회해보자.

### 쿠키 객체 얻기 
- 클라이언트에 저장된 모든 쿠키 객체를 가져오려면 request 내장 객체의 getCookies() 메서드를 사용합니다.
- 쿠키 객체가 여러 개 일때는 배열 형태로 가져옵니다.
```java
Cookie[] requst.getCookies()
```

#### getCookies() 메서드 사용 예
```java
Cookie[] cookies = request.getCookies();
```

### 쿠키 객체의 정보 얻기
```java
String getName() - 쿠키 이름
String getValue() - 쿠키 값 
```

## 쿠키 기한 정하기 SetCookieAction서블릿 코드 추가하기
```
package action;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class SetCookieAction
 */
@WebServlet("/cookie.do")
public class SetCookieAction extends HttpServlet {
	private static final long serialVersionUID = 1L;

	/**
	 * @see HttpServlet#service(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		
		Cookie cookie = new Cookie("param1", "asdf");
		cookie.setMaxAge(60*60*24*7);//일주일
		response.addCookie(cookie);
		

		response.sendRedirect("ex01_Cookie.jsp");
	}
}
```

![image](https://user-images.githubusercontent.com/54658614/233834027-ebc7a46e-bdd3-4271-8fd9-d80a3c0f8149.png)


## 쿠키 삭제
- Cookie 클래스는 쿠키를 삭제하는 기능을 별도로 제공하지 않는다.
- 쿠키를 더 유지할 필요가 없으면 쿠키의 유효기간을 만료하면 됩니다.
- setMaxAge() 메서드에 유효 기간을 0으로 설정하여 쿠키를 삭제할 수 있습니다.

```java
void setMaxAge(int age)
```

#### setMaxAge() 메서드 사용 예
```java
Cookie cookie = new Cookie("memberId", "admin");
cookie.setMaxAge(0);
response.addCookie(cookie);
```

# 세션
- 세션은 클라이언트와 웹 서버 간의 상태를 지속적으로 유지하는 방법2) 세션은 웹서버에서만 접근이 가능하므로 보안유지에 유리하며 데이터를 저장하는 데 한계가 적습니다.
- 세션은 오직 웹 서버에 존재하는 객체로 웹 브라우저마다 하나씩 존재하므로 웹 서버의 서비스를 제공받는 사용자를 구분하는 단위가 됩니다.
- 세션을 사용하면 클라이언트가 웹 서버의 세션에 의해 가상으로 연결된 상태가 되며, 웹브라우저를 닫기 전까지 웹 페이지를 이동하더라도 사용자의 정보가 웹 서버에 보관되어 있어 사용자 정보를 잃지 않습니다.
- JSP 페이지는 세션 기능을 사용할 수 있도록 session 내장 객체를 제공합니다.

## 개인의 정보를 어떻게 저장하는가?
- 쿠키값:JSESSIONID
- 브라우저별로 구별할 수 있는 유일한 값

#### session 내장객체 메서드 종류
- javax.servlet.http.HttpSession

|메서드|반환유형|설명|
|------|----|----------|
|getAttribute(String name)|Object|세션 속성 이름이 name인 속성 값을 Object형으로 반환합니다. 해당되는 속성 이름이 없을 때는 null을 반환합니다. 반환값이 Object 형이므로 반드시 형 변환을 하여 사용해야 합니다.|
|getAttributeNames()|Enumeration|세션 속성 이름을 Enumeration 객체 타입으로 반환|
|getCreationTime()|long|세션이 생성된 시간을 반환합니다. 1970년 1월 1일 0시 0초부터 현재 세션이 생성된 시간까지 경과한 시간을 1/1000초 값으로 반환|
|getId()|String|세션에 할당된 고유아이디를 String형으로 반환|
|getLastAccessedTime()|long|해당 세션에 클라이언트가 마지막으로 request를 보낸 시간을 반환합니다.|
|getMaxInactiveInterval(int interval)|int|해당 세션을 유지하기 위해 세션 유지 시간을 반환합니다. 기본 값은 1,800초(30분)입니다.|
|isNew()|boolean|해당 세션의 생성여부를 반환합니다. 처음 생성된 세션이면 true를 반환하고 이전에 생성된 세션이면 false를 반환합니다.|
|removeAttribute(String name)|void|세션 속성 이름이 name인 속성을 제거합니다.|
|setAttribute(String name, Object value)|void|세션 속성 이름이 name인 속성에 value를 할당합니다.|
|setMaxInactiveInterval(int interval)|void|해당 세션을 유지하기 위한 세션 유지 시간을 초 단위로 설정|
|invalidate()|void|현재 세션에 저장된 모든 세션 속성을 제거합니다.|

## 세션 생성
- 세션 속성값은 Object 객체 타입만 가능하기 때문에 int, double, char 등의 기본 타입은 사용할 수 없습니다.
- 만약 동일한 세션의 속성이름으로 세션을 설정하면 마지막에 설장한 것이 세션 속성 값이 됩니다.
```java
void setAttribute(String name, Object value)
```

## ex01_session.jsp 생성하기
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
    <%
    	session.setAttribute("param1", "asdf");
    	session.setAttribute("param2", "qwer");
    	session.setAttribute("param3", "zxcv");
    	
    	session.removeAttribute("param1");
    %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>

</body>
</html>
```

## ex02_session.jsp 생성하기
```
<%@page import="java.util.Enumeration"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
    
    <%
    	Enumeration<String> names = session.getAttributeNames();
    
    
    	while(names.hasMoreElements()){
    		String key = names.nextElement();
    		String value = (String)session.getAttribute(key);
    		out.println(key + " = " + value + "<br>");
    	}
    %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>

</body>
</html>
```

## ex03_session.jsp 생성
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
    
    <%
    	session.setMaxInactiveInterval(60*60*2);
    	//세션의 기본 지속시간은 30분이다.(1800초)
    	int second = session.getMaxInactiveInterval();
    	out.println(second);
    %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>

</body>
</html>
```

# DB에서 아이디 비밀번호 가져와서 로그인하기

## 테이블 생성하기
```

```

