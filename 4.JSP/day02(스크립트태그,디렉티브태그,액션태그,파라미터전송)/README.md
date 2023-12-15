# JSP
- JavaServerPages의 약자로 HTML코드에서 JAVA코드를 사용하여 동적인 웹페이지(Dynamic Web Page)를 생성하는 라이브러리이다.
- JSP가 하나의 Java클래스이기 때문에 모든 Java라이브러리를 끌어다 쓸 수 있다.

## JSP와 Servlet의 차이점
||장점|단점|형식|
|---|---|----|----|
|Servlet|복잡한 로직 구현에 적합함|Java코드 중심의 화면 작성 작업과 수정이 어려움| Java코드(주) + HTML코드|
|JSP|HTML코드 중심 -> 화면작성 작업, 수정용이|소스보기로 소스가 공개되어 중요정보에 부적합|HTML코드(주) + Java코드|

## JSP에서 Java코드를 넣는 방법

## 스크립트 태그의 종류

|스크립트 태그|형식|설명|
|----|---|----------|
|스크립트릿(scriptlet)|<% .... %>|자바 로직 코드를 작성하는데 사용합니다.|
|선언문(declaration)|<%! ... %>|자바 변수나 메서드를 정의하는 데 사용합니다.|
|표현문(expression)|<%= ... %>|변수, 계산식, 메서드 호출 결과를 문자열 형태로 출력하는 데 사용합니다.|

### 스크립트릿(scriptlet)
- 문법(각 행이 세미콜론으로 끝나야 한다.)
```jsp
<% 자바 코드; %>
```
- 스크립트릿 태그에 작성된 태그에 작성된 자바코드는 서블릿 프로그램으로 변환될 때 _jspService()메서드 내부에 복사된다.
- _jspService()메서드 내부에 복사되므로 지역변수가 되어 이 태그에 선언된 변수는 스크립트릿 태그 내에서만 사용할 수 있다.

### scriptlet01.jsp
```jsp
<html>
<head>
<title>Scripting Tag</title>
</head>
<body>
	<% 
		int a = 2;
		int b = 3;
		int sum = a + b;
		out.println("2 + 3 = " + sum);
	%>
</body>
</html>
```

### scriptlet02.jsp
```jsp
<html>
<head>
<title>Scripting tag</title>
</head>
<body>
	<%
		for (int i = 0; i <= 10; i++) {
			if (i % 2 == 0)
				out.println(i + "<br>");
		}
	%>
</body>
</html>
```
### 선언문(declaration)
- 선언문(declaration)태그는 변수나 메서드 등을 선언하는 태그
- 선언문 태그에 선언된 변수와 메서드는 서블릿 프로그램으로 번역될 때 _jspService() 메서드 외부에 배치되므로 JSP 페이지의 임의의 위치에서 선언할 수 있습니다.
- 스크립트릿 태그보다 나중에 선언해도 스크립트릿 태그에서 사용할 수 있습니다.
- 선언문 태그로 선언된 변수는 서블릿 프로그램으로 번역될 때 클래스 수준의 멤버 변수가 되므로 전역 변수로 사용됩니다.
- 선언문 태그로 선언된 메서드는 전역변수처럼 전역 메서드로 사용됩니다.


|선언문태그|스크립트릿태그|
|---------|-------------|
|변수뿐만아니라 메서드를 선언할 수 있다.|스크립트릿 태그는 메서드 없이 변수만을 선언할 수 있다.|
|서블릿 프로그램으로 변환될 때 _jspService() 메서드 외부에 배치된다|서블릿 프로그램으로 변환될 때 _jspService()메서드 내부에 배치된다.|

### declaration01.jsp
```jsp
<body>
	<%!int data = 50;
	int sum(int a, int b) {
		return a + b;
	}
	
	%>
	<%
		out.println("Value of the variable is:" + data + "<br>");
		out.println("2 + 3 = " + sum(2, 3));
	%>
</body>
```

### 표현문(expression)
- <%= 와 %>를 사용하여 웹브라우저에 출력할 부분을 표현합니다.
- 표현문 태그는 스크립트릿 태그에서 사용할 수 없으므로 이 경우에는 out.print()메서드를 사용해야 한다.

```jsp
<%=자바 코드%>     각 행을 세미콜론으로 종료할 수 없음
```

### expression01.jsp
```jsp
<html>
<head>
<title>Scripting Tag</title>
</head>
<body>
	<p> Today's date: <%=new java.util.Date()%></p>
</body>
</html>
```

### expression02.jsp
```jsp
<html>
<head>
<title>Scripting Tag</title>
</head>
<body>
	<%
		int a = 10;
		int b = 20;
		int c = 30;
	%>
	<%=a + b + c%>
</body>
</html>
```
## 디랙티브 태그

### 디렉티브 태그의 종류
|디렉티브 태그| 형식|설명|
|----|----|---------|
|page|<%@ page ...%>|JSP 페이지에 대한 정보를 설정합니다.|
|include|<%@ include ... %>|JSP 페이지의 특정 영역에 다른 문서를 포함합니다.|
|taglib|<%@ taglib ... %>|JSP 페이지에서 사용할 태그 라이브러리를 설정합니다.|

### page 디렉티브 태그의 기능과 사용법
- JSP 페이지에 대한 정보를 설정하는 태그
- JSP 페이지가 생성할 콘텐츠 유형의 문서, 사용할 자바 클래스, 오류 페이지 설정, 세션 사용 여부, 출력 버퍼의 존재 유무 등과 같이 JSP 컨테이너가 JSP 페이지를 실행하는 데 필요한 정보를 설정할 수 있습니다.
- JSP 페이지의 어디에서든 선언할 수 있지만 일반적으로 JSP 페이지의 최상단에 선언하는 것을 권장
- 하나의 page 디렉티브 태그에 하나 또는 여러 개의 속성을 설정할 수 있습니다. 또는 여러 개의 속성마다 개별적으로 page 디렉티브 태그를 선언할 수 있습니다.
- import 속성을 제외한 속성은 JSP 페이지에 한 번씩만 설정할 수 있습니다.

```jsp
<%@ page 속성1="값" [속성2="값2" .. ] %>    
<%과 @사이에 공백이 없어야 함
```

