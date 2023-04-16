## EL 표기법
- 익스프레션(expression)이라는 단어를 사전에서 찾아보면 여러가지 뜻이 있지만, 프로그래밍에서는 대개 '식(式)'이라는 뜻으로 사용됩니다.
- 식이란 수학시간에 배우는 3+4, 2x+7처럼 어떤 값을 산출하는 연산자와 피연산자의 조합을 말합니다. 익스프레션 언어는 이 같은 식을 중심으로 코드를 기술하는 언어입니다.
- 하지만 수학에서 사용하는 식과는 달리 연산자와 피연산자의 조합을 다음과 같이 ${ .. } 로 둘러싸서 표현합니다.

```
${cnt+1} 
```
- 위와 같은 형태의 식을 **EL 식**이라고 합니다.
- EL 식에 포함된 데이터 이름은 기본적으로 **애트리뷰트**의 이름으로 해석됩니다. 
- 애트리뷰트란 **setAttribute, getAttribute, removeAttribute** 메서드를 통해 저장되고 관리되는 데이터를 의미합니다.

## 익스프레션 언어의 기초문법
- EL식의 문법
```
${식}
```
- 위의 '식'의 위치에는 데이터 이름 하나로만 구성된 식이 들어갈 수도 있고, 
- 연산자를 포함하는 식이 들어갈 수도 있습니다. 

```
// 데이터 이름 하나로만 구성된 EL식 
 
${RESULT}
```
```
// 연산자를 포함하는 EL 식
${RESULT + 101}
```

### 데이터 이름 하나로만 구성된 EL 식
- 데이터 이름 하나로 구성된 EL식은 가장 간단한 형태의 EL 식입니다. 이런 EL식 안에 기술되는 데이터 이름은 앞 절에서 언급했던 것 처럼 **애트리뷰트 이름**으로 해석됩니다.
```
${RESULT}   // 애트리뷰트 이름 
```

- 서블릿과 JSP 기술에서 사용할 수 있는 setAttribute, getAttribute, removeAttribute 메서드는 4세트가 있고 그래서 애트리뷰트 종류도 네가지 입니다.
- JSP 페이지에서는 이런 메서드를 호출할 떄 각각 pageContext, request, session, application 내장 객체를 사용합니다.

#### JSP/서블릿 기술에서 사용되는 네 종류의 애트리뷰트

|애트리뷰트의 종류|호출할 때 사용하는 내장 변수|메서드의 소속|
|-----|-----|-------|
|page 애트리뷰트|pageContext 내장 객체|javax.servlet.jsp.JspContext 클래스|
|request 애트리뷰트|request 내장 객체|javax.servlet.ServletRequest 인터페이스|
|session 애트리뷰트|session 내장 객체|javax.servlet.http.HttpSession 인터페이스|
|application 애트리뷰트|application 내장 객체|javax.servlet.ServletContextg 인터페이스|

#### EL 식 안에 있는 데이터 이름이 해석되는 순서
page 애트리뷰트 -> request 애트리뷰트 -> session 애트리뷰트 -> application 애트리뷰트

- 그런데 어떤 경우에는 이 순서에 상관없이 특정한 종류의 애트리뷰트를 꼭 집어서 출력하고 싶을 때도 있을 것입니다.
- 그럴 때는 데이터 이름 앞에 그 데이터 영역에 해당하는 pageScope, requestScope, sessionScope, applicationScope라는 단어를 다음과 같이 표시하시면 됩니다.
	- ${pageScope.SUM} : page 애트리뷰트임을 표시
	- ${requestScope.RESULT} : request 애트리뷰트임을 표시
	- ${sessionScope.CART} : session 애트리뷰트임을 표시
	- ${applicationScope.DB_NAME} : application 애트리뷰트임을 표시 
	
- 위 EL 식에서 사용된 pageScope, requestScope, sessionScope, applicationScope라는 이름은 익스프레션 언어의 내장 객체 이름입니다.

#### 익스프레션 언어의 내장 객체

