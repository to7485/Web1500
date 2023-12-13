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

### 4대 영역객체

|기본 객체|유효 범위|설명|
|-----|-------|-----|
|pageContext|1개 JSP페이지|JSP페이지의 시작부터 끝까지. 해당 JSP 내부에서만 접근가능. 페이지당 1개|
|request|1+개 JSP페이지|요청의 시작부터 응답까지. 다른 JSP로 전달 가능. 요청마다 1개|
|session|n개 JSP페이지|session의 시작부터 종료까지(로그인~로그아웃).클라이언트마다 1개|
|application|context 전체|Web Application의 시작부터 종료까지. context내부 어디서나 접근 가능. 모든 클라이언트가 공유. context마다 1개|

|속성 관련 메서드|설명|
|---------|------|
|void setAttribute(String name, Object value) | 지정된 값(value)을 지정된 속성 이름(name)으로 저장|
|Object getAttribue(String name)|저장된 이름(name)으로 저장된 속성의 값을 반환|
|void removeAttribute(String name)|지정된 이름(name)의 속성을 삭제|
|Enumeration getAttributeNames()|기본 객체에 저장된 모든 속성의 이름을 반환|

### EL 식 안에 있는 데이터 이름이 해석되는 순서
page 애트리뷰트 -> request 애트리뷰트 -> session 애트리뷰트 -> application 애트리뷰트

- 그런데 어떤 경우에는 이 순서에 상관없이 특정한 종류의 애트리뷰트를 꼭 집어서 출력하고 싶을 때도 있을 것입니다.
- 그럴 때는 데이터 이름 앞에 그 데이터 영역에 해당하는 pageScope, requestScope, sessionScope, applicationScope라는 단어를 다음과 같이 표시하시면 됩니다.
	- ${pageScope.SUM} : page 애트리뷰트임을 표시
	- ${requestScope.RESULT} : request 애트리뷰트임을 표시
	- ${sessionScope.CART} : session 애트리뷰트임을 표시
	- ${applicationScope.DB_NAME} : application 애트리뷰트임을 표시 
	
- 위 EL 식에서 사용된 pageScope, requestScope, sessionScope, applicationScope라는 이름은 익스프레션 언어의 내장 객체 이름입니다.


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
## JSTL

### 코어(core) 라이브러리
- JSTL의 코어 라이브러리는 글자 그대로 가장 핵심적인 기능을 제공하는 라이브러리 입니다.
- 이 라이브러리에서 제공하는 커스텀 액션을 이용하면 일반 프로그래밍 언어에서 제공하는 변수 선언, 조건 분기, 반복 수행 등의 로직을 구사할 수 있고, 다른 JSP 페이지를 호출할 수도 있습니다.