#### page 디렉티브 태그의 속성
|속성|설명|기본값|
|----|-------|----|
|language|현재 JSP 페이지가 사용할 프로그래밍 언어를 설정합니다.<br><%@ page language="java" %>|java|
|**contentType**|현재 JSP 페이지가 생성할 문서의 콘텐츠 유형을 설정합니다.<br>(text/html, text/xml , text/plain)<br><%@ page contentType="text/html%><br>constentType은 문자열 세트를 설정 할 수 있음<br><%@ page contentType="text/html; charset=utf-8" %>|text/html|
|pageEncoding|현재 JSP 페이지의 문자 인코딩을 설정합니다.<br><%@ page pageEncoding="ISO-8859-1" %><br>contentType 속성의 문자열 세트로도 설정 할 수 있음<br><%@ page contentType="text/html; charset=ISO-8859-1" %>|ISO-8859-1|
|**import**|현재 JSP 페이지가 사용할 자바 클래스를 설정합니다.<br><%@ page import="java.io.*" %><br><%@ page import="java.io.*, java.lang.*" %><br><%@ page import="java.io.*" %><br><%@ page import="java.lang.*" %>||
|session|현재 JSP 페이지의 세션 사용여부를 설정합니다.<br><%@ page session="true" %>|true|
|buffer|현재 JSP 페이지의 출력 버퍼의 크기를 설정합니다.<br>속성값을 none으로 설정하면 출력 버퍼를 채우지 않고 웹 브라우저로 직접 전송<br> <%@ page buffer="none" %><br>출력 버퍼 크기를 32KB로 설정 : <% page buffer="32KB" %>|8KB|
|autoFlush|자동으로 출력 버퍼를 비우는 것을 제어<br><%@ page autoFlush="true" %>|true|
|isThreadSafe|현재 JSP 페이지의 멀티스레드 허용 여부를 설정합니다.<br>true – JSP 페이지에 대해 수신된 여러 요청이 동시에 처리<br>false – JSP 페이지에 대한 요청이 순차적으로 처리<br><%@ page isThreadSafe="true" %>|true|
|info|현재 JSP 페이지에 대한 설명을 설정합니다.(서블릿 인터페이스 getServletInfo() 메서드 사용)<br><%@ page info="Home Page JSP" %>||
|**errorPage**|현재 JSP 페이지에 오류가 발생했을 때 보여줄 오류페이지를 설정합니다.<br><%@ page errorPage="MyErrorPage.jsp" %>||
|isErrorPage|현재 JSP 페이지가 오류 페이지인지 여부를 설정합니다.<br>true로 설정하면 내장 객체인 exception 변수를 사용할 수 있습니다.<br><%@ page isErrorPage="true" %>|false|
|isELIgnored|현재 JSP 페이지의 표현언어(EL) 지원 여부를 설정합니다.<br><%@ isELIgnored="true" %>|false|
|isScriptingEnabled|현재 JSP 페이지의 스크립트 태그(선언문, 스크립틀릿, 표현문) 사용 여부를 설정합니다.<br><%@ page isScriptingEnabled="false" %>||

### include 디렉티브 태그의 기능과 사용법
- JSP 페이지의 특정 영역에 외부 파일의 내용을 포함하는 태그
- 포함할 수 있는 외부 파일은 HTML, JSP, 텍스트 파일 등이 있습니다.
- include 디렉티브 태그는 JSP 페이지 어디에서든 선언할 수 있습니다.
- include 디렉티브 태그는 서블릿 프로그램으로 번역될 때 현재 JSP 페이지와 설정된 다른 외부 파일의 내용이 병합되어 번역됩니다.

```jsp
<% include file="파일명" %>
```

### include01.jsp
```jsp
<%@ page contentType="text/html; charset=utf-8"%>
<html>
<head>
<title>Directives Tag</title>
</head>
<body>
	<%@ include file="include01_header.jsp"%>
	<p>방문해 주셔서 감사합니다.</p>
	<%@ include file="include01_footer.jsp"%>
</body>
</html>
```

### include01_header.jsp
```jsp
<%@ page contentType="text/html; charset=utf-8"%>
<%!
	int pageCount = 0;
	void addCount() {
		pageCount++;
	}
%>
<%
	addCount();
%>
<p>	이 사이트 방문은	<%=pageCount%>번째 입니다.</p>

```

### include01_footer.jsp
```jsp
Copyright MySite.com
```


### taglib 디렉티브 태그의 기능과 사용법
taglib 디렉티브 태그는 현재 JSP 페이지에 표현언어, JSTL, 사용자 정의 태그(custom tag)등 태그 라이브러리를 설정하는 태그이다.

```jsp
<% taglib prefix="태그 식별자" uri="경로" %>
```

- uri 속성은 사용자가 정의한 태그의 설정 정보를 가진 경로 주소
- prefix 속성은 uri에 설정한, 사용자가 정의한 태그를 식별하기 위한 고유 이름이다.
- 해당 JSP 페이지 내에서 uri 속성값을 그대로 사용하면 복잡하므로 prefix 속성 값이 대신 식별할 수 있게 해주는 것 
- uri 속성 값은 JSP 컨테이너에 사용자가 정의한 태그 라이브러리의 위치를 알려준다.

>JSTL 태그<br>일반적으로 웹 애플리케이션에서 쉽게 접할 수 있는 것은 JSTL 태그 라이브러리이다. 유용한 JSP 태그의 모음인 JSTL은 자주 사용되는 핵심 기능을 제공한다. 반복문, 조건문과 같은 논리적 구조 작업. XML 문서 조작, 국제화 태그 조작, SQL 조작 수행을 위한 태그 등을 지원한다.<br><br>STL을 사용하려면 WebContent/WEB-INF/lib/ 태그 디렉터리의 위치에 jstl.jar 라이브러리 파일이 있어야 합니다. 이 파일은 Apache Standard Taglib 페이지에서 다운로드할 수 있다.

## 액션태그
- 액션 태그는 서버나 클라이언트에게 어떤 행동을 하도록 명령하는 태그로 스크립트 태그, 주석, 디렉티브 태그와 함게 JSP 페이지를 구성하는 태그입니다.
- 액션 태그는 JSP 페이지에서 페이지와 페이지 사이를 제어하거나(forward), 다른 페이지의 실행 결과 내용을 현재 페이지에 포함하거나(include), 자바빈즈(javaBeans)등의 다양한 기능을 제공
- XML 형식 <jsp:... />를 사용합니다. 액션 태그는 반드시 끝나는 태그 />로 마무리해야 합니다.