|내장 객체 이름|표현하는 데이터|객체의 타입|
|-----|---------|----|
|pageScope|page 애트리뷰트의 집합|Map|
|requestScope|request 애트리뷰트의 집합|Map|
|sessionScope|session 애트리뷰트의 집합|Map|
|applicationScope|application 애트리뷰트의 집합|Map|
|param|웹 브라우저로부터 입력된 데이터의 집합|Map|
|paramValues|웹 브라우저로부터 입력된 데이터의 집합<br>(똑같은 이름의 데이터가 여럿일 때 사용)|Map|
|header|HTTP 요청 메세지에 있는 HTTP 헤더의 집합|Map|
|headerValues|HTTP 요청 메시지에 있는 HTTP 헤더의 집합<br>(똑같은 이름의 HTTP 헤더가 여럿일 때 사용)|Map|
|cookie|웹 브라우저로부터 전송된 쿠키의 집합|Map|
|initParam|웹 애플리케이션의 초기화 파라미터의 집합|Map|
|pageContext|JSP 페이지의 환경 정보의 집합|PageContext|

#### param 내장객체
- param은 웹브라우저에서 \<form\> 요소를 통해 입력된 데이터를 가져올 때 사용하는 내장 객체 입니다. 이 객체의 사용방법은 2가지 입니다.
	- ${param.NUM} : 객체의 이름 뒤에 마침표를 찍고, 그 다음에 해당 데이터 이름을 쓰는 것입니다.
	- ${param\["Color"\]} : 두번째 방법은 이 객체의 이름 뒤에 대괄호를 입력하고, 그 안에 작은 따옴표나 큰 따옴표로 묶은 데이터 이름을 쓰는 것입니다.

- 때로는 \<form\> 요소를 통해 똑같은 이름의 데이터가 여러 개 입력될 경우도 있습니다. 예를 들어 checkbox나 select 요소를 통해 데이트가 입력될 때 주로 그런일 이 생깁니다. 
- 그럴 때는 param 내장 객체 대신 paramValues라는 내장 객체를 사용하시면 됩니다.
	 - ${paramValues.ANIMAL\[0\]} : 객체의 이름 뒤에 마침표를 찍고, 그 다음에 데이터 이름을 쓰고, 그 다음에 대괄호 안에 데이터 값의 인덱스를 표시하는 것 입니다.
	 - ${paramValues\["ANIMAL"\]\[1\]} : 객체의 이름 뒤에 두 개의 대괄호를 입력하고, 그 안에 각각 따옴표로 묶은 데이터의 이름과 인덱스를 쓰는 것 입니다.

#### ex01_el.jsp 생성
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">

<% 
	/*
		EL(Expression Language):JSP에서 사용하는 표현식을 조금 더 간결하게 사용하기 위한 표현언어

		1.EL로 값을 표현하려면 4개의 객체(4개 영역은 모두 JSP의 내장 객체이다.)
		- 기본 객체의 영역은 객체의 유효기간이라고도 불리며, 객체를 누구와 공유할 
		  것인가를 나타낸다.


		(1) page scope 영역: 저장된 데이터를 현재 페이지에서만 공유하고 사용한다.
		굳이 자바로 치면 private이랑 비슷하고 너무 폐쇄적이라 잘 안쓴다.
		- page 영역은 한 번의 웹 브라우저(클라이언트)의 요청에 대해 하나의 JSP 
		  페이지가 호출된다.
		- 웹 브라우저의 요청이 들어오면 이때 단 한 개의 페이지만 대응이 된다.
		- 따라서 page 영역은 객체를 하나의 페이지 내에서만 공유한다.

		(2) request scope(가장 많이 사용하는 영역) - 지역개념으로 페이지가 닫히면 
		  종료된다.
		- 같은 request 영역이면 두 개의 페이지가 같은 요청을 공유할 수 있다
		- 따라서 request 영역은 객체를 하나 또는 두 개의 페이지 내에서 공유할 수 있다.
		- 주로 페이지 모듈화에 사용된다.

		(3) session scope(두번째로 많이 사용하는 영역) - 전역개념으로 페이지가 닫혀도 남아있다.
		- 톰캣이 실행될때 자동으로 만들어지는 영역.
		- session 영역은 하나의 웹 브라우저 당 1개의 session 객체가 생성된다
		- 즉, 같은 웹 브라우저 내에서는 요청되는 페이지들은 같은 객체를 공유하게 된다.
		네이버 탭 껐다키며 보여주기 네이버에서 세션을 준다면 내가 만든거에 가져다 쓸수 있음. 근데 잘 안줌 ㅋㅋㅋ

		(4) application scope - 최소한 내가 만든 모든 jsp에서는 값을 공유하는게 가능
		- application 영역은 하나의 웹 어플리케이션 당 1개의 applicaition 객체가 
		  생성된다.
		- 즉, 같은 웹 어플리케이션에 요청되는 페이지들은 같은 객체를 공유한다.
		
		에 실제 존재하는 데이터만 사용할 수 있다.
		
		2.EL 접근형식
		${영역.변수}
	*/

	String msg = "안녕"; <-- 이곳은 위의 4개 객체에 포함되지 않는 일반 스크립트릿 영역이다.
	
	//JSP를 통해서 pageScope영역에 값을 넣는다.
	pageContext.setAttribute("msg", msg);
	
	//JSP를 통해서 requestScope영역에 값을 넣는다.
	request.setAttribute("msg", "requestScope의 영역에 저장됨");
	MAP처럼 KEY값 VALUE값처럼 사용된다.

	session.setAttribute("msg","세션2"); 킷값이 request와 겹침 근데 오류안남;;;
	pageContext.setAttribute("msg","페이지영역");
	
	//JSP를 통해서 sessionScope영역에 값을 넣는다.
	session.setAttribute("msg2", "세션에 저장됨");
