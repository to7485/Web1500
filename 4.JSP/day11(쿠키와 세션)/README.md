# 쿠키

- 세션과 마찬가지로 클라이언트와 웹 서버 간의 상태를 지속적으로 유지하는 방법입니다.
- 세션과 달리 상태 정보를 웹서버가 아닌 클라이언트에 저장합니다.
- 쿠키는 클라이언트의 정보를 웹 브라우저에 저장하므로 이후에 웹 서버로 전송되는 요청에는 쿠키가 가지고 있는 정보가 포함됩니다.
- 웹 브라우저가 접속했던 웹 사이트에 관한 정보와 개인 정보가 기록되기 때문에 보안에 문제가 있습니다.

## 쿠키의 동작과정

- **쿠키 생성단계** : 쿠키를 사용하려면 먼저 쿠키를 생성해야 합니다. 쿠키는 주로 웹 서버 측에서 생성합니다. 생성된 쿠키는 응답 데이터와 함께 저장되어 웹 브라우저에 전송됩니다.
- **쿠키 저장단계** : 웹브라우저는 응답 데이터에 포함된 쿠키를 쿠키 저장소에 보관합니다. 쿠키는 종류에 따라 메모리나 파일로 저장됩니다.
- **쿠키 전송단계** : 웹브라우저는 한 번 저장된 쿠키를 요청이 있을 때마다 웹 서버에 전송합니다. 웹 서버는 웹 브라우저가 전송한 쿠키를 사용하여 필요한 작업을 수행할 수 있습니다.

> 일단 웹 브라우저에 쿠키가 저장되면 웹 브라우저는 쿠키가 삭제되기 전까지 웹 서버에 쿠키를 전송합니다

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
|setPath(String)|void|쿠키의 유효기간을 설정합니다.|
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

### ex01_Cookie.jsp 생성하기

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
    
    서버 쿠키 생성, 응답 -> 개인의 브라우저에 쿠키 저장 -> 브라우저가 요청시 요청 헤더에 담아서 서버에 전송
    
    <% Cookie cookie = new Cookie("param1","abcdefg");
    response.addCookie(cookie);%>
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
![image](https://user-images.githubusercontent.com/54658614/233546625-a0a67b82-7b25-4f61-9ff8-b8667a7f8a08.png)

응답헤더에 쿠키가 잘 저장이 되어있는걸 볼 수 있다.

쿠키는 개인의 컴퓨터에 저장이된다.

![image](https://user-images.githubusercontent.com/54658614/233546899-d581dc18-3677-40ad-9e07-91caef204449.png)

쿠키는 브라우저 자체에 저장이 된다.

쿠키의 이름과 들어있는 내용들을 조회해보자.

### ex02_Cookie.jsp 생성하기

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
    
<% Cookie[] cookies = request.getCookies(); 
	for(Cookie cookie : cookies){
		out.println(cookie.getName() + " : " + cookie.getValue()+"<br>");
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
쿠키는 브라우저에 저장이 되고 요청시마다 전송이 되기 때문에 보안에 취약하다는 문제가 있다.<br>
그래서 텍스트를 그대로 저장하지는 않고 암호화를 통해서 저장한다<br>

아래는 네이버 로그인 후 블로그에 접속했을 때 까지의 쿠키에 대한 정보이다.

![image](https://user-images.githubusercontent.com/54658614/233548583-18d97aa9-e171-4884-97f8-3a704ad4039c.png)

Domain : 현재 응답한 서버의 도메인 (도메인만 내 쿠키의 Value에 접근할 수 있다고 보면 된다.)<br>
Path : 쿠키의 경로<br>
Expires : 만료시간을 정할 수 있다.( ex)일주일동안 팝업창 띄우지 않기)<br>
HttpOnly : 원래 쿠키는 JS와 같은 프론트단에서도 접근할 수 있다. 그러면 해커의 공격에 더 취약해지기 때문에<br> &nbsp;&nbsp;&nbsp;&nbsp; HttpOnly를 체크하면 Servlet과 같은 소스에서만 접근할 수 있도록 해준다.





