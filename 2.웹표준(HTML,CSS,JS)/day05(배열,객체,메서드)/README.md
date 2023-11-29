## Array(배열)
```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>배열</title>
		<script type="text/javascript"> /*type="text/javascript" 는 이전버전과의 호환 때문에 있는 코드, 없어도 상관 없다.*/
			var arr = new Array(3); /* 배열을 만드는 순간에는 어떤 자료형의 데이터를 담을지 알 수 없다. */
			arr[0] = 10;
			arr[1] = 20;
			arr[2] = "안녕" /* 자바에서는 상상도 할 수 없는 내용 */
			arr[3] = 3.14; /* 방이 모자라면 더 만들어버림 */
			
			/* 자바에서의 배열의 생성 및 초기화
			int[]arr = {1,2,3}*/
			
			/* 자바스크립트에서의 배열의 생성 및 초기화
			var arr = [10,20,'안녕',3.14]*/
			
			for(var i = 0; i<arr.length; i++) {
				//document.write(arr[i] +"<br>");
				document.write(`${arr[i]}<br>`);
			}
		</script>
		
	</head>
	
	<body>
	
	</body>
</html>
```
## 객체
- 객체(Object) : 사물
  - 속성,기능으로 이루어져있다.
  
### 객체 리터럴
- 자바에서는 클래스를 만들고 클래스를 통해서 객체를 만들었지만 자바스크립트에서는 변수처럼 정의가 가능하다.
- 자바스트립트의 객체는 키(key)와 값(value)으로 구성된 프로퍼티(Property)들의 집합이다.
- 프로퍼티 : 변수와 거의 비슷한 속성
  - 이름 짓는 규칙이 비슷하다.
  
- 값 : 원시타입(숫자,문자,null,undifined, false, true),객체(객체의 값으로 객체가 들어올 수 있다.)
```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>Insert title here</title>
		<script type="text/javascript">
			var person = { name : "홍길동", age: 30};
		      //객체를 출력하는법
			document.write('person.name : ',person.name+"<br>");
			document.write('person.age : ',person.age+"<br>");
			document.write("<hr>");
      
     			document.write("person['name'] : ",person['name'],"<br>");
			document.write("person['age'] : ",person['age']+"<br>");
			document.write("<hr>");
			
			//없는 속성에 값만 넣으면 객체에 추가가 된다.
			person.bt = 'AB';
			document.write("person['bt'] : ",person['bt'],"<br>");
			document.write("<hr>");
			
			//객체에서 속성 삭제하기
			delete person.rank2;
			document.write('bt : ' + person['bt']+"<br>");
			document.write("<hr>");
			
			//속성이 객체에 속해있는지 확인하는법 : in -> 결과는 true , false로 반환
			document.write('name in person : '+('name' in person));
			document.write("<hr>");
			
			for(i in person){
				document.write(`${i} = ${person[i]}<br>`);
			}
			document.write("<hr>");
			//새로운 객체에 기존 객체를 참조 시킬수 있다. 주소값을 복사해오기 때문에 person의 속성값을 변경하면 같이 바뀐다.
			var person2 = person;
			
			person.age = 40;
			
			document.write('person2.age : ',person2.age); //결과 40
			
		</script>
	</head>
	<body>
	
	</body>
</html>
```
- 객체 안에 객체나 함수가 들어갈수도 있다.
```html
  <!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>Insert title here</title>
		<script type="text/javascript">
    
      //객체 안에 객체가 들어갈 수 있다.
      var circle = {
          center : {x : 1.0, y: 2.0},
	radius : 2.5
			}
      document.write("<hr>");

      document.write('circle.center.x : ',circle.center.x,' circle.center.y : ',circle.center.y+"<br>");
      document.write('circle.radius : ',circle.radius+"<br>");
      document.write("<hr>");
		
		
	//객체 안에 함수가 들어갈 수 있다.
	var person = {
			name : "홍길동",
			age : 30,
			getInfo : function(){
				document.write('이름 : ',this.name,"<br>",'나이 : ',this.age,"<br>");
			}
	};
			
			
	//함수의 호출
	person.getInfo();
		</script>
	</head>
	<body>
	
	</body>
</html>
  
```
 ## 함수