### 액션태그의 종류
|액션태그|형식|설명|
|----|-------|---------|
|forward|<jsp:forward .. />|다른 페이지로의 이동과 같은 페이지 흐름을 제어합니다.|
|include|<jsp:include ... />|외부 페이지의 내용을 포함하거나 페이지를 모듈화합니다.|
|useBean|<jsp:useBean ... />|JSP 페이지에 자바빈즈를 설정합니다.|
|setProperty|<jsp:setProperty ... />|자바빈즈의 프로퍼티 값을 설정합니다.|
|getProperty|<jsp:getProperty ... />|자바빈즈의 프로퍼티 값을 얻어옵니다.|
|param|<jsp:param ... />|<jsp:forward>, <jsp:include>, <jsp:plugin>태그에 인자를 추가|
|plugin|<jsp:plugin ... />|웹브라우저에 자바 애플릿을 실행합니다. 자바 플러그인에 대한 OBJECT 또는 EMBED 태그를 만드는 브라우저별 코드를 생성|
|element|<jsp:element ... />|동적 XML 요소를 설정합니다.|
|attribute|<jsp:attribute ... />|동적으로 정의된 XML 요소의 몸체를 설정합니다.|
|body|<jsp:body ,,, />|동적으로 정의된 XML 요소의 동체를 설정합니다.|
|text|<jsp:text ... />|JSP 페이지 및 문서에서 템플릿 텍스트를 작성합니다.|

### forward 액션 태그의 기능과 사용법
- 현재 JSP 페이지에서 다른 페이지로 이동하는 태그
- JSP 컨테이너는 JSP 페이지에서 forward 액션 태그를 만나면 그 전까지 출력 버퍼에 저장되어 있던 내용을 모두 삭제하고 forward 액션 태그에 설정된 페이지로 프로그램 제어가 이동합니다.
```
<jsp:forward page="파일명" />    반드시 끝나는 태그가 있어야 함
또는 
<jsp:forward page="파일명"> </jsp:forward>
```
> forward 액션 태그 사용 시 주의점<br>웹 서버는 forward 액션 태그를 수행할 때 출력 버퍼를 지우므로 현재 페이지에서 forward 액션 태그가 선언된 지점 이전까지 HTML 코드가 손실됩니다. 그러나 현재 페이지가 이미 전달 버퍼로 채워진 경우에는 전달이 중단될 때까지 해당 내용을 웹서버에 응답으로 보냅니다. 이렇게 하면 잘못된 페이지가 클라이언트로 전송될 수 있습니다. 따라서 큰 출력을 생성하는 페이지에서 forward 액션 태그를 호출할 때는 신중을 기해야 합니다.

### forward
```jsp
<%@ page contentType="text/html; charset=utf-8"%>
<html>
<head>
<title>Action Tag</title>
</head>
<body>
	<h2>forward 액션 태그</h2>
	<jsp:forward page="forward_date.jsp" />
	<p>-------------------------------</p>
</body>
</html>
```

### forward_date.jsp 
```jsp
<%@ page contentType="text/html; charset=utf-8"%>
<html>
<head>
<title>Action Tag</title>
</head>
<body>
	<p>오늘의 날짜 및 시각
	<p><%=(new java.util.Date()).toLocaleString()%>
</body>
</html>
```

### include 액션 태그의 기능과 사용법
- include 액션 태그는 include 디렉티브 태그처럼 JSP 페이지의 특정 영역에 외부 파일의 내용을 포함하는 태그
- JSP 페이지에 포함할 수 있는 외부파일은 HTML, JSP, 서블릿 페이지 등입니다.
```
<jsp:include page="파일명" flush="false" />
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

### include.jsp
```jsp
<%@ page contentType="text/html; charset=utf-8"%>
<html>
<head>
<title>Action Tag</title>
</head>
<body>
	<h2>include 액션 태그</h2>
	<jsp:include page="include_date.jsp" flush="true" />
	<p>-------------------------------</p>
</body>
</html>
```

### include_date.jsp
```jsp
<%@ page contentType="text/html; charset=utf-8"%>
<html>
<head>
<title>Action Tag</title>
</head>
<body>
	<p>오늘의 날짜 및 시각</p>
	<p><%=(new java.util.Date()).toLocaleString()%></p>
</body>
</html>
```

### JSP 내장객체
- 내장객체란 스크립트릿(<% %>)안에서만 사용할 수 있는 객체로, 개발자가 별도로 생성하지 않아도 JSP에서 바로 생성할 수 있는 객체이다.
- 컨테이너(톰캣)가 JSP를 Servlet으로 변환할 때 자동으로 객체가 생성된다.
- JSP페이지에 별도로 import문 없이 자유롭게 사용할 수 있다.
- service() 메서드의 local variable로 선언

|기본객체 | 타입|설명|
|-----|-----|----|
|<b>request</b>|javax.servlet.http.HttpServletRequest|요청 정보가 담겨있는 객체|
|<b>response</b>|javax.servlet.http.HttpServletResponse|요청에 응답을 작성할 때 사용|
|<b>session</b>|javax.servlet.HttpSession|HTTP session을 구현한 객체. 세션 정보 저장에 사용|
|<b>out</b>|javax.servlet.jsp.JspWriter|응답에 포함될 내용을 출력할 때 사용|
|application|javax.servlet.ServletContext|Web Application 전체에서 공유하는 객체|
|config|javax.servlet.ServletConfig|JSP 페이지에 대한 설정 정보가 담긴 객체|
|page|java.lang.Object|JSP페이지 객체 자신|
|pageContext|javax.servlet.jsp.PageContext|JSP페이지 constext정보를 제공|
|exception|java.lang.Throwable|예외가 발생했을 때 생성되는 예외 객체|

### request 내장 객체의 기능과 사용법
- JSP 페이지에서 가장 많이 사용되는 기본 내장 객체
- 웹 브라우저에서 서버의 JSP페이지로 전달하는 정보를 저장
- JSP컨테이너는 웹브라우저에서 서버로 전달되는 정보를 처리하기 위해 javax.servlet.http.HttpServletRequest 객체 타입의 request 내장객체를 사용

### 요청 파라미터 관련 메서드 종류 

|요청파라미터 관련 메서드|반환유형|설명|
|-------|-----|--------|
|getParameter(String name)|String|요청 파라미터 이름이 name인 값을 전달받습니다. 요청 파라미터 값이 없으면 null 값을 반환합니다.|
|getParameterValues(String name)|String[]|모든 요청 파라미터 이름이 name인 값을 배열 형태로 전달받습니다. 요청 파라미터 값이 없으면 null 값을 반환합니다.|
|getParameterNames()|java.util.Enumeration|모든 요청 파라미터의 이름과 값을 Enumeration 객체 타입으로 전달받습니다.|
|getParameterMap()|java.util.Map|모든 요청 파라미터 이름과 값을 Map 객체 타입으로 전달받습니다.|

### request01.jsp
```jsp
<%@ page contentType="text/html; charset=utf-8"%>
<html>
<head>
<title>Implicit Objects</title>
</head>
<body>
	<form action="request01_process.jsp" method="post">
		<p>	아 이 디 : <input type="text" name="id">
		<p>	비밀번호 : <input type="text" name="passwd">
		<p>	<input type="submit" value="전송" />
	</form>
