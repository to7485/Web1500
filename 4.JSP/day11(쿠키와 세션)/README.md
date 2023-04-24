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

Ex_날짜_login 프로젝트 생성하기

## 테이블 생성하기
이미 있는 사람은 추가하지 않아도 된다.
```
--일련번호 관리객체
create sequence seq_member_idx;

--회원테이블
create table member(
	idx int primary key,               --일련번호
	name varchar2(100) not null,       --이름
	id varchar2(100) not null unique,  --아이디(중복방지 unique)
	pwd varchar2(100) not null,        --패스워드
	email varchar2(100) unique,        --이메일
);

--샘플 데이터 추가
insert into member values( seq_member_idx.nextVal,
				 '홍길동', 
				 'one',
				 '1234',
				 'one@korea.com'
				 );

--커밋
commit;
```

## check_login.jsp 생성하기
-로그인이 되어있는 상태인지를 검증하기 위한 jsp
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	나는 로그인을 확인하지!
</body>
</html>
```
## main_content.jsp 생성하기
- 로그인이 됐을때만 접근 가능한 페이지
```html
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
url에 main_content.jsp를 그냥 쓰면 로그인을 안해도 접근이 가능하다.
check_login.jsp를 거쳐서 나오게 해보자
외부에 있는걸 붙힌다. 

로그인이 필요한 페이지에서는 아래의 코드를 가지고 있으면 된다.
	<jsp:include page="check_login.jsp"/>
	 <!-- main_content페이지에 접근하려고 해도 check_login에 먼저 접근해야 한다. -->
	메인페이지
</body>
</html>
```

### include 액션 태그의 기능과 사용법
- include 액션 태그는 include 디렉티브 태그처럼 JSP 페이지의 특정 영역에 외부 파일의 내용을 포함하는 태그
- JSP 페이지에 포함할 수 있는 외부파일은 HTML, JSP, 서블릿 페이지 등입니다.
```
<jsp:include page=“파일명” flush=“false” />
```
- flush 속성 값은 설정한 외부 파일로 제어가 이동할 때 현재 JSP 페이지가 지금까지 출력 버퍼에 저장한 결과를 처리합니다. 기본 값은 false이고, true로 설정하면 외부 파일로 제어가 이동할 때 현재 JSP 페이지가 지금까지 출력 버퍼에 저장된 내용을 웹 브라우저에 출력하고 출력 버퍼를 비웁니다.
- include 액션 태그는 forward 액션 태그처럼 외부 파일을 포함한다는 점이 비슷하지만 포함된 외부 파일이 실행된 후 현재 JSP 페이지로 제어를 반환한다는 것이 가장 큰 차이점
- JSP 컨테이너는 현재 JSP 페이지에서 include 액션 태그를 만나면 include 액션 태그에 설정된 외부 파일의 실행 내용을 현재 JSP 페이지의 출력 버퍼에 추가 저장되어 출력

>flush 속성 값<br>일반적으로 flush 속성은 false로 지정하는 것이 좋습니다. true로 지정하면 일단 출력 버퍼를 웹 브라우저에 전송하는데 이때 헤더 정보도 같이 전송됩니다. 헤더 정보가 웹 브라우저에 전송되고 나면 헤더 정보를 추가해도 결과가 반영되지 않습니다.

#### include 액션 태그와 include 디렉티브 태그의 차이
|구분|include 액션 태그|include 디렉티브 태그|
|----|-------|-------|
|처리시간|요청 시 자원을 포함합니다.|번역 시 자원을 포함합니다.|
|기능|별도의 파일로 요청 처리 흐름을 이동합니다.|현재 페이지에 삽입합니다.|
|데이터 전달방법|request 기본 내장 객체나 param 액션 태그를 이용하여 파라미터를 전달합니다.|페이지 내의 변수를 선언한 후 변수에 값을 저장합니다.|
|용도|화면 레이아웃의 일부분을 모듈화할 때 주로 사용합니다.|다수의 JSP 웹 페이지에서 공통으로 사용되는 코드나 저작권과 같은 문장을 포함하는 경우에 사용합니다.|
|기타|동적 페이지에 사용합니다.|정적 페이지에 사용합니다.|


## login_form.jsp 생성하기
- 아이디가 틀렸으면 아이디가 없다고, 비밀번호가 틀렸으면 비밀번호가 틀렸다고 유효성체크 나눠서 해주기
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
ajax사용하기 위해 등록해주기
로그인시도할 때 아이디가 없으면 아이디가 없다고 경고, 비밀번호가 틀리면 비밀번호가 틀렸다고 경고를 따로 띄워주자.
'아이디나 비밀번호가 틀렸습니다' 라고 나오면 개발자가 귀찮아서 코드를 추가하지 않은것
	<script src="js/httpRequest.js"></script>
	<script>
		function send(f){
			var id = f.id.value.trim();
			var pwd = f.pwd.value.trim();
			
			//유효성체크
			if(id==''){
				alert("아이디를 입력해주세요");
				return;
			}
			
			if(pwd==''){
				alert("비밀번호를 입력하세요");
				return;
			}
			
			var url = "login.do";
			var param = 													"id="+encodeURIComponent(id)+"&pwd="+encodeURIComponent(pwd);
			
			sendRequest(url,param,myChek,"POST");
		}
		
		//콜백메서드
		function myCheck(){
			if(xhr.readyState == 4 && xhr.status == 200){
				
			}
		}
	</script>
</head>
<body>
	<form>
		<table border="1" align="center">
			<caption>:::로그인:::</caption>
			<tr>
				<th>아이디</th>
				<td><input name="id"></td>
			</tr>
			<tr>
				<th>비밀번호</th>
				<td><input name="pwd" type="password"></td>
			</tr>
			<tr>
			
				<td colspan="2" align="center">
					<input  type="button" value="로그인" onclick="send(this.form)">
				</td>
			</tr>
			
		</table>
	</form>

</body>
</html>
```