- 일련의 처리를 하나로 모아 언제든 호출할 수 있도록 만들어 놓은것
- 수학 함수와 비슷
- 함수의 입력 값을 인수, 함수의 출력 값을 반환값
```javascript 
function 함수명 (인수) {   
	처리 로직 
 
   return 출력 반환값
}
```
### 함수 선언문으로 함수 정의하기
- 함수는 function 키워드를 사용해서 정의
```javascript
function square(x) {    
	var result = x * x; 
	
	return result; 
}  
``` 
- square - 함수 이름
- x - 인수
- var result = x \* x - 처리 로직 
- return result - 처리 후 반환값 
- 참조) 함수명 캐멀 표기법

### 함수 호출
함수를 호출하려면 함수 이름 뒤에 소괄호 인수를 묶어 입력
```javascript
square(3); // 9
```
### 인수
- 함수는 인수를 여러 개 받을 수 있음
- 인수가 여러 개라면 인수와 인수를 쉼표(,)로 구분
```javascript
function add(a, b) {   
	var c = a + b;
	return c;
}
```
- 인수를 받지 않는 함수도 정의할 수 있음
```javascript
function bark() {    
	alert("멍멍"); 
};
bark(); // 멍멍 
alert(bark()); // undefined - 반환값이 없으므로
```

### 함수의 실행흐름
- 호출된 코드에 있는 인수가 함수 정의문에 대입된다. 
- 함수 정의문의 중괄호 안에 작성된 플그램이 순차적으로 실행된다.
- return 문이 실행되면 호출된 코드로 돌아간다. return 문의 값은 함수의 반환값이 된다.
- return 문이 실행되지 않은 상태로 마지막 문장이 실행되면, 호출한 코드로 돌아간 후에 
- undefined가 함수의 반환값이 된다.

### 함수 선언문의 호이스팅
- 자바스크립트 엔진은 변수 선언문과 마찬가지로 함수 선언문을 프로그램의 첫머리로 끌어올림
- 따라서 함수 선언문은 프로그램 어떤 위치에서도 작성할 수 있다.

```javascript
alert(square(5)); // 25 
function square(x) {  return x * x; }
``` 


## HTML 요소의 선택

|속성명|속성값|설명|
|------|---|---|
|type|button<br>chcekbox<br>color<br>date<br>datetime-local<br>email<br>file<br>hidden<br>image<br>month<br>number<br>password<br>radio<br>range<br>reset<br>search<br>submit<br>tel<br>text<br>time<br>url<br>week<br>|input 요소가 나타낼 타입을 명시함.|

- 자바스크립트로 HTML 요소를 제어하려면 그 전에 제어하고자 하는 요소 객체를 먼저 가져와야 합니다.
- 물론 Document 객체의 DOM 트리를 타고 올라가 요소 객체를 가져오는 방법도 있지만 Document 객체는 이보다 편리하게 요소 객체를 가져올 수 있는 메서드가 마련되어 있습니다.


|메서드|설명|
|------|---|
|document.getElementsByTagName(태그이름)|해당 태그 이름의 요소를 모두 선택함.|
|document.getElementById(아이디)|해당 아이디의 요소를 선택함.|
|document.getElementsByClassName(클래스이름)|해당 클래스에 속한 요소를 모두 선택함.|
|document.getElementsByName(name속성값)|해당 name 속성값을 가지는 요소를 모두 선택함.|
|document.querySelectorAll(선택자)|해당 선택자로 선택되는 요소를 모두 선택함.|

### id 속성으로 노드 가져오기
- HTML 문서의 요소에는 id 속성을 지정할 수 있습니다.
- id 속성 값은 문서에서 유일한 값이어야 합니다. 따라서 id 속성 값으로 요소 하나를 가리킬 수 있습니다.