</body>
</html>
```
### request01_process.jsp
```jsp
<%@ page contentType="text/html; charset=utf-8"%>
<html>
<head>
<title>Implicit Objects</title>
</head>
<body>
	<%
		request.setCharacterEncoding("utf-8");
		String userid = request.getParameter("id");
		String password = request.getParameter("passwd");
	%>
	<p>	아이디 : <%=userid%>
	<p>	비밀번호 : <%=password%>
</body>
</html>
```

#### 요청 HTTP 헤더 관련 메서드
|요청HTTP헤더 관련 메서드|반환유형|설명|
|-------|-----|--------|
|getHeader(String name)|String|설정한 name의 헤더 값을 가져옵니다.|
|getHeaders(String name)|Enumeration|설정한 name의 헤더 목록 값을 가져옵니다.|
|getHeaderNames()|Enumeration|모든 헤더 이름을 가져옵니다.|
|getIntHeader(String name)|int|설정한 name의 헤더 값을 정수로 가져옵니다.|
|getDateHeader(String name)|long|설정한 name의 헤더 값을 시간 값으로 가져옵니다.|
|getCookies()|javax.servlet.http.Cookie|모든 쿠키 값을 가져옵니다.|

### request02.jsp
```jsp
<%@ page import="java.util.Enumeration" %>
<html>
<body>
<%
	Enumeration<String> en = request.getHeaderNames();
	while(en.hasMoreElements()) {
		String headerName = en.nextElement();
		String headerValue = request.getHeader(headerName);
		out.println(headerName + " : " + headerValue + "<br>");
	}
%>
</body>
</html>
```

#### 웹브라우저/서버 관련 메서드
|웹브라우저/서버 관련 메서드|반환유형|설명|
|-------|-----|--------|
|getRemoteAddr()|String|웹브라우저의 IP 주소를 가져옵니다.|
|getContentLength()|long|웹브라우저의 요청 파라미터 길이를 가져옵니다.|
|getCharacterEncoding()|String|웹브라우저의 문자 인코딩을 가져옵니다.|
|getContentType()|String|웹브라우저의 콘텐츠 유형을 가져옵니다.|
|getProtocol()|String|웹브라우저의 요청 프로토콜을 가져옵니다.|
|getMethod()|String|웹브라우저의 HTTP요청 메서드(GET, POST)를 가져옵니다.|
|getRequestURI()|String|웹브라우저가 요청한 URI 경로를 가져옵니다.|
|getContextPath()|String|현재 JSP 페이지의 웹 애플리케이션 콘텍스트 경로를 가져옵니다.|
|getServerName()|String|서버 이름을 가져옵니다.|
|getServerPort()|int|실행 중인 서버 포트 번호를 가져옵니다.|
|getQueryString()|String|웹브라우저의 전체 요청 파라미터 문자열[물음표(?) 다음 URL에 할당된 문자열]을 가져옵니다.|

### request03.jsp
```jsp
<%@ page contentType="text/html; charset=utf-8" %>
<html>
<body>
	클라이언트 IP : <%=request.getRemoteAddr()%><br>
	요청 정보 길이 : <%=request.getContentLength()%><br>
	요청 정보 인코딩 : <%=request.getCharacterEncoding() %><br>
	요청 정보 콘텐츠 유형 : <%=request.getContentType() %><br>
	요청 정보 프로토콜 : <%=request.getProtocol() %><br>
	요청 정보 전송방식 : <%=request.getMethod() %><br>
	요청 URI : <%=request.getRequestURI() %><br>
	콘텍스트 경로 : <%=request.getContextPath() %><br>
	서버 이름 : <%=request.getServerName() %><br>
	서버 포트 : <%=request.getServerPort() %><br>
        쿼리문 : <%=request.getQueryString() %>
</body>
</html>
```

### response 내장 객체의 기능과 사용법 
- response 내장 객체는 사용자의 요청을 처리한 결과를 서버에서 웹 브라우저로 전달하는 정보를 저장
- 서버는 응답 헤어와 요청 처리 결과 데이터를 웹 브라우저로 보냅니다.
- JSP 컨테이너는 서버에서 웹 브라우저로 응답하는 정보를 처리하기 위해 javax.servlet.http.HttpServletResponse 객체 타입의 response 내장 객체를 사용

### 페이지 이동 관련 메서드
|페이지 이동 관련 메서드|반환유형|설명|
|------|-----|--------|
|sendRedirect(String url)|void|설정한 URL 페이지로 강제 이동합니다.|

### 페이지 이동 방법
- 포워드(forward) 방식<br>
현재 JSP 페이지에서 이동할 URL 로 요청 정보를 그대로 전달하므로 사용자가 최초로 요청한 정보가 이동된 URL에서도 유효합니다. 그러나 이동된 URL이 웹브라우저의 주소 창에 나타나지 않고 처음 요청한 URL이 나타나기 때문에 이동 여부를 사용자가 알 수 없습니다.<br>
**<jsp:forward page="이동할 페이지" />**


- 리다이렉트(redirect) 방식<br>
처음 요청받은 현재 JSP 페이지로부터 이동할 URL을 웹브라우저로 반환합니다. 이때 웹 브라우저에서는 새로운 요청을 생성하여 이동할 URL에 다시 요청을 전송하므로 처음 보낸 요청 정보가 이동된 URL에서는 유효하지 않습니다. 즉, 클라이언트가 새로 페이지를 요청한 것과 같은 방식으로 페이지가 이동합니다. 따라서 이동된 URL이 웹 브라우저의 주소창에 보이는 것입니다.<br>
**response.sendRedirect("이동할 페이지")**

### response01.jsp
```jsp
<%@ page contentType="text/html; charset=utf-8"%>
<html>
<head>
<title>Implicit Objects</title>
</head>
<body>
	<form action="response01_process.jsp" method="post">
		<p>	아 이 디 : <input type="text" name="id">
		<p>	비밀번호 : <input type="text" name="passwd">
		<p>	<input type="submit" value="전송">
	</form>