## MemberVO 클래스 생성
```java
package vo;

public class MemberVO {
	private String name,id,pwd;

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public String getId() {
		return id;
	}

	public void setId(String id) {
		this.id = id;
	}

	public String getPwd() {
		return pwd;
	}

	public void setPwd(String pwd) {
		this.pwd = pwd;
	}	
}
```

## LoginAction 생성
```
package action;

import java.io.IOException;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import dao.MemberDAO;
import vo.MemberVO;

/**
 * Servlet implementation class LoginAction
 */
@WebServlet("/login.do")
public class LoginAction extends HttpServlet {
	private static final long serialVersionUID = 1L;

	/**
	 * @see HttpServlet#service(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// login.do?id=aaaa&pwd=1111
		
		String id = request.getParameter("id");
		String pwd = request.getParameter("pwd");
		
		MemberVO vo = MemberDAO.getInstance().selectOne(id); 
	}

}
```
## MemberDAO 클래스 생성
```
package dao;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

import service.DBService;
import vo.MemberVO;

public class MemberDAO {
	// single-ton pattern: 
	// 객체1개만생성해서 지속적으로 서비스하자
	static MemberDAO single = null;

	public static MemberDAO getInstance() {
		//생성되지 않았으면 생성
		if (single == null)
			single = new MemberDAO();
		//생성된 객체정보를 반환
		return single;
	}
	
	//로그인 체크
실수를 많이 하는부분 id랑 pwd를 둘다 받았다고 가정해보자 쿼리문이
select * from member where id=? and pwd=? 이렇게 만드는 순간
id가 없어도 null이 들어오고 비밀번호가 틀려도 null이 들어온다.
아이디나 비밀번호가 잘못되었습니다 라고 나옴;;
	public MemberVO selectOne(String id) {

		MemberVO vo = null;

		Connection conn = null;
		PreparedStatement pstmt = null;
		ResultSet rs = null;
		String sql = "select * from member where id=?";

		try {
			//1.Connection얻어온다
			conn = DBService.getInstance().getConnection();
			//2.명령처리객체정보를 얻어오기
			pstmt = conn.prepareStatement(sql);

			//3.pstmt parameter 설정
			pstmt.setString(1, id);
			//4.결과행 처리객체 얻어오기
			rs = pstmt.executeQuery();

			if (rs.next()) {
				vo = new MemberVO();
				//현재레코드값=>Vo저장
				vo.setPwd(rs.getString("pwd"));
				vo.setName(rs.getString("name"));
			}

		} catch (Exception e) {
			// TODO: handle exception
			e.printStackTrace();
		} finally {

			try {
				if (rs != null)
					rs.close();
				if (pstmt != null)
					pstmt.close();
				if (conn != null)
					conn.close();
			} catch (SQLException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
		}

		return vo;
	}
}
```
## LoginAction 에 코드 추가하기