![image](https://user-images.githubusercontent.com/54658614/232315276-472aff2f-5b97-4e15-9020-f3f179fceb07.png)

우리가 그동안 DB와 연결하기 위해 사용했던 4개의 라이브러리를 톰캣의 lib에 넣고 사용해도 되지만 한번 넣어놓으면 무조건 까먹으니<br>
이클립스의 lib 폴더에 넣어놨던것.

![image](https://user-images.githubusercontent.com/54658614/232315343-22202a0d-9671-4f00-bc0e-110badb52a97.png)

### <c:set> 커스텀 액션
- \<c:set\>은 변수를 선언하고 나서 그 변수에 초기값을 대입하는 기능의 커스텀 액션입니다.
- \<c:set var="num" value="100" /\>
	- num : 변수의 이름
	- 100 : 초기값

- 이렇게 선언한 변수는 익스프레션 언어의 EL 식 안에서 사용할 수 있습니다. 하지만 JSP 스트립팅 요소 안에서 사용할 수는 없습니다.
- \<c:set\> 커스텀 액션을 이용해서 선언한 변수는 자바 변수가 되는 것이 아니라 page 데이터 영역의 애트리뷰트가 되기 때문입니다.

```
<c:set var="num" value="100" />
...
${num}
```

### <c:if> 커스텀 액션 사용 방법
- \<c:if\> 커스텀 액션은 자바 프로그램의 if문과 비슷한 역할을 하는 커스텀 액션입니다.
- 자바와는 달리 조건식을 괄호 안에 쓰는 것이 아니라 test라는 이름의 애트리뷰트 값으로 지정해야 합니다.

```
<c:if test="${num1 > num2}">
	num1이 더 큽니다.
</c:if>
```
#### <c:choose> 커스텀 액션 사용 방법
- 자바 프로그램에 switch 문이 있다면, JSTL 코어 라이브러리에는 <c:choose> 커스텀 액션이 있습니다.
- \<c:when\>, \<c:otherwise\>라는 커스텀 액션과 함께 사용되는데, 이 두 액션은 각각 switch 문의 case, default 절과 비슷한 역할을 합니다.

#### Greeting.jsp
```
<%@page contentType="text/html; charset=utf-8"%>
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<html>
	<body>
		<c:choose>
			<c:when test="${param.NUM == 0}">
				처음 뵙겠습니다. <br>
			</c:when>
			<c:when test="${param.NUM == 1}">
				반갑습니다. <br>
			</c:when>
			<c:otherwise>
				안녕하세요. <br>
			</c:otherwise>
		</c:choose>
	</body>
</html>
```

#### <c:forEach> 커스텀 액션 사용방법
- \<c:forEach\> 커스텀 액션은 자바 프로그램의 for 문에 해당하는 기능을 제공하는 커스텀 액션입니다.
- 즉, 이 액션을 이용하면 특정 HTML 코드를 일정 횟수만큼 반복해서 출력할 수 있습니다.

```
<c:forEach begin="1" end="10">
	야호<br>   <!-- 반복 출력할 명령문 -->
</c:forEach>
```
- begin : 시작 값
- end : 끝 값

```
<c:forEach var="cnt" begin="1" end="10">
	${cnt} <br>
</c:forEach>
```
- cnt : 카운터 변수

```
<c:forEach var="cnt" begin="1" end="10" step="2">
	${cnt} <br>
</c:forEach>
```
- step : 증가치

```
<c:forEach var="str" items="${arr}">
	${str} <br>
</c:forEach>
```
- str : 배열의 각 항목을 저장할 변수
- arr : 배열 이름 

#### \<c:forEach\>액션의 items 애트리뷰트를 이용해서 처리할 수 있는 데이터 
- 배열
- java.util.Collection 객체
- java.util.Iterator 객체
- java.util.Enumeration 객체
- java.util.Map 객체
- 콤마(,)로 구분된 항목들을 포함한 문자열

#### <c:redirect> 커스텀 액션 사용 방법
- JSP 페이지를 호출하는 \<jsp:forward\> 표준 액션에 대해서 배웠습니다. 이 표준 액션은 RequestDispatcher 인터페이스의 forward 메서드와 동일한 방법으로 JSP 페이지를 호출합니다.
- \<c:redirect\> 커스텀 액션도 이와 비슷한 일을 합니다. 하지만 이 메서드는 forward 메서드가 아니라 sendRedirect 메서드와 동일한 방법으로 작동합니다. 
- 그렇기 때문에 이 액션을 이용하면 JSP 페이지가 아닌 웹 자원과 다른 웹 서버에 있는 웹 자원도 호출할 수 있습니다.

```
<c:redirect url="http://www.yonggyo.com:3001" />
```

- 때로는 다른 웹 자원을 호출하면서 데이터를 넘겨주어야 할 경우도 있습니다. 자바 코드에서는 이런 일을 하기 위해 해당 데이터를 쿼리 스트링 형태로 만들어서 URL 뒤에 덧붙여야 하지만 <c:redirect> 커스텀 액션을 이용하면 훨씬 더 간편하고 구조적인 방법으로 이런 일을 할 수 있습니다. 
- 다음과 같이 <c:redirect>의 시작 태그와 끝 태그 사이에 <c:param>이라는 커스텀 액션을 쓰고 name과 value라는 애트리뷰트를 이용해서 데이터 이름과 데이터 값을 각각 지정하면 됩니다.

```
<c:redirect url="Buy.jsp">
	<c:param name="code" value="75458" />
	<c:param name="num" value="2" />
</c:redirect>
```
- 위 코드가 실행되면 Buy.jsp?code=75458&num=2

#### Redirect.jsp
```
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<c:redirect url="Multiply.jsp">
	<c:param name="NUM1" value="5" />
	<c:param name="NUM2" value="25" />
</c:redirect>
```

#### <c:import> 커스텀 액션 사용 방법
- \<c:import\> 커스텀 액션은 8장에서 배웠던 \<jsp:include\> 표준 액션과 비슷한 일을 합니다. 다시 말해서 현재의 JSP 페이지에 다른 페이지의 결과를 포함시키는 일을 합니다.
- 하지만 다른 점은 이 액션을 이용하면 다른 웹 서버에 있는 JSP 페이지도 불러올 수 있고, JSP 페이지가 이닌 다른 종류의 웹 자원도 불러올 수 있다는 점입니다.

```
<c:import url="Add.jsp" />
```
- url : 호출할 웹 자원의 URL 

- 이 액션을 이용해서 호출하는 웹 자원에 데이터를 넘겨주어야 할 경우 시작 태그와 끝 태그 사이에 다음과 같이 \<c:param\> 커스텀 액션을 쓰면 됩니다. 
```
<c:import url="adScrap.jsp">
	<c:param name="product" value="TV" />
	<c:param name="ad_index" value="007" />
</c:import>
```

#### <c:url> 커스텀 액션 사용 방법
- \<c:url\> 커스텀 액션은 앞에서 배웠던 \<c:set\> 커스텀 액션과 마찬가지로 변수의 선언에 사용되는 액션이지만, URL을 저장하기 위한 변수의 선언에 사용됩니다.
- 그래서 이 액션에서는 URL을 쉽게 다룰 수 있는 방법도 제공하고 있습니다.

```
<c:url var="myUrl" value="Add.jsp" />
```

- URL 뒤에 때로는 스트링 형태로 데이터를 덧붙여야 할 필요가 있습니다. 그럴 때는 이 액션의 시작 태그와 끝 태그 사이에 \<c:param\> 커스텀 액션을 쓰고, name과 value 애트리뷰트 값으로 각각 데이터 이름과 데이터 값을 지정하면 됩니다.

```
<c:url var="myUrl" value="Add.jsp">
	<c:param name="NUM1" value="999" />
	<c:param name="NUM2" value="1" />
</c:url>
```
- Add.jsp?NUM1=999&NUM2=1

- \<c:url\> 커스텀 액션은 바로 앞에서 배운 \<c:redirect\>, \<c:import\> 커스텀 액션과 함께 유용하게 사용될 수도 있습니다.
#### NewRedirect.jsp
```
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<c:url var="next" value="Divide.jsp">
	<c:param name="NUM1" value="100" />
	<c:param name="NUM2" value="25" />
</c:url>
<c:redirect url="${next}" />
```


### ex01_jstl.jsp

JSTL을 사용하려면 헤더영역에 반드시 등록이 되어 있어야 한다.<br>
uri는 jstl라이브러리에서 지원되는 기능을 태그별로 구분해주는 구분자이다. <br>
- 문서 : https://docs.oracle.com/javaee/5/jstl/1.1/docs/tlddocs/c/tld-summary.html

```jsp
<%@page import="java.util.Date"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">

<!-- JSTL(JSP Standard Tag Library) 사용하려면... -->
<!-- c(Core) : if, choose, forEach와 같은 제어문을 사용할 수 있도록 해주는 라이브러리 -->
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>

<!-- fmt(format) : 출력형식(날짜, 숫자) -->
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>

<% 	int num = 10;
	request.setAttribute("n",num);
	
	DB에서 값이 넘어왔다고 가정하자
	List<String> array = new ArrayList<>();
	array.add("서울");
	array.add("대전");
	array.add("대구");
	
	request.setAttribute("array",array);
%>


<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
prefix에 c라고 접근했기 때문에 c라고 적는 것/ html안에서 사용가능한 조건식 모양
	<c:if test="${ n eq 10 }"> 
		<c:out value="참"/> -> 출력문인데 이거 잘 안씀
		참 -> 그냥 이렇게 쓰면됨
	</c:if> -> if – else 문이 없다.

	<hr>
	<!-- choose 영역 내부에는 주석을 달지 말 것 오류남!!! -->
	<c:choose> 여러 가지 조건을 비교할 때는 choose가 더 효율적
		<c:when test="${ param.msg eq 10 }">나는 10이야.</c:when>
		<c:when test="${ param.msg eq 11 }">나는 11이야.</c:when>
		<c:otherwise>모두 아니야</c:otherwise>
	</c:choose>
	<hr>

<!-- var : 값을 담을 변수
     begin : 시작값
     end : 끝값
     step : 증가값 -->
		 
	<c:forEach var="i" begin="1" end="5" step="1"> 
	var 정보는 scope영역에 등록을 안해도 el표기법으로 출력할 수 있다.
	
		<c:if test="${i mod 2 eq 1 }"><!-- i % 2 == 1 -->
		
			<font color="red" >안녕 (${ i })</font><br>
		</c:if>
	</c:forEach>

	이제 우리는 list에 담겨있는 값들을 for each를 통해서 결과를 출력해야 하는 상황이 됐다.
	db에서 정보를 가져왔다고 가정하면 db의 정보는 전부 list에 들어가 있을 테니 그 정보를 가지고
	스크립트릿 출력코드 없이 for each를 통해서 화면에 출력을 해봐야겠죠?

	<hr>
	
	원하는만큼 반복하는거하고, 전체를 알아서 돌리는거하고 코드가 다르다.
	
	<!-- for(String s : array) -->
	<!-- cnt.count : 순번
	     cnt.index : 0번부터 시작하는 index번호 -->
	<c:forEach var="s" items="${array}" varStatus="cnt">
		${cnt.count} / ${s} ------${cnt.index} / ${s} <br>
		 순번 출력됨 1,2,3		add된 방 번호 0,1,2 if문으로 제어하는게 가능
	</c:forEach>
</body>
</html>
```

#### UserVO.java 클래스 만들기

```java
public class UserVO {
	private int idx;
	private String name;
	
	public int getIdx() {
		return idx;
	}
	public void setIdx(int idx) {
		this.idx = idx;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	
	
}
```

#### ex01_jstl.jsp 코드 추가하기

```
...
	UserVO.jsp파일에서 작성 후 객체 생성
	UserVO u1 = new UserVO();
	u1.setIdx(1);
	u1.setName("홍길동");
	
	UserVO u2 = new UserVO();
	u2.setIdx(2);
	u2.setName("박길동");
	
	List<UserVO> uList = new ArrayList<>();

	request.setAttribute("list",uList);
%>


<%
	//fmt라이브러리 관련
	int money = 120000000;
	Date today = new Date();
	
	request.setAttribute("money", money);
	request.setAttribute("today",today);

%>

 ....
 
	<c:forEach var="u" items="${list}">
		${u.idx} / ${u.name} <br>
	</c:forEach>
	
	<hr>
	fmt라이브러리 관련<br>
	<fmt:formatNumber>여기다 값 넣으면 오류남</fmt:formatNumber>
	&#8361;<fmt:formatNumber value="${ money }"/>원 숫자를 세글자 단위로 쉼표를 찍어줌
	<fmt:formatDate value="${ today }"/><br>
	<fmt:formatDate value="${ today }" pattern="yyyy년MM월dd일"/><br>
	dd : 1월 1일부터 오늘까지 경과된 일 수
	DD: 오늘 일

```