```javascript
document.getElementById(id 값);
```

### 태그이름으로 노드 가져오기
- 인수로 넘긴 문자열과 같은 이름을 가진 태그 목록을 가져올 수 있습니다. 인수로는 태그 이름을 넘깁니다.
- 태그 이름은 대소문자를 구별하지 않습니다.
- getElementsByTagName 메서드는 반환값이 복수개의 요소 입니다. 이것은 일반적인 HTML 문서에는 이름이 같은 태그가 많이 사용되기 때문입니다. 
- getElementsByTagName 메서드는 NodeList 객체를 반환합니다. NodeList 객체는 유사 배열 객체이며 읽기 전용입니다. 
- 요소 이름 대신 와일드카드(\*)를 지정할 수도 있습니다. 이 경우에는 NodeList에 HTML 문서 안의 모든 요소를 담아서 반환합니다.

```javascript
document.getElementsByTagName(요소의 태그 이름)[index];
```

### class 속성 값으로 노드 가져오기
- class 속성 값은 0개 이상의 식별자(클래스 이름)를 CSS에서 사용하는 공백 문자(공백, 탭 등)로 연결한 문자열로 표기합니다.
- 이 class 속성으로 class 속성 값을 갖는 요소의 집합이 정의됩니다.
- 즉, 똑같은 식별자 class 속성 값으로 포함된 요소들이 모인 집합 하나가 정의됩니다.
- getElementsByClassName 메서드를 사용하면 특정 class  속성 값을 class 속성 값으로 가지는 요소 객체 목록(NodeList)을 가져올 수 있습니다.

```javascript
document.getElementsByClassName(class의 이름)[index];
```

### CSS선택자로 노드 가져오기
```js
document.querySelector("#태그아이디값");
document.querySelector(".태그클래스값");

```

## DOM요소 속성 조정
|메서드|설명|
|------|---|
|hasAttribute(속성)|지정한 속성을 가지고 있는지 검사한다.|
|getAttribute(속성)|속성의 값을 가져온다.|
|setAttribute(속성명,값)|속성과 속성값을 설정한다.|
|removeAttribute(속성)|지정한 속성을 제거한다.|
|hasAttribute(속성)|지정한 속성을 가지고 있는지 검사한다.|
|.attributes|속성들을 모아서 배열로 반환|

### js_dom.html 생성하기
```html
<!DOCTYPE html>
<html>
  <body>
  <input type="text">
  <script>
     const input = document.querySelector('input[type=text]');
     console.log(input);

     if (!input.hasAttribute('value')) {  // value 어트리뷰트가 존재하지 않으면
       // value 어트리뷰트를 추가하고 값으로 'hello!'를 설정
       input.setAttribute('value', 'hello!');
     }

     // value 어트리뷰트 값을 취득
     console.log(input.getAttribute('value')); // hello!

     // value 어트리뷰트를 제거
     input.removeAttribute('value');

  </script>
  </body>
</html>
```

## DOM 객체의 스타일 변경하기
- element.style 객체로 태그의 css를 변경할수도 있다.
1. 요소의 배경색 변경
```js
var element = document.getElementById('myElement');
element.style.backgroundColor = 'lightblue';
```
2. 폰트 색상 변경
```js
var element = document.getElementById('myElement');
element.style.color = 'red';
```
3. 폰트 크기 변경
```js
var element = document.getElementById('myElement');
element.style.fontSize = '20px';
```
4. 테두리 추가:
```js
var element = document.getElementById('myElement');
element.style.border = '2px solid black';
```
5. 요소의 가시성 변경
```js
var element = document.getElementById('myElement');
element.style.display = 'none'; // 숨기기
// 또는
element.style.display = 'block'; // 보이기
```
6. 요소의 위치 변경
```js
var element = document.getElementById('myElement');
element.style.position = 'relative';
element.style.left = '50px';
element.style.top = '20px';
```
7. 클래스의 추가/제거
```js
var element = document.getElementById('myElement');
element.classList.add('newClass'); // 클래스 추가
// 또는
element.classList.remove('oldClass'); // 클래스 제거
```
8. 다중 속성 변경
```js
var element = document.getElementById('myElement');
element.style.cssText = 'color: blue; font-size: 18px;'; // 여러 속성을 동시에 변경
```
### js_
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>

