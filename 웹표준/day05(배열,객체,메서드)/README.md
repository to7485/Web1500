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
  
  
  
  
  
  
  