```
.....
MemberVO vo = MemberDAO.getInstance().selectOne(id); 

String param = " ";		
String resultStr=" ";

//vo가 null인경우 id자체가 DB에 존재하지 않는다는 의미
if(vo==null) {
	param = "no_id";
	resultStr = String.format("[{'param':'%s'}]", param);
	response.getWriter().print(resultStr);
	return;
} 

if(!vo.getPwd().equals(pwd)) {
	//비밀번호가 일치하지 않을 때
	param = "no_pwd";
	resultStr = String.format("[{'param':'%s'}]", param);
	response.getWriter().print(resultStr);
	return;
}

//아이디와 비빌번호 체크에 문제가 없다면 세션에 바인딩 한다.
//세션은 서버의 메모리(램)를 사용하기 때문에 세션을 많이 사용할수록 브라우저가 느려지기 때문에
//필요한 곳에서만 세션을 쓰도록 하자

//세션유지시간(기본값 30분)
		HttpSession session = request.getSession();
		//session.setMaxInactiveInterval(3600); //세션이 1시간 유지 초단위로 써줘야함;;
		session.setAttribute("vo", vo); 

//포워딩을 할 필요가 없고 어느 jsp에서든 el표기법으로 vo.name,vo.pwd를 사용할 수 있다.

로그인 성공한 경우
param="clear";
resultStr = String.format("[{'param':'%s'}]", param);
response.getWriter().print(resultStr);


여기서 할 일을 마쳤으니 콜백메서드로 돌아가자
```
## login_form.jsp 콜백메서드에 코드 작성하기
```
function myCheck(){
			if(xhr.readyState == 4 && xhr.status == 200){
				var data = xhr.responseText;
				var json = eval(data);
				
				if(json[0].param == 'no_id'){
					alert("아이디가 존재하지 않습니다.");
				}else if(json[0].param == 'no_pwd'){
					alert("비밀번호가 맞지 않습니다.");
				}else {
					alert("로그인 성공");
					location.href="main_content.jsp";
로그인에 성공하면 메인 화면으로 이동할건데 로그인 하지 않아도 위의 주소를 치면 이동이 가능하다는 치명적인 단점이 있다. 이걸 잡아줘야 한다.
				}
			}
		}
```

## check_login.jsp 수정하기
```
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<c:if test="${empty vo }"> <!-- vo가 비어있다는 것은 세션에 저장을 못했다는것! -->
	<script>
		alert("로그인 후 이용해주세요");
		location.href="login_form.jsp";
	</script>
</c:if>
```
## 로그아웃 기능 만들기
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<jsp:include page="check_login.jsp"/>
	 <!-- main_content페이지에 접근하려고 해도 check_login에 먼저 접근해야 한다. -->
	메인페이지<br>
	${vo.name}님 로그인을 환영합니다.
	<input type="button" onclick="logout.do">
</body>
</html>
```
## LogoutAction 생성하기
```
package action;

import java.io.IOException;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

/**
 * Servlet implementation class LogoutAction
 */
@WebServlet("/logout.do")
public class LogoutAction extends HttpServlet {
	private static final long serialVersionUID = 1L;

	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		HttpSession session = request.getSession(); 
		세션객체는 static이라서 주소를 다 공유한다. 어차피 객체를 여러개 만들어도 하나만 생성되어 있다.
		//session.removeAttribute("vo");
		//세션에 들어있는 모든 속성을 제거한다.
		session.invalidate();
		
		response.sendRedirect("login_form.jsp");
	}
}
```
## <jsp:include> 액션태그 응용하기
- 학원 홈페이지만 둘러봐도 상단과 하단은 계속 같은 내용을 보여주는것을 알 수 있다.
- 로그인 기능을 구현하면서 <jsp:include>를 사용했다.<br> 코드는 항상위에서 아래로 실행이 되면서 <jsp:include>의 page속성에 정의된 페이지가 먼저 실행되는 모습을 보았다.
- 이를 이용하여 페이지를 모듈화하여 상단과 하단의 코드를 jsp파일을 따로 만들어 한번만 작성하여 코드를 효과적으로 줄일 수 있다.

![image](https://user-images.githubusercontent.com/54658614/233902036-fe75e585-e1b1-4a05-ae08-08600586081a.png)

\<jsp:include\> 액션태그와 <%@ include %> 처리과정 차이

![image](https://user-images.githubusercontent.com/54658614/233902229-9e748287-c116-4ce7-8334-20b33952d363.png)




### layout.jsp 생성하기
```
<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
<html>
<head>
    <title>layout</title>
</head>
<body>
    
    <table width="400" border="1" cellpadding="0" cellspacing="0">
        <tr>
            <td>
                <jsp:include page="top.jsp" flush="false"/>
            </td>
        </tr>
        <tr>
            <td>
                <jsp:include page="content.jsp" flush="false"/>
            </td>
        </tr>
        <tr>
            <td>
                <jsp:include page="bottom.jsp" flush="false"/>
            </td>
        </tr>
    </table>
    
</body>
</html>
```

### top.jsp 생성하기
```
<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
<html>
<head>
    <title>top</title>
</head>
<body>
    상단메뉴 : 
    <a href="#">HOME</a> 
    <a href="#">INFO</a>
</body>
</html>
```

### bottom.jsp 생성하기
```
<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
<html>
<head>
    <title>bottom</title>
</head>
<body>
    하단메뉴 : 
    <a href="#">도움말</a> 
    <a href="#">약관</a>
    <a href="#">사이트맵</a>
</body>
</html>
```

### content.jsp
```
<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
<html>
<head>
    <title>content</title>
</head>
<body>
    <br><br>
    내용 페이지
    <br><br>
</body>
</html>
```