</head>
<body>
	<p id="p_id"> 자바스크립트로 css 바꾸기 연습</p>
	
	<script type="text/javascript">
		var element = document.getElementById("p_id"); 
		element.style.backgroundColor='lightblue';
		element.style.color='red';
		element.style.fontSize = '30px';
		element.style.border='2px solid black';
		element.classList.add('newClass');
		console.log(element.getAttribute('class'))

</script>
</body>
</html>
```

## JavaScript의 이벤트 처리
1. HTML 속성을 이용한 이벤트 처리
- 이벤트를 HTML요소에 직접 추가하는 방식이다.
- 예를들어 'onclick' 속성을 사용하여 클릭 이벤트를 처리할 수 있다.
```html
<button onclick="myFunction()">클릭</button>

<script>
  function myFunction() {
    alert("버튼이 클릭되었습니다!");
  }
</script>

```
2. DOM 요소에 이벤트 리스너 추가
- JavaScript를 사용하여 DOM 요소에 이벤트 리스너를 동적으로 추가하는 방식이다.
- addEventListener 함수를 사용한다.
```html
<button id="myButton">클릭</button>

<script>
  // 요소를 선택하고 이벤트 리스너를 추가
  document.getElementById("myButton").addEventListener("click", function() {
    alert("버튼이 클릭되었습니다!");
  });
</script>
```

## 이제 body영역에서 작성한 내용을 Script영역까지 가져와보자.
- a 태그는 클릭해서 이동하는것은 가능하지만 내용을 가져오는데는 한계가 있다.
- 자바스크립트와 밀접한 관련이 있는 input 태그는 사용자가 데이터를 입력할 수 있는 입력필드를 정의할 때 사용합니다.
- input태그의 type속성을 달리함으로써 여러 가지 모양을 나타낼 수 있습니다.

#### js_function.html 생성하기
```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>자바스크립트의 메서드(함수)</title>
		<script type="text/javascript">
			function fun01(){
				alert("메서드 정상 호출됨");
			}
			
			function fun02(n) {//파라미터에 자료형을 적어줄 필요가 없다.
				//script영역에서 body로 접근하여 특정 객체를 검색
				var res = document.getElementById("result");
				
				res.value = n; //입력상자의 값이 홍길동으로 바뀐다.
			}
			
			function fun03() {
				//name이라는 아이디를 가진 태그를 찾아서
				//해당 태그에 쓰여있는 value를 alert()으로 출력하기
				var res = document.getElementById("name");
				var value = res.value;
				alert(value);	
				
			}
			
			function fun04(){
				var res = document.getElementById("res").value;
				var m_div = document.getElementById("m_div");
				m_div.innerHTML = res;
				//document.write();를 쓰게 되면 페이지가 아예 새로써진다.
			}
		</script>
	</head>
	
	<body>
		<input type="button" value="버튼1" onclick="fun01();"><!-- onclick : 마우스로 클릭했을 때 실행-->
		<input type="button" value="파라미터 버튼" onclick="fun02('홍길동');"><br><!-- 문자로 보내고 싶을 때는 홑따옴표라도 묶어서 보내야 한다. -->
		
		<hr>
		<input type="text" value="연습" id="name">
		<input type="button" value="버튼3" onclick="fun03();"> <!-- 버튼을 눌렀을 때 입력상자에 쓰여있는 값을 호출해보고싶다.  -->
		
		<hr>
		<input id="res">
		<input type="button" value="버튼3" onclick="fun04();">
		<div id="m_div">
		
		</div>
		
		<hr>
		<input type="password" id="pwd"><br>
		
	</body>
