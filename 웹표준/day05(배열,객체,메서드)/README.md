## Array(배열)
```
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
```
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
 함수,메서드 -> function 키워드로 작성 
```
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

함수는 인수를 여러 개 받을 수 있음
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

### 함수 선언문의 끌어올림
- 자바스크립트 엔진은 변수 선언문과 마찬가지로 함수 선언문을 프로그램의 첫머리로 끌어올림
- 따라서 함수 선언문은 프로그램 어떤 위치에서도 작성할 수 있다.

```javascript
alert(square(5)); // 25 
function square(x) {  return x * x; }
``` 
#### 이제 body영역에서 작성한 내용을 Script영역까지 가져와보자.
- a 태그는 클릭해서 이동하는것은 가능하지만 내용을 가져오는데는 한계가 있다.
- 자바스크립트와 밀접한 관련이 있는 input 태그는 사용자가 데이터를 입력할 수 있는 입력필드를 정의할 때 사용합니다.
- input태그의 type속성을 달리함으로써 여러 가지 모양을 나타낼 수 있습니다.
  
|속성명|속성값|설명|
|------|---|---|
|type|button<br>chcekbox<br>color<br>date<br>datetime-local<br>email<br>file<br>hidden<br>image<br>month<br>number<br>password<br>radio<br>range<br>reset<br>search<br>submit<br>tel<br>text<br>time<br>url<br>week<br>|input 요소가 나타낼 타입을 명시함.|

```
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
  
  