%>

<html>

<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
	
</head>

<body>
	
	EL표현식(파라미터) : ${param.msg}<br><!-- aaa.jsp?msg=안녕 <- 써보면 됨 -->
	JSP표현식 스크립트릿에 저장된 데이터: <%=msg %> <br> //원래 알고있던 형태
	EL표현식(requestScope 데이터) : ${requestScope.msg}<br>
	EL표현식(session 데이터) : ${sessionScope.msg2}<br>
	EL표현식(생략) : ${msg}<br> 
	영역을 생략해도 키값이 중복되지 않는다면 그 영역에 있는 데이터를 가져온다.
	영역을 생략했을 때 접근 순서가 정해져 있다.
	
	영역별로 킷값을 저장할 때 킷값을 중복되게 만드는 경우가 거의 없다.
	
	<!-- 생략식 영역참조순서 
		1.pageScope 
		2.requestScope 
		3.sessionScope 
		4.applicationScope
	-->
</body>

</html>
```

![image](https://user-images.githubusercontent.com/54658614/232313191-b8e447aa-9ddc-46f3-8bdf-5b10b2102f37.png)

두번째로 받는 파라미터가 Object 타입이기 때문에 기본자료형,객체 모두 가능하다.

## 익스프레션 언어의 연산자
익스프레션 언어를 이용하면 간단한 연산을 해서 그 결과를 출력할 수도 있습니다.

#### 익스프레션 언어의 연산자

|구분|연산자|설명|
|----|--------|-----|
|산술 연산자|\+ - \* / % div mod|덧셈, 뺄셈, 곱셈, 나눗셈, 나머지|
|비교 연산자|\< \> \<= \>= == != lt gt le ge eq ne|크기와 동등 여부 비교|
|논리 연산자|&& \|\| ! and or not|논리적인 AND와 OR|
|조건 연산자|? :|조건에 따라 두 값 중 하나를 선택|
|엠프티 연산자|empty|데이터의 존재 유무 여부|
|대괄호와 마침표 연산자|\[\] .|집합 데이터에 있는 한 항목을 선택|
|괄호|()|연산자의 우선순위 지정|

#### ex2_el.jsp 생성

```jsp
<html>

<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>