</html>
```
![image](https://user-images.githubusercontent.com/54658614/229685111-1c71bc5c-c716-4de4-ad0e-c35beafb3da6.png)

## 구구단 만들기
```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>Insert title here</title>
		
		<style type="text/css">
			#disp{width:300px; height:300px; background: black; color:white; }
		</style>
		
		<script type="text/javascript">
		
		function gugu() {
			var m_dan = document.getElementById("dan").value;
			
			//유효성체크
			if( m_dan.trim() == '' ){
				alert("값을 입력해야 합니다.")
				return; //더이상 진행하지 않고 끝내라
			}
			
			if( m_dan<2 || m_dan>9 ){
				alert("2~9 만 입력해 주세요")
				return; //더이상 진행하지 않고 끝내라
			}
			
			//구구단 생성
			var str = "";
			
			for(var i = 1; i<10; i++) {
				str = str + m_dan + 'X' + i + '=' + (m_dan * i) + "<br>";
			}
			var gugudan = document.getElementById("disp");
			gugudan.innerHTML = str;
			 
		}//gugu()
		var b_show = true;
		function show() {
			
			b_show = !b_show;
			document.getElementById("bt_show").value = b_show ? "숨기기":"보이기";
			document.getElementById("disp").style.display = b_show ? 'block' : 'none';
		}
		</script>
	</head>
	
	<body>
		<p>
			단:
			<input id="dan">
			<input type="button" value="계산시작" onclick="gugu();">
			<input type="button" id="bt_show"  value="숨기기" onclick="show();">
		</p>
		
		<hr>
		
		<div id="disp">
			여기에 구구단
		</div>
	</body>
</html>
```
![image](https://user-images.githubusercontent.com/54658614/229685861-d2e09053-76fc-44e9-94a6-28ed1ac7dbbc.png)

## 계산기 만들기
```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>Insert title here</title>
		<style type="text/css">
		li{list-style: none;
			float:left;
			margin-left:10px;}
		ul{overflow:hidden;}
		input{border-style:none;}
		</style>
		<script type="text/javascript">
		function cal(n) {
			var num1 = Number(document.getElementById("su1").value);
			var num2 = Number(document.getElementById("su2").value);
			var num3 = document.getElementById("result");
			if(n == '+') {
				num3.value = num1+num2;
			}
			else if(n == '-') {
				num3.value = num1-num2;
			}
			else if(n == '*') {
				num3.value = num1*num2;
			}
			else if(n == '/') {
				num3.value = num1/num2;
			}
			
		}
		
		
		</script>
	</head>
	
	<body>
		<table border = "1">
			<tr>
				<th class="su">수1</th>
				<td><input id="su1" placeholder="정수만 입력하세요"></td>
			</tr>
			
			<tr>
				<th class="su">수2</th>
				<td><input id="su2" placeholder="정수만 입력하세요"></td>
			</tr>
			
			<tr>
				<td colspan="2">
					<ul>
						<li>
							<input type="button" value="+" onclick="cal('+');">
						</li>
						<li>
							<input type="button" value="-" onclick="cal('-');">
						</li>
						<li>
							<input type="button" value="*" onclick="cal('*');">
						</li>
						<li>
							<input type="button" value="/" onclick="cal('/');">
						</li>
					</ul>
				</td>
			</tr>
					<tr>
						<th>결과</th>
						<td><input id="result"></td>
					</tr>
		</table>
	</body>