</body>
</html>
```

### response01_process.jsp
```jsp
<%@ page contentType="text/html; charset=utf-8"%>
<html>
<head>
<title>Implicit Objects</title>
</head>
<body>
	<%
		request.setCharacterEncoding("utf-8");
		String userid = request.getParameter("id");
		String password = request.getParameter("passwd");

		if (userid.equals("관리자") && password.equals("1234")) {
			response.sendRedirect("response01_success.jsp");
		} else {
			response.sendRedirect("response01_failed.jsp");
		}
	%>
</body>
</html>
```

#### day04/response01_success.jsp
```jsp
<%@ page contentType="text/html; charset=utf-8"%>
<html>
<head>
<title>Implicit Objects</title>
</head>
<body>
	로그인을 성공했습니다!!
</body>
</html>
```

#### day04/response01_failed.jsp
```jsp
<%@ page contentType="text/html; charset=utf-8"%>
<html>
<head>
<title>Implicit Objects</title>
</head>
<body>
	<p>로그인을 실패했습니다.
	<p><a href="./response01.jsp"> 로그인가기</a>
</body>
</html>
```

### 응답 HTTP 헤더 관련 메서드
- 응답 HTTP 헤더 관련 메서드는 서버가 웹 브라우저에 응답하는 정보에 헤더를 추가하는 기능을 제공
- 헤더 정보에는 주로 서버에 대한 정보가 저장되어 있습니다.

|응답 HTTP 헤더 관련 메서드|반환유형|설명|
|-------|-----|--------|
|addCookie(Cookie cookie)|void|쿠키를 추가합니다.|
|addDateHeader(String name, long date)|void|설정한 헤더 이름 name에 날짜/시간을 추가합니다.|
|addHeader(String name String value)|void|설정한 헤더 이름 name에 value를 추가합니다.|
|addIntHeader(String name, int value)|void|설정한 헤더 이름 name에 정수 값 value를 추가|
|setDateHeader(String name, long date)|void|설정한 헤더 이름 name에 날짜/시간을 설정|
|setHeader(String name, String value)|void|설정한 헤더 이름 name에 문자열 값 value 설정|
|setIntHeader(String name, int value)|void|설정한 헤더 이름 name에 정수 값 value를 설정|
|containsHeader(String name)|boolean|설정한 헤더 이름 name이 HTTP 헤더에 포함되었는지 여부를 확인|
|getHeader(String name)|String|설정한 헤더 이름 name 값을 가져옵니다.|

### response02.jsp
```jsp
<%@ page contentType="text/html; charset=utf-8" %>
<html>
<body>
	이 페이지는 5초마다 새로고침 됩니다.<br>
	<%
		response.setIntHeader("Refresh", 5);
	%>
	<%=java.util.Calendar.getInstance().getTime()%>
</body>
</html>
```

### 응답 콘텐츠 관련 메서드 
|응답 콘텐츠 관련 메서드|반환유형|설명|
|-------|-----|--------|
|setContentType(String type)|void|웹브라우저에 응답할 MIME 유형을 설정합니다.|
|getContentType()|String|웹브라우저에 응답할 MIME 유형을 가져옵니다.|
|setCharacterEncoding(String charset)|void|웹브라우저에 응답할 문자 인코딩을 설정합니다.|
|getCharacterEncoding()|String|웹브라우저에 응답할 문자 인코딩을 가져옵니다.|
|sendError(int status_code, String message)|void|웹브아루저에 응답할 오류(코드 및 오류메세지)를 설정|
|setStatus(int statuscode)|void|웹브라우저에 응답할 HTTP 코드를 설정합니다.|

### response03.jsp
```jsp
<%@ page contentType="text/html; charset=utf-8" %>
<html>
<body>
	<%
		response.setCharacterEncoding("utf-8");
		response.setContentType("text/html; charset=utf-8");
	%>
	문자 인코딩 : <%=response.getCharacterEncoding()%><br>
	콘텐츠 유형 : <%=response.getContentType()%>
</body>
</html>
```


#### day04/response03.jsp
```
<%@ page contentType="text/html; charset=utf-8"%>
<html>
<body>
	<%
		response.sendError(404, "요청 페이지를 찾을 수 없습니다.");
	%>
</body>
</html>
```

### hellojsp.jsp

```jsp
기본적으로 갖고 있어야 하는 정보들이 기입이 되어있다. 

<%@page import="java.util.Random"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
    
<!-- JSP(Java Server Page) : 연산능력을 가지고 있는 html
자바코드도 사용할 수 있고 자바스크립트를 사용하는데도 제약이 없다.-->

<%
	//스크립트릿(Scriptlet) : jsp에서 자바코드를 사용하기 위해 지정하는 영역
	//request(요청처리객체),response(응답처리객체) 등의 객체는
	//Servlet클래스가 가진 객체이다.

	//jsp가 servlet으로 변환되는 과정에서 service()메서드의 지역변수로
	//선언되므로 jsp에서 따로 선언하지 않고 사용할 수 있는 것이다.
	
	String ip = request.getRemoteAddr();
	
	//난수만들기
	Random rnd = new Random();//헤더파일에 Random클래스에 import 필수
	int num = rnd.nextInt(5) + 1;

	int a = 10;
	int b = 7;
	int c = 30;
%>
    
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<p>&lt;%@내용%&gt; : jsp헤더- 실행시 인코딩, import등을 위해 반드시 지정해야 하는 영역</p>
	<p>&lt;% 자바코드 %&gt; : 스크립트릿 - jsp에서 자바코드를 작성하기 위해 생성하는 영역</p>
	<p>&lt;%= 변수명 %&gt; : 스크립트릿의 출력코드</p>
	<%= ip %> <!-- 주의 변수뒤에 ;콜론 붙히면 안됨  -->
	<br>
	난수 : <%= num %>
