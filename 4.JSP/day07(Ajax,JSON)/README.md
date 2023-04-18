# Ajax
- Asynchronous JavaScript and XML
- 자바스크립트를 이용한 백그라운드 통신 기술( 비동기 통신)
    - Ajax를 이용하면 웹 페이지 전체를 다시 로딩하지 않고도, 웹 페이지의 일부분만을 갱신할 수 있습니다.
    - 이때 서버와는 다음과 같은 다양한 형태의 데이터를 주고받을 수 있습니다.
          - JSON
          - XML
          - HTML
          - 텍스트 파일 등

## Ajax를 사용하기 위한 준비

![image](https://user-images.githubusercontent.com/54658614/232671748-aa2f9a6d-e529-4acd-8bf3-7423f771d092.png)

httpRequest.js 복사해서 넣기(인터넷에 코드 다 있기는 하지만 매번 복사해서 쓰기 귀찮으니 파일로 만들어놓고 쓰자.

![image](https://user-images.githubusercontent.com/54658614/232671809-1feb615a-950d-47c3-b6e6-a1f931b68d4a.png)

## httpRequest.js의 구조
Ajax의 가장 핵심적인 구성 요소는 바로 XMLHttpRequest객체입니다.<br>
Ajax에서 XMLHttpRequest 객체는 웹 브라우저가 서버와 데이터를 교환할 때 사용됩니다.<br>
웹 브라우저가 백그라운드에서 계속해서 서버와 통신할 수 있는 것은 바로 이 객체를 사용하기 때문입니다.<br>

```js
var xhr = null; //그냥 변수

function createRequest(){
		
//JavaScript를 이용하여 서버로 정보를 보내는 HTTP request를 만들기 위해서는 이런 기능을 제공하는 클래스 인스턴스가 필요하다. 
//이런 클래스는 InternetExplorer에서는 XMLHTTP라고 불리는 ActivX object를 말한다. 
//그러면 Mozzlia, Safari나 다른 브라우저에서도 Microsoft의 ActiveX 객체의 method와 property를 지원하기 위해 XMLHttpRequest클래스를 구현 하고 있다.

	if(xhr!=null)return;
	if(window.ActiveXObject) //비동기식 통신 방식인 XMLHttp는 가장 처음으로 익스플로러 5 버전에서 ActiveXObject라는 객체를 사용하여 구현됩니다.
		xhr = new ActiveXObject("Microsoft.XMLHTTP");
	else
		xhr = new XMLHttpRequest(); //모질라와 사파리에서 XMLHttpRequest라는 이름으로 도입하여 널리 사용되기 시작합니다.
}


function sendRequest(url, param, callBack, method){
	createRequest();//HTTP request생성

	//전송타입 구분
	var httpMethod = 
	(method!='POST' && method!='post')?'GET':'POST';
	
	//파라미터 구분
	var httpParam = 
	(param==null || param == '')?null:param;
	
	//접근 url
	var httpURL = url;
	
	//요청 방식이 get방식이고, 전달할 파라미터 값이 있다면
	//url경로를 제작 해야 한다.(.../test.jsp?ch=123)
	if(httpMethod == 'GET' && httpParam != null)
		httpURL = httpURL+"?"+httpParam;
	
  //xhr.open( 요청방식, 접근url, 비동기(true면 비동기) ); 
	xhr.open(httpMethod, httpURL, true);

	//만약 "POST" type을 보내려 한다면, 요청(request)에 MINE type을 설정 해야 한다.
  //예를 들자면 send()를 호출 하기 전에 아래와 같은 형태로 send()로 보낼 쿼리를 이용해야 한다.
	xhr.setRequestHeader("Content-Type",
	  "application/x-www-form-urlencoded");
    
//application/x-www-form-urlencoded는 html의 form의 기본 Content-Type이다.
//application/x-www-form-urlencoded는 key=value&key=value의 형태로 전달된다는 점입니다.

	//작업이 완료된 후 호출될 콜백메서드 지정
	xhr.onreadystatechange = callBack;
	
	xhr.send(httpMethod == 'POST'?httpParam:null);
}
```


### gugudan.jsp 파일 생성
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
    
<!-- Ajax (Asynchronous JavaScript and XML)
자바스크립트를 이용한 백그라운드 통신 기술( 비동기 통신 ) -->
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>Insert title here</title>
		<!-- js폴더에 들어있는 httpRequest.js파일을 사용할 준비 -->
		<script src="js/httpRequest.js"></script>
		<script type="text/javascript">
			function send(){
				var dan = document.getElementById("dan").value;
				
				var url = "gugudan_ajax.jsp"; //목적지
				var param = "dan="+dan;
				
				//httpRequest.js 안에 정의되어 있는 메서드
				sendRequest(url, param, resultFn, "GET");
			}
			
			function resultFn(){ //콜백함수
				alert("call back")
			}
		</script>
	</head>
	<body>
		단 : <input id="dan">
			<input type="button" value="결과보기" onclick="send()">
			<br>
			<div id="disp"></div>
	</body>
</html>
```

### gugudan_ajax.jsp 클래스 생성

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
    
    <%
    
    	int dan = Integer.parseInt(request.getParameter("dan"));
    
    	String str = "";
    	
    	for(int i = 0; i <= 9; i++){
    		str+= dan + " * " + i + " = " + (dan * i) + "<br>";
    	}
    
    바디에 출력하려고 함에도 불구하고 callback함수로 돌아가기 때문에 gugudan.jsp의 콜백함수로 돌아간다.
    %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<%= str %>
</body>
</html>
```
콜백함수를 통해서 계산한 구구단값을 가져와보자

### gugudan.jsp 코드 추가하기

```jsp

Ajax에서 서버로부터의 응답을 확인하기 위해 사용하는 XMLHttpRequest 객체의 프로퍼티는 다음과 같습니다.
 - readyState 프로퍼티
 - status 프로퍼티
 - onreadystatechange 프로퍼티
 
readyState : XMLHttpRequest의 readyState 프로퍼티는 XMLHttpRequest 객체의 상태를 보여줍니다. 
 - UNSENT (숫자 0) : XMLHttpRequest 객체가 생성됨.
 - OPENED (숫자 1) : open() 메소드가 성공적으로 실행됨.
 - HEADERS_RECEIVED (숫자 2) : 모든 요청에 대한 응답이 도착함.
 - LOADING (숫자 3) : 요청한 데이터를 처리 중임.
 - DONE (숫자 4) : 요청한 데이터의 처리가 완료되어 응답할 준비가 완료됨.

status : 서버의 문서 상태를 나타냅니다.
 - 200 : 서버에 문서가 존재함.
 - 404 : 서버에 문서가 존재하지 않음.

function resultFn(){
    console.log( xhr.readyState + " / " + xhr.status);

    if(xhr.readyState == 4 && xhr.status == 200){
    //도착한 데이터를 읽어오기
    var data = xhr.responseText; //최종결과값을 갖고 돌아온다.
    //alert(data);
    //중복되는 태그들은 innerHTML이 알아서 제거해준다.
    document.getElementById("disp").innerHTML = data;
    }

```

# JSON 표기법
- JavaScript Object Notation 표기법
- 서로 다른 프로그램에서 데이터를 교환하기 편하도록 규격화된 표기법

![image](https://user-images.githubusercontent.com/54658614/232677104-eb590990-9ccd-455a-9d6b-bbe0b389950a.png)

C언어로 물어봐도, IOS언어로 물어봐도, ANDROID언어로 물어봐도 똑같은 결과로 돌려줍니다.

## 문법
```
{'데이터이름' : '값'}
```
- 값에 넣을 수 있는 데이터
    - 숫자 : 정수,실수,지수  -> JSON에서는 8진수나 16진수 등을 표현하는 방법은 제공하지 않습니다.
    - 문자열
    - 논리형 : true,false(항상 소문자료 표기해야 한다)
    - 객체가 들어갈 수 있다.(계층구조가 형성된다.)
    - 배열 : JSON배열은 대괄호 ([])로 둘러싸여 있습니다.
    - null : JSON에서 null은 반드시 소문자로 표기해서 사용해야 한다.


### ex03_json.jsp

```JSP
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>Insert title here</title>
		<script type="text/javascript">
			var p = {'name':'홍길동', 'age':'20', 'tel':'010-1111-1111'};
			document.write("이름 : " + p.name + "<br>");
			document.write("나이 : " + p.age + "<br>");
			document.write("전화 : " + p.tel + "<br>");
			
			document.write("<hr>");
			
			 //json 1차원 배열
			var p_arr = [{'name':'일길동', 'age':'30'},{'name':'이길동','age':'40'}];
			
			document.write(p_arr[0].name + " / " + p_arr[0].age +"<br>");
			document.write(p_arr[1].name + " / " + p_arr[1].age +"<br>");
			
			//json 다차원 배열
			var course = {'name':'웹앱개발',
						  'start' : '2023-02-13',
						  'end' : '2023-07-07',
						  'sub':['java','jsp','spring'],
						  'student':[{'name':'홍길동','age':'20'},{'name':'김길동','age':'22'}]}
			document.write("<hr>");
			document.write("과목명 : "+course.name+"<br>");
			document.write("시작일 : " + course.start+"<br>");
			document.write("종료일 : " + course.end+"<br>");
			//document.write("과목 : " + course.sub+"<br>"); 같은 결과가 나옴
			document.write("과목 : " + course.sub[0]+", "+course.sub[1]+", "+course.sub[2]+"<br>");
			document.write("학생들 : " + course.student[0].name+"/"+course.student[0].age+"<br>");
		</script>
	</head>
	<body>
	
	</body>
</html>
```
결과

![image](https://user-images.githubusercontent.com/54658614/232681406-5c768df8-21fa-460e-a2c7-c13b58a954db.png)