</html>
```

![image](https://user-images.githubusercontent.com/54658614/229686187-0676b0aa-53ac-4486-90ca-7e4015f3756f.png)


## 갤러리 만들어보기
```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>이미지 슬라이드</title>
		<style type="text/css">
			a{text-decoration:none;}
		</style>
		
		<script type="text/javascript">
			var num = 1;
			var path="image/img";
			
			
			function prev() {
				num--;
				
				if(num<1) {
					num = 7;
				}
				//gallery라는 아이디를 가진 태그를 검색
				var my_a = document.getElementById("gallery"); //my_a가 image1.jpg를 갖고 있는 a태그가 된다.
				my_a.src= path + num + ".jpg";
				
			}//prev
			
			function next() {
				num++;
				
				if(num>7) {
					num = 1;
				}
				var my_b = document.getElementById("gallery");
				my_b.src = path + num + ".jpg";
			}
			
			setInterval("next()",1000); // 1초 간격으로 자동으로 next() 메서드를 호출
		</script>
	</head>
	
	<body>
	
		<div id="galleryWrap">
		
			<p>
				<a href="#" onclick="prev();">
					<img alt="이전" src="image/left_btn.png">
				</a>
				
				<img alt="갤려리" id="gallery" src="image/img1.jpg" width="300" height="200">
				
				<a href="#" onclick="next();">
					<img alt="다음" src="image/right_btn.png">
				</a>
			</p>
		
		</div>
		
	</body>
</html>
```
![image](https://user-images.githubusercontent.com/54658614/229685278-97f71f02-dad3-4958-b43a-e4f2bdcab247.png)

## form태그를 통한 값 전달

### HTML 폼
- 폼에 입력한 데이터를 웹 서버로 보내고 웹 서버는 그 데이터를 처리한다. 그 결과를 사용자에게 반환하거나 데이터베이스에 저장한다.
- 클라이언트 측 자바스크립트로 웹 애플리케이션을 만들 때 사용자 입력을 받는 사용자 인터페이스로 사용한다. 이때 데이터 처리는 클라이언트 측 자바스크립트 프로그램이 담당한다.

### 폼 요소와 폼 컨트롤 요소
- 웹 서버에 데이터를 보낼 때는 다음과 같은 과정을 거칩니다. 우선 form 요소를 작성하고 method와 action 속성을 지정합니다.
	- <b>method 속성</b> : 데이터 전송 방법("POST" 또는 "GET")
	- <b>action 속성</b> : 데이터를 처리하는 CGI 프로그램의 URL
- 그 다음 form 요소 안에 사용자로부터 입력을 받는 input 요소 등의 폼 컨트롤 요소를 배치합니다.
- 마지막으로 form 요소 안에 데이터를 전송하기 위한 submit 버튼과 데이터 입력을 취소하는 reset 버튼을 배치합니다. 

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>form 태그를 통한 데이터 전달1</title>
		<script type="text/javascript">
		function check(){
			//forms : 현재 html문서내의 form들을 순차적으로 배열로 정의해둔 형태
			//body에서 form태그가 여러개 존재할 경우 foms배열에 0,1,2,등의 인덱스로 form태그를 구별한다.
			var f = document.forms[0]; 
			
			//form태그에서 m_id라는  name속성을 가지고 있는 입력상자
			//document객체가 필요하지 않다 정말 편하다.
			var myId = f.m_id.value;
			
			//아이디를 입력하지 않았을 때 경고를 띄워주는 유효성 검사
			//클라이언트 단에서 해주는것이 좋다.
			if(myId == '') {
				alert("아이디는 필수 사항입니다.");
				return;
			}
			
			var myPwd = f.m_pwd.value;
			if(myPwd == ''){
				alert("비밀번호는 필수 사항입니다.");
				return;
			}
			
			var myAge = f.m_age.value;
			if(myAge == ''){
				alert("나이는 필수 사항입니다.");
				return;
			}
			
			//form태그에서 서버로 값을 전달하고자 한다면 알아야 하는 것
			
			//전송방식(GET,POST)
			//GET: 파라미터가 URL에 노출, 속도가 빠르지만 보안성이 취약
			//POST: 파라미터가 URL에 노출되지 않는다. 속도가 빠르지 않지만 보안성이 높고 바이너리 타입의 데이터를 전달하는것이 가능하다
			f.method="get";
			
			//파라미터를 전달할 페이지(처리객체 지정)
			f.action = "login.jsp";
			

			
			//입력된 데이터를 전송
			//f가 가진 name속성이 action으로 지정해둔 페이지에 파라미터로 전달(주소창을 보면 login.jsp?m_id=aaa&m_pwd=1234로 ? 뒤에 파라미터로 전달이 된다.)
			f.submit();

		}
		</script>
	</head>
	
	<body>
		서버로 전달하고 싶은 모든 데이터는 form 태그 안에 있어야 한다.
		그렇지 않으면 다른 jsp,java클래스로 전달할수 없다. 매우 중요!
		<!--<form></form>-->
		<form>
			<!--<form>태그의 공식적인 자식태그의 형태는 거의 <input> 태그형태로 이루어져 있다고 생각하면된다.
			<TextArea>는 name 속성을 붙힐 수 있다. 그 외 태그들 <div><a>이런 태그들에는 name을 붙혀도 파라미터로 넘어오지 않는다.-->
			
			<table border = "1"> 
				<tr>
					<th>ID : </th>
					<!--name 속성은 form안에서 특정 input을 찾아낼 수 있도록 해주는 식별자
					id와 역할은 비슷하지만 id로 지정을 해놓으면 다른 페이지로 값을 전달할 수 없다.-->
					
					<!--placeholder : 입력창에 가이드라인을 준다.-->
					
					<td><input name="m_id" placeholder="아이디를 입력해주세요"></td> 
					<!-- login.jsp에게 name으로 설정되 있는 id와 pwd를 파라미터로 써 날아간다. -->
				</tr>
				
				<tr>
					<th>AGE : </th>
					<td><input name="m_age"></td>
				</tr>
				
				<tr>
					<th>PWD : </th>
					<td><input type="password" name="m_pwd"></td>
				</tr>
				
				<tr >
					<td colspan="2" align="center">
						<input type="button" value="전송" onclick="check();">
						<!--type="submit"으로 보내면 무조건 서버로 날리기 때문에 유효성 체크가 안되므로 일반적으로 쓰이지 않는다. -->
					</td>
				</tr>
			</table>
		</form>
	</body>
</html>
```