</body>
</html>
```
![image](image/jsp1.png)

## JSP의 작동 원리
1. Client가 web browser로 JSP페이지를 요청한다.
2. jsp 컨테이너(HttpJspBase)가 jsp 파일을 Servlet파일(.java)로 변환
	- jsp 컨테이너 : jsp를 Servlet으로 변환하는 프로그램
	- jsp 컨테이너 역시 서블릿으로 구현된 프로그램
3. 예) hello.jsp -> hello_jsp.java -> hello_jsp.class 로 컴파일 된다.
4. 요청한 Client에 html파일 형태로 응답

### HttpJspBase
- HttpJspBase클래스는 HttpServlet을 상속받고 있다. 이는 JSP가 Servlet으로 처리되기 위한 기능을 상속받을 수 있다.
- JSP는 Servlet으로 실행될 때 필요한 환경을 설정하고 실행을 지원한다.
```java
/**
 * This is the super class of all JSP-generated servlets.
 *
 * @author Anil K. Vijendran
 */
public abstract class HttpJspBase extends HttpServlet implements HttpJspPage {

    private static final long serialVersionUID = 1L;

    protected HttpJspBase() {
    }

    @Override
    public final void init(ServletConfig config)
        throws ServletException
    {
        super.init(config);
        jspInit();
        _jspInit();
    }

    @Override
    public String getServletInfo() {
        return Localizer.getMessage("jsp.engine.info", Constants.SPEC_VERSION);
    }

    @Override
    public final void destroy() {
        jspDestroy();
        _jspDestroy();
    }

    /**
     * Entry point into service.
     */
    @Override
    public final void service(HttpServletRequest request, HttpServletResponse response)
        throws ServletException, IOException
    {
        _jspService(request, response);
    }

    @Override
    public void jspInit() {
    }

    public void _jspInit() {
    }

    @Override
    public void jspDestroy() {
    }

    protected void _jspDestroy() {
    }

    @Override
    public abstract void _jspService(HttpServletRequest request,
                                     HttpServletResponse response)
        throws ServletException, IOException;
}
```

### 컴파일된 파일의 위치
- work\.metadata\.plugins\org.eclipse.wst.server.core\tmp0\work\Catalina\localhost\Test\org\apache\jsp

```java
/*
 * Generated by the Jasper component of Apache Tomcat
 * Version: Apache Tomcat/9.0.74
 * Generated at: 2023-12-13 07:03:09 UTC
 * Note: The last modified time of this file was set to
 *       the last modified time of the source file after
 *       generation to assist with modification tracking.
 */
package org.apache.jsp;

import javax.servlet.*;
import javax.servlet.http.*;
import javax.servlet.jsp.*;
import java.util.Random;

public final class hello_jsp extends org.apache.jasper.runtime.HttpJspBase
    implements org.apache.jasper.runtime.JspSourceDependent,
                 org.apache.jasper.runtime.JspSourceImports {

  private static final javax.servlet.jsp.JspFactory _jspxFactory =
          javax.servlet.jsp.JspFactory.getDefaultFactory();

  private static java.util.Map<java.lang.String,java.lang.Long> _jspx_dependants;

  private static final java.util.Set<java.lang.String> _jspx_imports_packages;

  private static final java.util.Set<java.lang.String> _jspx_imports_classes;

  static {
    _jspx_imports_packages = new java.util.HashSet<>();
    _jspx_imports_packages.add("javax.servlet");
    _jspx_imports_packages.add("javax.servlet.http");
    _jspx_imports_packages.add("javax.servlet.jsp");
    _jspx_imports_classes = new java.util.HashSet<>();
    _jspx_imports_classes.add("java.util.Random");
  }

  private volatile javax.el.ExpressionFactory _el_expressionfactory;
  private volatile org.apache.tomcat.InstanceManager _jsp_instancemanager;

  public java.util.Map<java.lang.String,java.lang.Long> getDependants() {
    return _jspx_dependants;
  }

  public java.util.Set<java.lang.String> getPackageImports() {
    return _jspx_imports_packages;
  }

  public java.util.Set<java.lang.String> getClassImports() {
    return _jspx_imports_classes;
  }

  public javax.el.ExpressionFactory _jsp_getExpressionFactory() {
    if (_el_expressionfactory == null) {
      synchronized (this) {
        if (_el_expressionfactory == null) {
          _el_expressionfactory = _jspxFactory.getJspApplicationContext(getServletConfig().getServletContext()).getExpressionFactory();
        }
      }
    }
    return _el_expressionfactory;
  }

  public org.apache.tomcat.InstanceManager _jsp_getInstanceManager() {
    if (_jsp_instancemanager == null) {
      synchronized (this) {
        if (_jsp_instancemanager == null) {
          _jsp_instancemanager = org.apache.jasper.runtime.InstanceManagerFactory.getInstanceManager(getServletConfig());
        }
      }
    }
    return _jsp_instancemanager;
  }

  public void _jspInit() {
  }

  public void _jspDestroy() {
  }

  public void _jspService(final javax.servlet.http.HttpServletRequest request, final javax.servlet.http.HttpServletResponse response)
      throws java.io.IOException, javax.servlet.ServletException {

    if (!javax.servlet.DispatcherType.ERROR.equals(request.getDispatcherType())) {
      final java.lang.String _jspx_method = request.getMethod();
      if ("OPTIONS".equals(_jspx_method)) {
        response.setHeader("Allow","GET, HEAD, POST, OPTIONS");
        return;
      }
      if (!"GET".equals(_jspx_method) && !"POST".equals(_jspx_method) && !"HEAD".equals(_jspx_method)) {
        response.setHeader("Allow","GET, HEAD, POST, OPTIONS");
        response.sendError(HttpServletResponse.SC_METHOD_NOT_ALLOWED, "JSP들은 오직 GET, POST 또는 HEAD 메소드만을 허용합니다. Jasper는 OPTIONS 메소드 또한 허용합니다.");
        return;
      }
    }

    final javax.servlet.jsp.PageContext pageContext;
    javax.servlet.http.HttpSession session = null;
    final javax.servlet.ServletContext application;
    final javax.servlet.ServletConfig config;
    javax.servlet.jsp.JspWriter out = null;
    final java.lang.Object page = this;
    javax.servlet.jsp.JspWriter _jspx_out = null;
    javax.servlet.jsp.PageContext _jspx_page_context = null;


    try {
      response.setContentType("text/html; charset=UTF-8");
      pageContext = _jspxFactory.getPageContext(this, request, response,
      			null, true, 8192, true);
      _jspx_page_context = pageContext;
      application = pageContext.getServletContext();
      config = pageContext.getServletConfig();
      session = pageContext.getSession();
      out = pageContext.getOut();
      _jspx_out = out;

      out.write("\n");
      out.write("\n");
      out.write("    \n");
      out.write("<!-- JSP(Java Server Page) : 연산능력을 가지고 있는 html\n");
      out.write("자바코드도 사용할 수 있고 자바스크립트를 사용하는데도 제약이 없다.-->\n");
      out.write("\n");

	//스크립트릿(Scriptlet) : jsp에서 자바코드를 사용하기 위해 지정하는 영역
	//request(요청처리객체),response(응답처리객체) 등의 객체는
	//jsp가 web페이지로 전환되는 과정에서 만나는 Servlet클래스가 가진 객체이므로
	//jsp에서는 Servlet클래스가 허용하는 범위안에서 객체를 마음대로 가져다 사용할 수 있다.
	String ip = request.getRemoteAddr();
	
	//난수만들기
	Random rnd = new Random();//헤더파일에 Random클래스에 import 필수
	int num = rnd.nextInt(5) + 1;

      out.write("\n");
      out.write("    \n");
      out.write("<!DOCTYPE html>\n");
      out.write("<html>\n");
      out.write("<head>\n");
      out.write("<meta charset=\"UTF-8\">\n");
      out.write("<title>Insert title here</title>\n");
      out.write("</head>\n");
      out.write("<body>\n");
      out.write("	<p>&lt;%@내용%&gt; : jsp헤더- 실행시 인코딩, import등을 위해 반드시 지정해야 하는 영역</p>\n");
      out.write("	<p>&lt;% 자바코드 %&gt; : 스크립트릿 - jsp에서 자바코드를 작성하기 위해 생성하는 영역</p>\n");
      out.write("	<p>&lt;%= 변수명 %&gt; : 스크립트릿의 출력코드</p>\n");
      out.write("	");
      out.print( ip );
      out.write(" <!-- 주의 변수뒤에 ;콜론 붙히면 안됨  -->\n");
      out.write("	<br>\n");
      out.write("	난수 : ");
      out.print( num );
      out.write("\n");
      out.write("</body>\n");
      out.write("</html>");
    } catch (java.lang.Throwable t) {
      if (!(t instanceof javax.servlet.jsp.SkipPageException)){
        out = _jspx_out;
        if (out != null && out.getBufferSize() != 0)
          try {
            if (response.isCommitted()) {
              out.flush();
            } else {
              out.clearBuffer();
            }
          } catch (java.io.IOException e) {}
        if (_jspx_page_context != null) _jspx_page_context.handlePageException(t);
        else throw new ServletException(t);
      }
    } finally {
      _jspxFactory.releasePageContext(_jspx_page_context);
    }
  }
}

