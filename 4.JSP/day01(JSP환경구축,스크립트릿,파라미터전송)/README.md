# 실습 환경 구축하기

1. WEB1500 폴더 아래 JSP 폴더 생성후 안에 util과 work폴더 만들기

![image](https://user-images.githubusercontent.com/54658614/230702953-9d8cc484-f186-4dfb-b6e1-bf43cb5134ab.png)

2. util 폴더에 eclipse와 tomcat 넣기
  - 이클립스 압축폴더는 지우지말고 갖고 있을 것 이후 과정에서 계속 새로 풀어서 사용할 것이기 때문에

![image](https://user-images.githubusercontent.com/54658614/226802076-a4bb3306-b813-496c-bbea-33c8435ad70e.png)

## 톰캣
1. apache.org 사이트로 접속한다.

![image](https://user-images.githubusercontent.com/54658614/226802448-d7ab5716-b0eb-4ec1-8bcf-8d005e598c5a.png)

2. 스크롤을 많이 내리면 T 부분에 톰캣이 있다.

![image](https://user-images.githubusercontent.com/54658614/226802570-87c4a3c5-cbe9-4659-9877-acd73a188d32.png)

3. 왼쪽에 다운로드 받는 탭이 있고 원하는 버전을 선택한 뒤 운영체제에 맞는 파일을 다운로드 받는다.

![image](https://user-images.githubusercontent.com/54658614/226802738-bf19dc32-0f6f-4429-82f2-df49b636879f.png)

## 톰캣의 구조
![image](https://user-images.githubusercontent.com/54658614/226803279-57f73f5d-113c-4019-a4cd-9f307c28a759.png)

#### 톰캣 디렉토리 설명

|디렉토리 이름|설명|
|----|---------|
|bin|톰캣을 실행하고, 종료시키는 스크립트 (.bat , .sh 등) 파일이 들어있다.|
|conf|서버 전체 설정파일 폴더 ( server.xml 등 )|
|lib|톰캣구동하는데 필요한 라이브러리(jar)가 들어있다|
|logs|예외 발생 사항 등의 로그 저장|
|temp|임시 저장용 폴더|
|webapps|웹 어플리케이션 폴더|
|work|jsp 파일을 서블릿형태로 변환한 java 파일과 class 파일이 저장|

#### 톰캣 주요 파일들

|파일 이름|설명|
|----|---------|
|context.xml|세션,쿠키 저장 경로 등을 지정하는 설정 파일이다. |
|server.xml|Tomcat의 주 설정 파일로 접근/접속에 관한 설정이 주를 이룬다.|
|web.xml|Tomcat의 환경설정 파일이며 서블릿, 필터, 인코딩 등을 설정할 수 있다.<br>가장 먼저 읽는 파일 DefaultServlet 지정 및 Servlet-mapping|


4. conf폴더의 server.xml을 켜 서버와 관련된 설정을 해주자(메모장으로 열어주자)

![image](https://user-images.githubusercontent.com/54658614/226803820-1869e834-76b0-4719-b4bd-e134cde340fa.png)

5. 주석처리가 되어있지 않는 Connector 부분의 포트번호를 9090으로 바꿔주자
    - 데이터베이스가 8080포트를 차지하게 될 것이므로 최고한 8080,8081은 피해주자.
  
![image](https://user-images.githubusercontent.com/54658614/226804150-6f98a567-7f6c-43e7-9b9a-2b378f101c80.png)

## 이클립스 설정하기

1. 인코딩 타입 설정하기
Window > Preferences

![image](https://user-images.githubusercontent.com/54658614/226804554-8990af76-be15-4b76-90f0-bd5ef2087bf8.png)

General > Workspace > Text file encoding
한글이 깨지지 않게 설정을 해주자.

![image](https://user-images.githubusercontent.com/54658614/226804733-7137a44d-9a9b-48b6-b295-7fdb7bb7f70f.png)

General > Web Brower

![image](https://user-images.githubusercontent.com/54658614/226804993-fcadf6ae-6ce6-4f3f-bff8-833f71b2d460.png)

2. 서버 설정하기

Server > Runtime Environments

![image](https://user-images.githubusercontent.com/54658614/226805141-0a338172-9b35-440c-8392-ed51ae01c340.png)

톰캣 버전 선택하고 Next 누르기 (수업할 때는 8.5버전을 사용하지만 상황에 따라서 유동적으로 설치한 버전 선택해서 하기)

![image](https://user-images.githubusercontent.com/54658614/226805263-87e4d715-0ffd-4dae-851c-7b065806ae02.png)

Browse 눌러서 톰캣이 설치된 경로 잡아주기

![image](https://user-images.githubusercontent.com/54658614/226805381-a3875021-bcef-475e-8852-19270c5f2336.png)

bin 폴더가 눈으로 보이는 곳에서 폴더 선택하기(bin 폴더까지 절대로 들어가지 말것!!)

![image](https://user-images.githubusercontent.com/54658614/226805537-77d9ab82-74a1-4a04-aef8-a7718bff4d62.png)

Finish 누르기

3. Web의 인코딩 타입 변경하기

Web > CSS Files

![image](https://user-images.githubusercontent.com/54658614/226806029-5a7eda11-2c77-464b-ba5c-353f14bb81f4.png)

Web > HTML Files

![image](https://user-images.githubusercontent.com/54658614/226806171-1d1356f1-460b-4929-883f-7e0258e4e068.png)


Web > JSP Files

![image](https://user-images.githubusercontent.com/54658614/226806216-a2946c2f-463c-4252-8ca7-3cd12a6da6c1.png)

### 프로젝트 생성하기

File > New > Dynamic Web Project

![image](https://user-images.githubusercontent.com/54658614/226806476-d4e2ac7b-6a97-4e18-bf8f-51284c78b41f.png)

톰캣, 모듈버전(3.1로 잡혀있는지) 확인 후 finish

![image](https://user-images.githubusercontent.com/54658614/226806619-cd77fd30-19cf-4371-807e-3981021559f0.png)

## Servlet (Server + let)
- 클라이언트의 요청을 처리하고, 그 결과를 다시 반환해주는 Servlet클래스의 구현 규칙을 지킨 자바 웹 프로그래밍 기술

예를 들어, 어떠한 사용자가 로그인을 하려고 할 때. 사용자는 아이디와 비밀번호를 입력하고, 로그인 버튼을 누를것이다.<br>
그때 서버는 클라이언트의 아이디와 비밀번호를 확인하고, 다음 페이지를 띄워주어야 하는데, 이러한 역할을 수행하는<br>
것이 바로 서블릿(Servlet)이다. 그래서 서블릿은 자바로 구현 된 *CGI라고 흔히 말합니다.<br>

### Servlet의 특징
- 클라이언트의 요청에 대해 동적으로 작동하는 웹 어플리케이션 컴포넌트
- html을 사용하여 요청에 응답한다.
- Java Thread를 이용하여 동작한다.
- MVC 패턴에서 Controller로 이용된다.
- HTTP 프로토콜 서비스를 지원하는 javax.servlet.http.HttpServlet 클래스를 상속받는다.
- UDP보다 처리 속도가 느리다.
- HTML 변경 시 Servlet을 재컴파일해야 하는 단점이 있다.

![image](https://user-images.githubusercontent.com/54658614/231054942-f29baabf-1500-48cb-96f4-7898dc4814b6.png)

출처 : Servlet Architecture (출처 : https://www.geeksforgeeks.org/servlet-architecture/ )

#### 구조
- WebContent에서 HTML파일이 아닌 JSP파일을 만든다.

![image](https://user-images.githubusercontent.com/54658614/230703065-42ef6943-1e5a-4176-9a52-c54664c3706a.png)

HTML과는 별로 차이점이 없어보인다.

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
	//jsp가 web페이지로 전환되는 과정에서 만나는 Servlet클래스가 가진 객체이므로
	//jsp에서는 Servlet클래스가 허용하는 범위안에서 객체를 마음대로 가져다 사용할 수 있다.
	String ip = request.getRemoteAddr();
	
	//난수만들기
	Random rnd = new Random();//헤더파일에 Random클래스에 import 필수
	int num = rnd.nextInt(5) + 1;
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
JSP에도 BODY부분이 있기 때문에 웹으로 출력이 될텐데 바로 출력이 될 수는 없다.
중간에 SERVLET이라는 클래스를 거친다.

![image](https://user-images.githubusercontent.com/54658614/230704173-f9cdfc41-4e32-463f-bf1a-d835d011de45.png)

JSP는 SERVLET이 가지고 있는 객체를 가져다가 사용할 수 있다.

#### test2.jsp 만들기
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

<!-- 
jsp를 실행한 후 웹페이지에서 우측클릭 -> 소스보기를 통해서 확인해보면 두 주석의 차이점을 알 수 있다. 그러므로 id:a123 / pwd:asdf 와 같이 html주석에 중요한 정보를 담아놓으면 안된다 -->



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