![image](https://user-images.githubusercontent.com/54658614/229689463-0f9ad6de-7430-41b7-b729-4620a403f99c.png)

### form 태그를 검색하는 방법-2
```html
!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>form 태그를 통한 데이터 전달1</title>
		<script type="text/javascript">
		function check(){
			//form태그에 준 이름을 가지고 바로 검색할 수 있다.
			var f = document.ff;
			
			var myId = f.m_id.value; //유효성 체크를 하기 위해 id의 값을 가져오는것이지 하지 않을거라면 안써도 무방하다
			
	
			//f.method="get";
			
			//f.action = "login.jsp";
			
			f.submit();

		}
		</script>
	</head>
	
	<body>
	<!-- form 태그에 이름을 준다!-->
		<form name="ff" action="login.jsp" method="GET">

			<table border = "1"> 
				<tr>
					<th>ID : </th>
					<td><input name="m_id" placeholder="아이디를 입력해주세요"></td> 

				</tr>
				
				<tr>
					<th>AGE : </th>
					<td><input name="m_age"></td>
				</tr>
				
				<tr>
					<th>PWD : </th>
					<td><input type="password" name="m_pwd"></td>
				</tr>
				
				<tr >
					<td colspan="2" align="center">
						<input type="button" value="전송" onclick="check();">
					</td>
				</tr>
			</table>
		</form>
	</body>
</html>
```

### form 태그를 검색하는 방법-3
```html
		<script type="text/javascript">
		function check(f){
			
			f.submit();

		}
		</script>
	</head>
	
	<body>
		<form action="login.jsp" method="GET">
			<table border = "1">
				<tr >
				<!--this는 form에만 사용할 수 있다. this.form은 버튼을 포장하고 있는 현재 form 태그를 의미한다.-->
					<td colspan="2" align="center">
						<input type="button" value="전송" onclick="check(this.form);">
					</td>
				</tr>
			</table>
		</form>
	</body>
</html>
```