```

## 다른 페이지로 데이터 전송하기

### parameter.jsp 만들기
- jsp는 어떻게 사용을 해야할까?
- 만약 DB에서 정보가 넘어왔다면 어떻게 처리해야할까??
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
    
<%
	//DB에서 다섯개의 과일목록을 가지고 왔다고 가정
	String[] fruit = {"사과","배","참외","토마토","복숭아"};
%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<ul>
		<li><%= fruit[0] %></li>
		<li><%= fruit[1] %></li>
		<li><%= fruit[2] %></li>
		<li><%= fruit[3] %></li>
		<li><%= fruit[4] %></li> 이렇게 작성을 하면 나중에 6개의 과일이 넘어오면 처리하기가 힘들다.
    
    <%for(int i = 0; i<fruit.length;i++){ %>
		<li><%= fruit[i] %></li>
		<%} %>
    
	</ul>

</body>
</html>
```

### 테이블을 사용하여 구구단 출력하기

![image](https://user-images.githubusercontent.com/54658614/230828122-122aa577-0c41-4af9-8845-d1dd3319d8f4.png)


```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>

<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">

<html>

<head>

	<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
	<title>Insert title here</title>
	
	<style>
		table{border:2px solid black;
		      border-collapse:collapse;} -> 두줄짜리 외곽선 한줄로 만듦

		tr{height:30px;}

		td{width:110px;
		  text-align:center;}
	</style>
	
</head>
<!-- 주석 -->
<!-- html주석(그대로 노출되어 보안성이 떨어짐) : html주석은 컴파일시에 자바코드로 전환된다.-->

<%-- jsp주석(보안성이 좋다) : jsp주석은 컴파일시에 자바코드로 전환되지 않는다. --%>

<!-- jsp를 실행한 후 웹페이지에서 우측클릭
소스보기를 통해서 확인해보면 두 주석의 차이점을 알 수 있다.
그러므로 id:a123 / pwd:asdf 와 같이 html주석에 중요한 정보를 담아놓으면 안된다 -->
<body>

	구구단

  테이블 안에서 자바코드를 사용해야 하기 때문에 스크립트릿이 바디 안에 있어야 된다.
  for문은 자바코드이기 때문에 스크립트릿을 잘 사용하고 어느 부분은 html 태그이니까 스크립트릿으로 묶으면 안되는 판단을 할 줄 알아야 한다. 
  스크립트릿은 db연결하고 할 때 여러가지 필요한 메서드를 하나에 모아서 릿으로 하고 거기서 개념설명을 한다음에 그게 어떤식으로 클래스로 2개 3개 나눠지는 설명할거다.
	<table border="1">
			<% for(int i = 1; i <= 9; i++ ){%>
			<tr>
				<% for(int j = 2; j <= 9; j++){%>
				<td>
				
				<%-- 이렇게 해도 되고...
					<%= j %> * <%= i %> = <%= j * i %> --%>
					<%= j + " * " + I + "=" + j*i %>
				<%-- 이렇런식으로 해도 된다. --%>
					<% String str = String.format("%d * %d = %d", j, i, j * i); %>
					<%= str %>
				</td>
				<% } %>
			</tr>
			<% } %>	
	</table>
	
</body>

</html>
```

## JSP와 DataBase연결하기
DB를 연결하기 위해서 알아야할 자바의 개념이 있다.

- VO(Value Object) : 한 개 또는 그 이상의 속성들을 묶은 객체를 의미한다.
DB를 연동하고 사람의 정보를 db에서 가져왔다고 가정하면 결과적으로 jsp의 body 부분에서 출력을 하게된다.

![image](https://user-images.githubusercontent.com/54658614/230829218-5a20d2be-3ef3-4386-a65d-fa63d03feb24.png)

#### test.jsp
```
<%@page import="java.util.List"%>
<%@page import="vo.PersonVO"%>
<%@page import="java.util.ArrayList"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
    
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>

<% 
	//준비해놓은 VO객체를 ArrayList에 적재한다.
	ArrayList<PersonVO> pList = new ArrayList<>(); 
ArrayList는 어차피 List 인터페이스의 구현 클래스 이기 때문에 부모로 시작해서 자식객체를 만드는게 더 빠르다고 함
	List<PersonVO> pList = new ArrayList<>(); 가 더 효율적이라고 함

	//DB에서 두명의 유저 정보를 가지고 와서 pList에 저장했다고 가정
	
	pList.add(new PersonVO( "홍길동", 20, "011-123-4567" ));
	pList.add(new PersonVO( "김길동", 40, "010-456-1234" ));
	pList.add(new PersonVO( "박길동", 27, "012-111-2222" ));
	pList.add(new PersonVO( "조길동", 13, "015-222-3333" ));
	pList.add(new PersonVO( "독고길동", 70, "016-333-4444" ));	
%>

<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>

<style type="text/css">
	table{border:1px solid black;
	      border-collapse:collapse;}
	      
	td{width:100px; text-align:center;}
	.td3{width:150px;}
</style>

</head>

<body>
	<table border="1">
	<caption>::: 개인정보 목록 :::</caption>
		
		<tr>
			<th>이름</th>
			<th>나이</th>
			<th>전화번호</th>
		</tr>
		
		<% for(int i = 0; i < pList.size(); i++){ %>		
		<tr>
			<td> <%= pList.get(i).getName() %> </td>
			<td> <%= pList.get(i).getAge() %> </td>
			<td class="td3"> <%= pList.get(i).getTel() %> </td>
		</tr>	
		<% } %>
		
	</table>
</body>

</html>
```

![image](https://user-images.githubusercontent.com/54658614/230829447-57c8e230-ad39-4e1e-ae8f-e35c2370ae53.png)

#### PersonVO 클래스 생성

```java
package vo;

public class PersonVO {
	//VO : ValueObject – DB에서 넘어온 row의 정보를 저장하기 위한 클래스 개념

	//VO클래스에서 만드는 변수명은 실제 DB의 컬럼명과 똑같은 이름으로 만들어 주는 것이 	//좋다
	private String name; //이름
	private int age; //나이
	private String tel; //전화번호

	
	public PersonVO() {
		

	}
	
	public PersonVO( String name, int age, String tel) {
		this.name = name;
		this.age = age;
		this.tel = tel;
	}
	
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public int getAge() {
		return age;
	}
	public void setAge(int age) {
		this.age = age;
	}
	public String getTel() {
		return tel;
	}
	public void setTel(String tel) {
		this.tel = tel;
	}
}
```
#### jsp_input.jsp 생성

![image](https://user-images.githubusercontent.com/54658614/230830605-9ff370d9-eafa-4efb-a53d-dd7526cef5ea.png)

위 사진과 같이 input 태그에 입력한 내용을 다른 jsp로 전송해보자.

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>

<!DOCTYPE html>

<html>

<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">

<title>Insert title here</title>

<script type="text/javascript">
	function send(f) {
		var name = f.name.value.trim(); 유효성 체크 할 거 아니면 굳이 작성하지 않아도 됨
		var age = f.age.value.trim();
		var tel = f.tel.value.trim();
		
		//유효성체크 메서드
		if(name == ''){
			alert("이름을 입력하세요");
			f.name.focus();
			return;
		}
		
		if(tel == ''){
			alert("전화번호를 입력하세요");
			f.tel.focus();
			return;
		}
		
		//숫자판단 정규식
		var pattern = /^[0-9]{1,3}$/;
		if(!pattern.test(age)){
			alert("나이는 정수로 입력하세요");
			f.age.focus();
			return;
		}
		
		f.action = "test_param.jsp"; //url mapping이 아닌 jsp페이지를 명시
		//f.method = "GET";
		f.submit();//지정된 액션 페이지로 전송
	}
	
</script>

</head>

<body>
	<form action="">
	
		이름:<input name="m_name"><br> 
		나이:<input name="age"><br>
		전화:<input name="tel"><br> 
		
		<input type="button" value="파라미터 전송" onclick="send1(this.form)"> 
	
	</form>
</body>

</html>
```
input태그에 적어준 값을 적어서 test_param.jsp로 보내주자

#### test_param.jsp 생성하기
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>

<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">

<%	
	//JSP내장객체 : 보이진 않지만 JSP가 실행되면 존재하는 객체
	//이것들은 Servlet을 만들게 되면 service메서드에 존재하는 객체들이다.
	
	//request(요청처리객체)
	//response(응답처리객체)

	//ex01_jsp_input.jsp에서 넘겨준 세 개의 파라미터를 수신해보자
	//text_param.jsp?m_name=a&age=10&tel=010;

	String t_name = request.getParameter("m_name");
	int age = Integer.parseInt(request.getParameter("age"));
	String tel = request.getParameter("tel");

	//파라미터로 넘어오는 모든 값은 문자열이거나 바이너리타입이다.
	//파라미터 데이터는 위의 두 타입이 아닌 정수, 문자, 실수타입 등으로 넘어오는
	//경우가 아예 없다!!
%>
<html>

<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>

	<style>
		table{border:1px solid black;
		      border-collapse:collapse;}
	</style>

</head>

<body>
	<table border="1">
		<caption>수신된 데이터</caption>
		
		<tr>
			<th>이름</th>
			<td><%= name %></td> <%-- <% out.print(name) %> --%>
		<tr>
		
		<tr>
			<th>나이</th>
			<td><%= age %></td>
		<tr>
		
		<tr>
			<th>전화번호</th>
			<td><%= tel %></td>
		<tr>
	</table>
	
</body>

</html>

```