<body>
	EL산술연산자<br>
	
	<!-- \는 EL기법이 아닌 일반문자로 인식 -->
	<!-- 상수는 4가지 영역이 아니어도 사용 가능  -->
	${ 1 + 1 }<br>
	\${ 1 + 1 }<br>	
	\${1 + 1} = ${1 + 1}<br>
	\${2 - 2} = ${2 - 1}<br>
	\${3 * 2} = ${3 * 2}<br> 
	\${10 / 3} = ${10 / 3}<br> ${10 div 3} 확실하게 나누기인걸 인식
	\${10 % 3} = ${10 % 3}<br> ${10 mod 3} 확실하게 나머지 연산인걸 인식
	
	<hr>
	
	EL관계(비교)연산자<br>
	\${3 > 2} = ${3 > 2} 또는 ${3 gt 2}<br><!--  greater than -->
	\${3 >= 2} = ${3 >= 2} 또는 ${3 ge 2}<br><br><!--  greater equal -->
	
	\${3 < 2} = ${3 < 2} 또는 ${3 lt 2}<br><!-- little than -->
	\${3 <= 2} = ${3 <= 2} 또는 ${3 le 2}<br><br><!-- little equal -->
	
	\${3 == 2} = ${3 == 2} 또는 ${3 eq 2}<br><!-- equal -->
	\${3 != 2} = ${3 != 2} 또는 ${3 ne 2}<br><!-- not equal -->
	
	<hr>
	
	EL삼항연산자<br>
	<!-- 파라미터로 넘어온 값(aa.jsp?msg="값있음")이 있을때와 없을때를 비교 -->
	<!-- empty : 비어있는지 확인. -->
	파라미터 값: ${ empty param.msg ? '그래 참이다.' : ‘거짓이다’ }<br>
	파라미터 값: ${ param.msg eq null ? '그래 참이다.' : ‘거짓이다’ }<br>
	
	<hr>
	
	EL논리 연산자<br>
	파라미터 값: 
	${ empty param.abc || param.abc eq 10}<br>
	파라미터 값: 
	${ empty param.abc or param.abc eq 10}
	
</body>

</html>
```

#### ex03_el.jsp 생성하기
- EL표기법을 통해 출력가능한 여러가지 방법들

```jsp
<%@page import="java.util.HashMap"%>
<%@page import="java.util.Map"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">

<% 

	Map<String, Integer> map = new HashMap<String, Integer>();
	map.put("key1", 100);
	map.put("key2", 200);
	map.put("key3", 300);
	map.put("key4", 400);

	request.setAttribute("myMap",map);
	
	//map이 Scope영역중 한군데에는 들어가 있어야 EL표현식이 가능하다 
	pageContext.setAttribute("map1", map);
%>

<html>

<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>

<body>
	driver : ${ pageScope.map1.get("driver") }<br>
	url : ${ map1.get("url") }<br>
	url : ${ map1['url'] }<br><!-- 이렇게도 씀 -->
	user : ${ map1["user"] }<br>
	pwd : ${ map1.pwd }<!-- 가장 편한방법(제일 많이 쓴다) -->

		map에 저장된 데이터 가져오기<br>
	${ myMap.get("key1") } <br>
	${ myMap['key2'] } <br>
	${ myMap["key3"] } <br>
	${ myMap.key4 } <br> <!-- 가장 많이 쓰는 방법 -->

</body>

</html>
```
객체 또한 EL표기법으로 출력할 수 있다.

#### PersonVO 클래스 생성

```java

public class PersonVO {

	String name;
	int age;
	
	public PersonVO() {
		// TODO Auto-generated constructor stub
	}
	
	public PersonVO(String name, int age) {
		this.name = name;
		this.age = age;
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

}
```

#### ex04_el.jsp

```jsp
<% 
	//PersonVO객체
	PersonVO vo = new PersonVO();
	vo.setName("홍길동");
	vo.setAge(20);

	//vo를 Scope영역에 넣는다.
	request.setAttribute("vo", vo);

	PersonVO vo1 = new PersonVO();
	vo.setName("박길동");
	vo.setAge(25);

	//ArrayList객체
	List<PersonVO> list = new ArrayList<>();
	arr.add(vo);
	arr.add(vo1);
	
	request.setAttribute("list", list); 
	스코프 영역안에 저장을 안해놓으면 el표기법으로 출력할 수 없다.
%>

<html>

<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>

<body>
	<!-- {vo.age}  -> vo 객체에 있는 getAge()를 호출하는 기능이므로 vo에 반드시
	getter가 생성되어 잇어야 한다. -->
	이름 : ${ vo.name } 또는 ${ requestScope.vo.name } <br>
	나이 : ${ vo.age } 또는 ${ vo['age'] }

	<hr>

	List에 담긴 데이터 가져오기<br>
	${ list[0].name } / ${ lis[0].age} <br>
	
</body>

</html>
```
