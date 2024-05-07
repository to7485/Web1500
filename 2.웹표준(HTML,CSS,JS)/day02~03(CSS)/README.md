# CSS
- HTML이나 XML과 같은 구조화된 문서(Document)를 화면, 종이 등에 어떻게 렌더링할 것인지를 정의하기 위한 언어이다.
- 즉 CSS는 HTML의 각 요소(element)의 style(design, layout etc)을 정의 하여 화면(Screen) 등에 어떻게 렌더링하면 되는지 브라우저에게 설명하기 위한 언어이다.
- CSS(Cascading Style Sheet)는 HTML과 같은 마크업 언어로 작성된 문서에 색상, 폰트, 각 요소의 위치 변경, 백그라운드 및 간단한 애니메이션(transition) 등의 효과를 주어 보다 보기 좋게 꾸며 주는 역할을 합니다.

## HTML에서 CSS를 적용하는 방법
- HTML과 CSS는 각자의 문법을 갖는 별개의 언어이며 HTML은 CSS를 포함할 수 있다.
- 그러나 HTML없이 단독으로 존재하는 CSS는 의미가 없다.

## CSS 기본 문법
![image](img/css_grammer.png)

### 1. 선택자(Selector)
- CSS를 적용하고자 하는 HTML 요소(element)
### 2. 선언부
- 하나 이상의 선언들을 세미콜론(;)으로 구분하여 포함할 수 있으며, 중괄호({})를 사용하여 전체를 둘러 싼다.
- 각 선언은 CSS 속성명(property)과 속성값(value)을 가지며, 그 둘은 콜론(:)으로 연결된다.
- 이러한 CSS 선언은 언제나 마지막에 세미콜론으로 끝마친다.

## CSS 선언 방식

### 인라인(in-line)방식
- HTML의 각 태그에서 style 속성에 직접 작성하는 방식
```html
<p style="color: red">빨간색</p>
```

### 내장(embaedded)방식
- 같은 HTML 문서에 \<style\> \</style\> 태그 안에 스타일을 적용하는 방식 입니다. 

### ex01_embedded.html
```html
<!DOCTYPE html>
	<html>
		<head>
			<meta charset="UTF-8">
			<title>css(스타일시트)</title>
			
			<!-- css를 적용하기 위한 태그 -->
			<style type="text/css">
				/* body안에 존재하는 각각의 태그에게 스타일을 적용 */
            /*태그이름{속성 : 속성값;}*/
            
            /*body에 있는 h1 태그는 모두 색이 바뀌게 된다.*/
				h1{color:red;} 
				h2{color:yellow;}
				h3{color:buttonhighlight;}
            
            /*네이버에 생상표 검색 스크롤 내리면 RGB 코드값이 나온다.
            색깔을 16진수로 표현한것. 빨강 초록 파랑 순서.
            16진수는 0~9 A~F까지로 이루어져있다.*/
				p{color:#ffaaff;} /* #faf;  */ 
			</style>
		</head>
		
		<body>
			<h1 >스타일 연습중1-1</h1>
			<h1>스타일 연습중1-2</h1>
			<h2>스타일 연습중2</h2>
			<h3>스타일 연습중3</h3>
			<p>스타일 연습중4</p>
		</body>
	</html>
```

### 링크(link)방식
- HMTL <link>를 이용하여 외부 문서로 CSS를 불러와 적용하는 방식
```html
<link rel=”stylesheet” type=”text/css” href='css 외부 파일 경로'>
```
### css 폴더 만들고 style.css 파일 만들기
```css
p{color : red;}
```

### ex02_link.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link href="css/style.css" rel="stylesheet" type="text/css">
    <title>Document</title>
</head>
<body>
    <p>외부 스타일시트 적용</p>
</body>
</html>
```

### @import 방식
- @import를 이용하여 외부 문서로 CSS를 불러와 적용하는 방식

### ex03_import.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <style>
        @import url(css/style.css);
    </style>
    <title>Document</title>
</head>
<body>
    <p>외부 스타일시트 적용</p>
</body>
</html>
```

## CSS 선택자

### HTML 요소 선택자
- CSS를 적용할 대상으로 HTML요소의 이름을 직접 선택하여 사용할 수 있다.
```css
h2 { color: teal; text-decoration: underline; }


<h2>이 부분에 스타일을 적용합니다.</h2>
```
   
### 클래스(class)선택자
- 특정 집단의 여러 요소를 한 번에 선택할 때 사용한다.
- 이런 특정 집단을 묶을 수 있는걸 클래스(class)라고 하며, 같은 클래스 이름을 가진 요소들을 모두 선택해준다.

### ex04_class.html
```html
<!DOCTYPE html>
	<html>
		<head>
			<meta charset="UTF-8">
			<title>class를 통한 css 적용</title>
			<style type="text/css">

			.headings {
				color: lime;
				text-decoration: overline;
			}
			.headings2 { 
				color: blue; 
				font-size: 50px; 
			}
			/*h2 태그에서 class가 headings 인것만 적용*/
			h2.headings{
				color : red;
			}

			</style>
		</head>
		
		<body>
			<!-- class의 개념은 똑같은 태그가 있을때 구별하기 위한 식별자가 된다. -->
			<h1>클래스 선택자를 이용한 선택</h1>

			<h2 class="headings">이 부분에 스타일을 적용합니다.</h2>

			<p>클래스 선택자를 이용하여 스타일을 적용할 HTML 요소들을 한 번에 선택할 수 있습니다.</p>

			<h3 class="headings">이 부분에도 같은 스타일을 적용합니다.</h3>

			<h3 class="headings headings2">이 부분에는 다른 스타일과 추가 스타일을 적용합니다.</h3>
		</body>
	</html>
```
      
### 클래스 활용
### ex05_class.html
```html
<!DOCTYPE html>
	<html>
		<head>
			<meta charset="UTF-8">
			<title>일부분 지정 태그(span)를 통한 css변경</title>
			<style type="text/css">
				/* .box라는 이름을 가진 클래스의 자식(내부태그)중 h2태그에게만 속성을 적용  중요!
            			띄어쓰기가 내 안에 있는 자식 요소에 적용되기 때문에 띄어쓰기는 주의해서 써야 한다.*/
				body .box h2{color:aqua;}
				span{color : gray;}
				body div.box h2 span{color : red;};
			</style>
		</head>
		
		<body>
			<div class="box">
				
				<h2>클래스 <span>연습</span>중</h2>
				<h1>클래스 연습중이라고!</h1>
				
				<div class="box">
					<h2>클래스 <span class="span_blue">연습</span>중2</h2>
					
				</div>
			</div>
			
			<h2>클래스 <span class="span_green">연습</span>중3</h2>
		</body>
	</html>
```
      
### id
- CSS를 적용할 대상으로 특정 요소를 선택할 때 사용
- 이때 #을 써서 구분해준다.
#### 주의
- HTML과 CSS에서는 하나에 웹 페이지에 속하는 여러 요소에 같은 아이디 이름을 사용해도 문제없이 동작한다.
- 하지만 중복된 아이디를 이용해 자바스크립트 작업을 하게 되면 오류가 발생한다.
- 되도록 하나의 웹 페이지에 속하는 요소에는 다른 아이디 이름을 사용하거나 클래스를 사용하는것이 좋다.
```html
<!DOCTYPE html>
	<html>
		<head>
			<meta charset="UTF-8">
			<title>id를 통한 css적용</title>
			<style type="text/css">
			/* a1이라는 id를 가진 태그로 접근
				클래스 접근은 .
				id 접근은 #
			 */
         
			#a1{ color : red;}
			#a2{ color : blue;}			
			</style>
		</head>
		
		<body>
         
			<!-- id속성은 절대로 중복된 이름으로 작성하지 않는다. 필수! -->
         <!-- 클래스의 경우 디자인을 위해서 만들어졌기 때문에 겹쳐도 상관이 없지만
              id의 경우 나중에 서버로 데이터를 넘겨야 하는 경우도 있기 때문에 혼란을 야기할 수 있다.-->
			<h1 id="a1">아이디 연습중</h1>
			<h1 id="a2">아이디 연습중2</h1>
		</body>
	</html>      
```

### 그룹(group)선택자
- 위에서 언급한 여러 선택자를 같이 사용할 때 사용한다.
- 여러 선택자를 쉼표(,)로 구분하여 연결한다.
- 그룹 선택자는 코드를 중복해서 작성하지 않도록 하여 코드를 간결하게 만들어준다.

### ex06_group.html
```html
<!DOCTYPE html>
	<html>
		<head>
			<meta charset="UTF-8">
			<title>class를 통한 css 적용</title>
			<style type="text/css">

			h1, h2, p { background-color: lightgray; }

			</style>
		</head>
		
		<body>
			<h1>그룹 선택자를 이용한 선택</h1>

			<h2 class="headings">이 부분에 스타일을 적용합니다.</h2>

			<p>그룹 선택자를 이용하여 여러 선택자를 같이 사용할 수 있다.</p>

			<h3 class="headings">선택하지 않으면 적용되지 않는다.</h3>

		</body>
	</html>
```

### 전체 선택자
- 전체 선택자는 *문자를 사용하여 HTML문서 내의 모든 요소를 선택한다.
```html
<!DOCTYPE html>
	<html>
		<head>
			<meta charset="UTF-8">
			<title>class를 통한 css 적용</title>
			<style type="text/css">

			* { color: teal; text-decoration: underline; }

			div * { color: teal; text-decoration: underline; }

			</style>
		</head>
		
		<body>
			<h1>전체 선택자를 이용한 선택</h1>

			<h2>전체가 선택된다.</h2>

			<p>특정 요소 안의 모든 요소를 선택할수도 있다.</p>
			<div>
				<h3>선택한 요소 안쪽에만 스타일이 적용된다.</h3>
			</div>

		</body>
	</html>
```

### 스타일 적용 우선순위
- 인라인 선언 방식
- 아이디 선택자
- 클래스 선택자
- 태그 선택자
- 전체 선택자


## 테두리(border)
```html
<!DOCTYPE html>
	<html>
	<head>
		<meta charset="UTF-8">
		<title>border(테두리, 선)</title>
		<style type="text/css">
		
		/* border-width: 선 두께
		   border-color: 선 색상 */
			p{ border-width:15px;
			   border-color:blue;}
		
		/* border-style:선의 스타일 */
			.box1{ border-style:none;}
			.box2{ border-style:hidden;}
			.box3{ border-style:dotted;} /* 점선 */
			.box4{ border-style:dashed;} /* 긴 점선 */
			.box5{ border-style:solid;} /* 가장 많이 사용하는 스타일 */
			.box6{ border-style:double;}
			.box7{ border:20px groove red;} /* 두께감을 가지는 테두리 */
			.box8{ border:20px green ridge;}
			.box9{ border-style:inset 20px green;} /* 두께감은 없지만 그림자는 있음 */
			
			.box10{ border:20px outset #F00;} /* 한번에 스타일주기 (두께, 스타일, 색깔)  */
			
			.div1{ border:10px solid #0f0;}
			
			.div2{ border:10px solid red;
				   width:200px;
				   height:100px;}
			/*옆에 공간이 생겨도 요소가 들어올수 없다 왜 block요소니까*/
			
			/* 상,우,하,좌 순으로 각각 다른 두께의 테두리를 주고 싶다면 반드시 border-로 시작하는 속성을 사용해야 한다. */
			.div3{ border-bottom : 10px solid blue;
					border-top : 10px solid green;
					border-left : 10px solid red;}
					
			/*inline태그이기 때문에 원하지 않는 영역까지 침범할 수 있다. */
			span{border : 5px solid #aaf;
				width : 500px;
				height:100px;}
		</style>
	</head>
	
	<body>
		<p class="box1">border테스트 중1</p>
		<p class="box2">border테스트 중2</p>
		<p class="box3">border테스트 중3</p>
		<p class="box4">border테스트 중4</p>
		<p class="box5">border테스트 중5</p>
		<p class="box6">border테스트 중6</p>
		<p class="box7">border테스트 중7</p>
		<p class="box8">border테스트 중8</p>
		<p class="box9">border테스트 중9</p>
		<p class="box10">border테스트 중10</p>
		
		<div class="div1">
			div에 테두리 주기
		</div>
		<div class="div2">
			div에 테두리 주기
		</div>
		<div class="div3">
			div에 테두리 주기
		</div>
		
		<span>hi</span>
		<span>hello</span>
	</body>
	</html>
```
![image](https://user-images.githubusercontent.com/54658614/227424421-e35f6ce4-b5e1-4f27-bd4a-b73cfe5d9a04.png)

## padding(패딩)
- 내용(content)와 테두리(border) 사이의 간격의 크기를 설정해주는 속성
```html
<!DOCTYPE html>
	<html>
		<head>
		<meta charset="UTF-8">
		<title>padding(안쪽여백)</title>
		<style>
			p{ border:5px solid red;
		 	background:yellow;}
		 	
			.box1{ padding:20px;} /*상자 안에 상,하,좌,우 여백을 준다. */
		 	.box2{ padding:20px 50px;}/* 상하,좌우 적용*/
		 	.box3{ padding:20px 50px 30px;}/*상,좌우,하 적용  */
		 	.box4{ padding:20px 30px 50px 70px;}/*상,우,하,좌 적용 */
		 	/* 0픽셀인 경우 px단위는 생략이 가능하다. */
		 	
		 	/* padding- 방향을 통해 방향별로 패딩을 설정할 수도 있다. */
		 	.box5{ padding-left : 20px;}
		 	/* .box5{ padding-left : 0 0 0 20px;} */
		 		   
		 	
		</style>
		</head>
		
		<body>
			<p class="box1">패딩적용 연습중</p>
			<p class="box2">패딩적용 연습중</p>
			<p class="box3">패딩적용 연습중</p>
			<p class="box4">패딩적용 연습중</p>
			<p class="box5">패딩적용 연습중</p>
		</body>
	</html>
```

![image](https://user-images.githubusercontent.com/54658614/227425331-8a4d70bd-286f-454b-b881-0b995292104b.png)

- padding을 주다보면 태그의 크기가 커질 수 있다.

## margin(마진)
- 요소 주변 여백을 뜻합니다.
```html
<!DOCTYPE html>
	<html>
		<head>
			<meta charset="UTF-8">
			<title>margin(바깥쪽 여백)</title>
			<style>
				p{border:2px solid red;
				  background: aqua;}
				  
				div{border:2px solid red;}
				
				#box1{ margin:0;}/* 상,하,좌,우 적용 */
				#box2{ margin:20px 50px;} /* 상하,좌우 적용 */
				#box3{ margin:30px 50px 30px;} /*상,좌우,하 적용  */
				#box4{ margin:50px 30px 40px 70px;}
				
				div{ margin: 50px;}
			</style>
		</head>
		
		<body>
			<p id="box1">마진적용 테스트중</p>
			<p id="box2">마진적용 테스트중</p>
			<p id="box3">마진적용 테스트중</p>
			<p id="box4">마진적용 테스트중</p>
			
			<div id="box5">div마진</div>
			<div id="box6">div마진</div>
		</body>
	</html>
```
![image](https://user-images.githubusercontent.com/54658614/227426311-b60e2466-8961-48d5-b924-d8cab96f7321.png)

- margin을 주다보면 태그의 크기가 줄어들 수 있다.

## Font(폰트)
```html
<!DOCTYPE html>
	<html>
		<head>
			<meta charset="UTF-8">
			<title>font</title>
			
			<style>
			.a1{font-size: 30px;} /* 글자 크기 */
			.a2{font-style: italic;} /* 기본값 : normal */
			.a3{font-weight: bold;} /* 기본값 : normal */
			
			/* 글꼴을 여러종류 준비해두고, 브라우저에서 지원한다면 그것을 사용하되, 지원하지 않는다면 가장 표준적인 글꼴인 sans-serif를 사용한다. */
			.a4{font-family: "궁서","굴림",sans-serif;}/* 콤마로 구별하는 스타일 */
			
			/* 폰트에 속성을 한번에 적용하려면 weight,style,size, family 순으로 적용해야 한다!! 순서를 지키지 않으면 속성 적용이 안된다.  */
			.a5{font: bold	/* weight */
					  italic /* style */
					  30px	/* size */
					  "궁서","돋움",sans-serif;} /* family */
					  
			.a6{font-variant: small-caps;}
			</style>
		</head>
		
		<body>
			<p class="a1"> 글자크기</p>
			<p class="a2"> 글자 스타일</p>
			<p class="a3"> 글자 두께</p>
			<p class="a4"> 글꼴 변경</p>
			<p class="a5"> 글꼴 최종</p>
			<p class="a6"> The end</p>
		</body>
	</html>

```
![image](https://user-images.githubusercontent.com/54658614/227431474-fb561cc2-3f97-46c1-b564-d78abac58010.png)

    

## dl CSS 응용 실습
- 아래와 같은 모습 만들어보기

![image](https://user-images.githubusercontent.com/54658614/227429600-87563155-f888-4604-b2ee-7f405a75e832.png)

    - 우리가 margin이나 padding을 주지 않아도 어느정도 값을 갖고있는 태그들이 있다.
    
![image](https://user-images.githubusercontent.com/54658614/227430215-3fd3cb4b-b965-4577-b9fc-a669704b47a1.png)


```html
<!DOCTYPE html>
	<html>
		<head>
		<meta charset="UTF-8">
		<title>Insert title here</title>
		<style>
			*{ margin:0; padding:0;} /* *: body안에 있는 모든 태그를 의미한다. */
			
			dl{ width: 650px;
				border: 5px double gray;
				
				/* 상하마진0, 좌우마진 브라우저 자동정렬 -> auto를 사용하려면 반드시 width속성이 적용되어 있어야 한다. */
				margin: 0 auto;}
			
			dt{ padding: 15px 0;
				text-align:center;
				background:#aaf;
				border-bottom:10px solid blue;
				letter-spacing:10px;}/* 자간 */
			
			dd{ padding: 10px 0;
				text-indent:15px;}/*  */
		
			.line{ border-bottom:3px dotted black;}
			
		</style>
		</head>
		
		<body>
			<dl>
				<dt>한국의 속담</dt>
					<dd class="line">
						까마귀 날자 배 떨어진다.
					</dd>
					<dd>
						발없는 말이 천리 간다.
					</dd>
			</dl>
		
		</body>
	</html>
```

## 배경이미지
배포받은 이미지 폴더 넣기
![image](https://user-images.githubusercontent.com/54658614/227840344-1e3bbe4b-cb09-4fdd-bee7-e8e079fd3a93.png)
```html
<!DOCTYPE html>
	<html>
		<head>
			<meta charset="UTF-8">
			<title>배경</title>
			<style>
				p{border : 1px solid black;
				  width:300px;
				  height:300px;
				  background:url(image/backgroundImage.gif);} 
				  /* no-repeat(한번만 찍기), 
				     repeat(바둑판)
				     repeat-x(x좌표로 출력)
 				     repeat-y(y좌표로 출력) */
				
			</style>
		</head>
		
		<body>
			<p></p>
		</body>
	</html>
```
![image](https://user-images.githubusercontent.com/54658614/227840955-6482ede1-6037-41f0-9223-0f3ea3523797.png)

```css
p{border : 1px solid black;
  width:300px;
  height:300px;
  background:yellow url(image/backgroundImage.gif) repeat-x;} 
```
![image](https://user-images.githubusercontent.com/54658614/227841459-c959f15c-a20a-4294-815c-449fb41ce459.png)
  
### 이미지 위치 지정하기
```html
<!DOCTYPE html>
	<html>
		<head>
			<meta charset="UTF-8">
			<title>배경</title>
			<style>
				p{border : 1px solid black;
				  width:300px;
				  height:300px;
				  background:url(image/backgroundImage.gif);} 
				  /* no-repeat(한번만 찍기), 
				     repeat(바둑판)
				     repeat-x(x좌표로 출력)
 				     repeat-y(y좌표로 출력) */
				     
				.b1{ background : url(image/background.jpg) no-repeat right bottom;}
				.b2{ background : url(image/background.jpg) no-repeat 20px 100px;} /* x좌표 y좌표 */
				.b3{ background : url(image/background.jpg) no-repeat center;} /* 중앙배치 */
				.b4{ background : url(image/background.jpg) no-repeat 70% 10%;}
				
			</style>
		</head>
		
		<body>
			<p></p>
			<p class="b1">
			나는 눈이 좋아서 꿈에 눈이 오나봐
			</p>
			<p class="b2">
			나는 눈이 좋아서 꿈에 눈이 오나봐
			</p>
			<p class="b3">
			나는 눈이 좋아서 꿈에 눈이 오나봐
			</p>
			<p class="b4">
			나는 눈이 좋아서 꿈에 눈이 오나봐
			</p>
		</body>
	</html>
```
![image](https://user-images.githubusercontent.com/54658614/227842541-0b16dcd1-b383-4e69-847f-60baf3dd0032.png)
  
### ul_background
```html
<!DOCTYPE html>
	<html>
		<head>
			<meta charset="UTF-8">
			<title>ul의 CSS 활용</title>
			
			<style>
				.u1 li{ border: 1px solid black;
						list-style:none/* li에 있는 점을 제거*/;
						padding-left : 10px;
						background: url(image/arrow.gif) no-repeat 0 50%;}
						
						
				.u2 li{ background: url(image/arrow.gif) no-repeat 0 50%;
						padding-left: 10px;
						list-style:none;
						margin-left: -10px}
			</style>
			
		</head>
		
		<body>
			<ul class="u1">
				<li>홈</li>
				<li>갤러리</li>
				<li>게시판</li>
				<li>오시는길</li>
			</ul>
			
			<ul class="u2">
				<li>홈</li>
				<li>갤러리</li>
				<li>게시판</li>
				<li>오시는길</li>
			</ul>
		</body>
	</html>
```
![image](https://user-images.githubusercontent.com/54658614/227843171-490e2c32-3f9f-4fbe-b833-a74a8bb4f20d.png)
  
## 후손 선택자
```html
<!DOCTYPE html>
	<html>
		<head>
			<meta charset="UTF-8">
			<title>후손선택자</title>
			<style type="text/css">
			/* 공백 기호는 후손선택자로서 해당 아이디를 가지는
			하위 요소들이 모두 영향을 받는다. */
			#header h1{ color:red;}
			#contents h1{ color:blue;}
			</style>
		</head>
		
		<body>
			<div id="header">
				<h1>나는 Header의 자식이오</h1>
				
				<div id="nav">
				<h1>navigation</h1>
				</div>
				
				<div id="nav1">
				<h1>navigation</h1>
				</div>
			</div>
			
			<div id="contents">
				<h1>contents의 자식</h1>
			</div>
		</body>
	</html>
```
![image](https://user-images.githubusercontent.com/54658614/227844112-dc5f315d-a4c0-4a68-a353-bb4e24d1a695.png)


## 자식선택자
```html
<!DOCTYPE html>
	<html>
		<head>
			<meta charset="UTF-8">
			<title>자식선택자</title>
			<style type="text/css">
			
			/*  > : 자식선택자는 직계자식에게만 속성을 적용한다. */
			#header > h1{ color:red;}
			#contents > h1{ color:blue;}
			</style>
		</head>
		
		<body>
			<div id="header">
				<h1>Header의 자식</h1>  <!-- header의 직계자식 -->
				
				<div id="nav">
				<h1>navigation</h1>
				</div>
				
				<div id="nav1">
				<h1>navigation</h1>
				</div>
			</div>
			
			<div id="contents">
				<h1>contents의 자식</h1> <!-- contents의 직계자식 -->
			</div>
		</body>
	</html>
```

![image](https://user-images.githubusercontent.com/54658614/227845091-4a10c682-4c1d-4667-9e37-64c86d78e4b5.png)

## display
		
- block : 한 영역을 차지 하는 박스형태를 가지는 성질이다. 그렇기 때문에 기본적으로 block은 width값이 100%이다.
	- block은 height와 width값을 지정할 수 있다.
	- block은 margin과 padding을 지정할 수 있다.
		
- inline : 주로 텍스트를 주입할 때 사용되는 형태이다. 기본적으로 block처럼 width값이 100%가 아닌 컨텐츠 영역만큼 자동으로 잡히게 된다.<br>높이 또한 폰트의 크기만큼 잡힌다.
	- width와 height를 명시할 수 없다.
	- margin은 위아래엔 적용되지 않는다.
	- padding은 좌우 공간과 시각적인 부분이 모두 적용되지만 위아래는 시각적으로는 추가되지만 공간을 차지하지는 않는다.
		
- inline-block : inline-block은 말 그대로 inline의 특징과 block의 특징을 모두 가진 요소이다.
	- 줄바꿈이 이루어지지 않는다.
	- block처럼 width와 height를 지정할 수 있다.
	- 만약 width와 height를 지정하지 않을 경우, inline과 같이 컴텐츠만큼 영역이 잡힌다.
- 블록요소를 인라인 요소로 바꾸고 싶다면 display 속성을 inline으로 주면 된다.
- 인라인요소를 블록 요소로 바꾸고 싶다면 display 속성을 block으로 주면 된다.

	
```html
<!DOCTYPE html>
	<html>
		<head>
			<meta charset="UTF-8">
			<title>인라인 요소 -> 블록 요소</title>
			<style type="text/css">
				img{ display:block;} /* display: block -> 인라인 요소를 블록요소로 바꿈  display: inline -> 블록요소를 인라인요소로 바꿈*/
				span{ display:block;
					  border:1px solid red;}
				
			</style>
		</head>
		
		<body>
			<div> 
				<img src="image/acid2Test.jpg" alt="스마일"/>
				<span>이미지1</span>
				<span>웃어요</span>
				<a href="#">링크</a>
			</div>
		</body>
	</html>
```
![image](https://user-images.githubusercontent.com/54658614/227845635-62384bc3-8850-428e-a67b-0c3c8a0ac963.png)

## block_menu
```html
<!DOCTYPE html>
	<html>
		<head>
			<meta charset="UTF-8">
			<title>스타일 시트를 통한 메뉴 만들기</title>
			
		</head>
		
		<body>
			<div id="box">
				<h1>KOREA IT</h1>
				
				<ul>
					<li><a href="#">COMPANY</a></li>
					<li><a href="#">PRODUCT</a></li>
					<li><a href="#">SERVICE</a></li>
					<li><a href="#">COMMUNITY</a></li>
				</ul>
			</div>
		
		</body>
	</html>
```
![image](https://user-images.githubusercontent.com/54658614/227845802-0ad0bbc3-7ef0-4e5a-9136-4554e8554d36.png)

점도 제거하고 마우스를 올렸을 때 색깔이 바뀌도록 만들어보자<br>

```html
<!DOCTYPE html>
	<html>
		<head>
			<meta charset="UTF-8">
			<title>스타일 시트를 통한 메뉴 만들기</title>
			
 			<style>
			 *{ margin:0; padding: 0;}
			li{ list-style:none;}
			ul{ width:130px;
				margin-left: 20px}
				
			 a{ 	display:block; /* 글씨의 개수가 다르기 때문에 영역을 통일하기 위해 블록으로 만듦*/
			 	background:#f30;
			 	padding:5px;/*인라인 요소에서 padding값을 잘못주면 영역을 침범한다.*/
				margin:3px 0;
				text-decoration: none; /* a태그 밑줄제거 */
				color:#fff; /*글자색 변경*/
				font-weight:bold;
			 	}
			 
			 /*특정한 상황을 구별해주는 키워드
			    : 가 붙어있으면 가상클래스라고 생각하면 좋다.*/
			 /* a:hover -> a태그에 마우스 오버시 동작 
			 가상클래스 일종의 이벤트 처리 */
			 a:hover{ background:#0c0;
			          color:#f00;
			          text-decoration: underline;}
			 		  
			 		  
			</style>
		</head>
		
		<body>
			<div id="box">
				<h1>KOREA IT</h1>
				
				<ul>
					<li><a href="#">COMPANY</a></li>
					<li><a href="#">PRODUCT</a></li>
					<li><a href="#">SERVICE</a></li>
					<li><a href="#">COMMUNITY</a></li>
				</ul>
			</div>
		
		</body>
	</html>
```
![image](https://user-images.githubusercontent.com/54658614/227846065-491da9d9-ac52-45c9-8031-f06ec987df30.png)

## overflow
```html
<!DOCTYPE html>
	<html>
		<head>
			<meta charset="UTF-8">
			<title>overflow(영역관리)</title>
		<style>
			p{ border: 1px solid black;
			   width: 150px;
			   height:70px;
			   padding:5px;}
			   
			   /* 지정된 영역 안에서만 내용을 표시 */
			.a1{ overflow:hidden;}
			
			   /* 내용의 넘침에 관계없이 무조건 스크롤을 보여준다 */
			.a2{ overflow:scroll;}
			   
			   /* 내용dl 넘칠때만 스크롤을 보여준다 */
			.a3{ overflow:auto;}
			
			/* 기본값 - 내용을 모두 표시 */
			.a4{ overflow:visible ;}
		</style>
		
		</head>
		
		<body>
			<p class="a1">Michaelmas term lately over, and the Lord Chancellor sitting in Lincoln's Inn Hall. Implacable November weather. As much mud in the streets as if the waters had but newly retired from the face of the earth.</p>
			<p class="a2">Michaelmas term lately over, and the Lord Chancellor sitting in Lincoln's Inn Hall. Implacable November weather. As much mud in the streets as if the waters had but newly retired from the face of the earth.</p>
			<p class="a3">Michaelmas term lately over, and the Lord Chancellor sitting in Lincoln's Inn Hall. Implacable November weather. As much mud in the streets as if the waters had but newly retired from the face of the earth.</p>
			<p class="a4">Michaelmas term lately over, and the Lord Chancellor sitting in Lincoln's Inn Hall. Implacable November weather. As much mud in the streets as if the waters had but newly retired from the face of the earth.</p>
		</body>
	</html>
```
![image](https://user-images.githubusercontent.com/54658614/227847299-22dccf38-0b7b-4d15-ae1b-451a06c91f43.png)

## 가상클래스
- 제일 위쪽이나 제일 아래쪽에 대해서는 CSS를 적용할 수는 있으나 중간의 요소에는 적용시킬 수 없다는 단점이 있다.
```html
<!DOCTYPE html>
	<html>
		<head>
			<meta charset="UTF-8">
			<title>가상클래스 : first-child</title>
			<style>
				li{list-style:none;}
				li:first-child{ color:red;}/* first-child -> 특정 태그를 골라내는 것 */
				li:last-child{ color:blue;}
			</style>
			
		</head>
		
		<body>
			<ul>
				<li>A</li>
				<li>B</li>
				<li>C</li>
				<li>D</li>
			</ul>
			
			<ul>
				<li>A</li>
				<li>B</li>
				<li>C</li>
				<li>D</li>
			</ul>
		</body>
	</html>
```
![image](https://user-images.githubusercontent.com/54658614/227848081-8e9c2f19-b621-49f3-8a37-8a2e6b010b78.png)

## pseudo(슈도)클래스
```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>pseudo(슈도클래스)</title>
		<style>
		li{list-style:none;
			display:inline;}
		
		li:nth-child(odd){ color:red;}/* 홀수번째 태그만 변경 */
		li:nth-child(even){ color:green;}/* 짝수번째 태그만 변경 */
		li:nth-child(4n){ color:blue;}/* 4의배수 태그만 변경 */
		li:nth-child(6){ color:skyblue;
				 background:yellow;}/* 내가 원하는 번호 태그만 변경 */
		</style>
	</head>
	
	<body>
		<ul>
			<li>1</li>
			<li>2</li>
			<li>3</li>
			<li>4</li>
			<li>5</li>
			<li>6</li>
			<li>7</li>
			<li>8</li>
			<li>9</li>
			<li>10</li>
		</ul>
	</body>
</html>
```

![image](https://user-images.githubusercontent.com/54658614/227848660-0dd50dc8-ea3f-44fc-b85a-12bd1552e698.png)

## link 가상클래스
```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>a태그의 가상클래스</title>
		<style>
			li{list-style:none;
			   margin:5px 0;}
			 a{ text-decoration: none;
			 	color:gray; }
			 	
			 a:hover {color:orange;
			 		  text-decoration: line-through} /* 마우스를 올렸을때 */
			 
			 /* hover -> active 순서로 작성해야 정상적으로 작동한다. */
			 a:active {color:red;}					 /* 클릭을 했을 때  */
		</style>
	</head>
	
	<body>
		<ul>
			<li><a href="#">홈</a></li>
			<li><a href="#">회원가입</a></li>
			<li><a href="#">사이트맵</a></li>
			<li><a href="#">오시는길</a></li>
		</ul>
	</body>
</html>
```
- 마우스를 올리면 오렌지색으로 바뀜
- 클릭하면 빨간색으로 바뀜

![image](https://user-images.githubusercontent.com/54658614/227849252-ea370975-0670-4850-9b64-11ec34e0e1b5.png)

## z-index
```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>z인덱스</title>
		<style type="text/css">
			*{margin:0; padding:0;}
			
			p{ border:1px solid black;
			   width:80px;
			   height:80px;
			   padding:10px;
			   position:absolute;} /* 겹쳐서 표시 하는데 마지막에 만든게 가장 위로 올라온다. */
			
			.box1{ background:yellow;
				   left:100px; top:100px;
				   z-index: 3; } /* 화면에 표시되는 순서를 바꿔준다. z인덱스 값이 클수록 화면 위쪽에 보여진다. */
			.box2{ background:orange;
				   left:120px; top:120px;
				   z-index: 2; }
				   
			.box3{ background:teal;
				   left:140px; top:140px; 
				   z-index: 1;}
			
		</style>
	</head>
	
		<body>
			<p class="box1">박스1</p>
			<p class="box2">박스2</p>
			<p class="box3">박스3</p>
		</body>
</html>
```
![image](https://user-images.githubusercontent.com/54658614/228431870-9c5776d2-c42c-4937-a9a1-422750decb7f.png)

## float
- float 프로퍼티는 해당 요소를 다음 요소 위에 떠 있게 한다. 
- 여기서 떠 있다(float)는 의미는 요소가 기본 레이아웃 흐름에서 벗어나 요소의 모서리가 페이지의 왼쪽이나 오른쪽에 이동하는 것이다.
- 보통 레이아웃을 구성할 때 요소를 가로 정렬하기 위해 사용되는 기법이다.
- inherit : 부모 요소에서 상속
- left : 요소를 왼쪽으로 이동시킨다.
- right : 요소를 오른쪽으로 이동시킨다.
- none : 요소를 떠 있게 하지 않는다.(기본값)
```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>float(왼쪽, 오른쪽 정렬)</title>
		
		<style type="text/css">
			
			/* float : 블록요소의 객체를 왼쪽이나 오른쪽으로 나란히 정렬하기 위한 속성
			float은 '부유하다'의 의미를 가지고 있으므로, 부모영역에 영향을 받지 않고 공중에 떠오른 형태가 된다. */

			div.page {
				border: 3px solid #CD5C5C;
				overflow: auto;
			}

			/* float 속성을 사용하면 해당 요소는 일반적인 흐름에서 벗어나게 되어 요소의 부모 요소는 해당 요소의 높이를 인식하지 못하게 되는데, 이 경우 부모 요소에 overflow : hidden 속성을 추가하여 해결할 수 있다. */

			h2 { text-align: center; }

			header { border: 3px solid #FFD700; }

			nav {
				border: 3px solid #FF1493;
				width: 150px;
				float: left; // 네비게이션은 왼쪽 위치
			}

			section {
				border: 3px solid #00BFFF;
				margin-left: 156px;
			}

			footer{ border: 3px solid #00FA9A; }
		</style>
	</head>
	
	<body>
		<h1>float 속성을 이용한 레이아웃</h1>
<div class="page">

    <header>
        <h2>header 영역</h2>
    </header>
    <nav>
        <h2>nav 영역</h2>
        <p>여기에는 보통 메뉴가 들어갑니다.</p>
    </nav>
    <section>
        <h2>section 영역</h2>
        <p>여기에는 페이지에 해당하는 내용이 들어갑니다.<br>
            Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nam ornare sapien suscipit tincidunt ullamcorper. Cras ac sem sed mauris maximus rhoncus vel in metus. Nam pharetra arcu sit amet dolor interdum, eget scelerisque libero finibus. Phasellus quis vulputate ante. Fusce sit amet viverra justo. Donec id elementum mauris. Nam id porttitor nisl, et suscipit nunc. Vestibulum sit amet volutpat quam. Cum sociis natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus. Duis placerat sem eu facilisis ultricies.
        </p>
    </section>
    <footer>
        <h2>footer 영역</h2>
    </footer>
</div>
	</body>
</html>
```
![image](https://user-images.githubusercontent.com/54658614/228128504-69c6961d-eb32-4135-b992-2252e2ec08cf.png)


## CSS Flexbox
- 모던 웹을 위하여 제안된 기본 layout보다 더 세련된 방식으로 만들기 위해 나온 방식이다.
- 요소의 사이즈가 불명확하거나 동적으로 변화할 때에도 유연한 레이아웃을 실현할 수 있다.
- 복잡한 레이아웃이라도 적은 코드로 보다 간단하게 표현할 수 있다.

## Flexbox 기본구조
- flex item
  - 복수의 자식 요소
- flex - container
  - 부모 요소

### flexbox 설정
- HTML 부모 요소의 display 속성에 flex를 지정한다.(부모 요소가 inline인 경우 inline-flex를 지정한다.)

### flexbox의 속성
- flex container에 정의하는 속성
  - 전체적인 정렬이나 흐름에 관련된 속성
- flex item에 정의하는 속성
  - 자식 요소의 크기나 순서에 관련된 속성

### Flex Container 속성
|속성|의미|
|-----|-----|
|display| Flex Container를 정의|
|flex-direction| Flex Items의 주 축(main-axis)을 설정|
|flex-wrap| Flex Items의 여러 줄 묶음(줄 바꿈) 설정|
|flex-flow| flex-direction와 flex-wrap의 단축 속성|
|justify-content| 주 축(main-axis)의 정렬 방법을 설정|
|align-content| 교차 축(cross-axis)의 정렬 방법을 설정(2줄 이상)|
|align-items| 교차 축(cross-axis)에서 Items의 정렬 방법을 설정(1줄)|


### flex-direction
- flex 컨테이너의 주축(main-axis) 방향을 설정한다.
- 주의할 점은, Items 순서를 거꾸로 뒤집거나, 수직으로 설정할 경우, 주 축과 교차축 모두 변하게 된다.
#### flex-direction : row;
- 좌에서 우로 수평 배치된다.(기본값)

![image](img/flex_row.png)


#### flex-direction : row-reverse;
- 우에서 좌로 수평 배치된다.

![image](img/flex_row_reverse.png)

#### flex-direction : column;
- 위에서 아래로 수직 배치된다.

![image](img/flex_column.png)
#### flex-direction : column-reverse;
- 아래서 위로 수직 배치된다.

![image](img/flex_column_reverse.png)

### flex-wrap
- flex 컨테이너의 복수 flex item을 1행으로 또는 복수행으로 배치한다.
- flex는 기본적으로 1차원 형태이다.
- flex 컨테이너의 width보다 flex item들의 width의 합계가 더 큰 경우, 한줄로 표현할 것인지, 여러줄로 표현할 것인지를 지정한다.

#### flex-wrap : nowrap;
- item을 개행하지 않고 1행에 배치한다.
- 각 item의 폭은 container에 들어갈 수 있는 크기로 축소된다.

![image](img/flex_nowrap.png)

#### flex-wrap : wrap;
- item들의 width합계가 flex 컨테이너의 width보다 큰 경우, item을 복수행에 배치한다.
- 기본적으로 좌에서 우로, 위에서 아래로 배치된다.

![image](img/flex_wrap.png)

#### flex-wrap : wrap-reverse;
- wrap과 동일하나 아래에서 위로 배치된다.

![image](img/flex_wrap_reverse.png)

### flex-flow
- direction과 wrap속성을 설정하기 위한 shorth and이다.

```css
/* flex-flow : direction속성  wrap속성*/
flex-flow : row-reverse wrap;
```


### justify-content
- container의 main axis를 기준으로 item을 수평 정렬한다.

#### justify-content : flex-start;
- 좌측을 기준으로 정렬한다. (기본값)

![image](img/justify_start.png)

#### justify-content : flex-end;
- 우측을 기준으로 정렬한다.
- flex-direction:row-reverse와 다른점은 1,2,3,4,5 순서는 그대로인점

![image](img/justify_end.png)

#### justify-content : center;
- 중앙에 정렬한다.

![image](img/justify_center.png)

#### justify-content : space-between;
- 첫번째와 마지막 item은 끝에 정렬되고 나머지는 균등한 간격으로 정렬된다.

![image](img/justify_between.png)

#### justify-content : sapce-around;
- 모든 item은 균등한 간격으로 정렬된다.

![image](img/justify_around.png);

## position
- static : 기본적인 위치 지정 방식, 문서의 기본적인 흐름을 따른다.
	- 모든 태그들은 처음에 position : static 상태이다.
	- left, top, right, bottom값이 적용되지 않는다.
	- 찰계대로 왼쪽에서 오른쪽, 위에서 아래로 쌓인다.

- relative : static과 유사하나 원래 위치에서 주어진 값만큼 이동한다.
	- top,right,bottom,left 속성을 사용해 위치 조절이 가능하다.
	- Relative 속성에서 top:5px을 주면 아래로 5px을 이동한다.

- absolute : 기본 흐름을 따르지 않고 부모 요소의 상대적위치로 지정된다.
	- 부모 요소의 포지션이 relative, absolute, fixed인 태그가 있다면 부모 요소의 기준으로 움직인다.
	- 부모 요소의 포지션이 static이라면 body태그를 기준으로 배치된다.
	- 부모요소가 없다면 포지션 문서의 body를 기준으로 배치된다.

- fixed : fixed 포지션은 화면의 스크롤이나 움직임에 관계 없이 화면의 특정 부분에 고정되는 포지션이다.
```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>absolute를 통한 스타일시트 적용</title>
		<link rel="stylesheet" href="css/reset.css">
		<style>
			#header{ width:1000px; height:80px; 
					 margin:10px auto;
					 border:1px solid blue;
					 position:relative;}
					 
			/* 자식이 position 속성을 absolute으로 가지고 있으면
			부모가 position 속성을 relative로 가지고 있어야 한다. */
	 
			.aa{position:absolute; /* float처럼 요소가 공중에 붕 뜬다. 내 부모가 body라고 생각을 해버리게 된다. */
			    left: 0; top: 7px;}
			
			ul{overflow:hidden;}
			li{ float:left; }
			
			p:last-child{ position:absolute;
						  right:10px; top:5px;}
						  
			.a1{position:absolute;
				left:350px; top:10px;}
			.a2{position: absolute;
				left:225px; top:40px;}
		</style>
	</head>
	
	<body>
		<div id="header">
			<p class="aa">
				<img alt="이미지" src="image/acid2Test.jpg"
					width="70"; height="60";>
			</p>
			
			<ul class="a1">
				<li><img src="image/menu_13.jpg"/></li>
				<li><img src="image/menu_14.jpg"/></li>
				<li><img src="image/menu_15.jpg"/></li>
				<li><img src="image/menu_16.jpg"/></li>
				<li><img src="image/menu_17.jpg"/></li>
				<li><img src="image/menu_18.jpg"/></li>
				<li><img src="image/menu_19.jpg"/></li>
				<li><img src="image/menu_20.jpg"/></li>
				<li><img src="image/menu_21.jpg"/></li>
				
				</ul>
				
				<ul class="a2">
					<li><img src="image/menu01_12.jpg"/></li>
					<li><img src="image/menu01_13.jpg"/></li>
					<li><img src="image/menu01_14.jpg"/></li>
					<li><img src="image/menu01_15.jpg"/></li>
					<li><img src="image/menu01_16.jpg"/></li>
					<li><img src="image/menu01_17.jpg"/></li>
					<li><img src="image/menu01_18.jpg"/></li>
					<li><img src="image/menu01_19.jpg"/></li>
				</ul>
			<p><img src="image/img_standards.gif"/></p>
		</div>
	</body>
</html>
```
![image](https://user-images.githubusercontent.com/54658614/228131928-b87cbc3f-4962-4a1b-8e6b-57f18f4fd342.png)

## CSS응용 드롭다운 메뉴 만들어보기
```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>Insert title here</title>
		<link rel="stylesheet" href="css/reset.css">
		<style>
			#a1{margin-left:10px;
				margin-top:10px;}
			
			ul{ overflow : hidden;}
			li{ float : left}
			
			li{ border-right : 1px solid gray;
			    border-top : 1px solid gray;
			    border-bottom: 1px solid gray;}
			    
			li:nth-child(1){border-left:1px solid gray;}
			    
			a{display : block; padding : 2px 10px;}
			  
			a:hover{background:#999;
			        color:#fff;}
			        
			.depth_1{margin-left:10px;}
			        
			#menu > li:hover .depth_1 { display : block;}
			          
			#menu .depth_1{display:none;
			             position:absolute;
			             left:0;
			             right:0;
			             text-align:center;}
			 #menu .depth_1 a{display:block; padding:5px;}
			          
			 

		</style>
	</head>
	<body>
			<div id="a1">
				<ul id="menu">
					<li><a href="#">HTML</a>
						<ul class="depth_1">
							<li><a href="#">메뉴1</a></li>
							<li><a href="#">메뉴2</a></li>
							<li><a href="#">메뉴3</a></li>
						</ul>
					</li>
					<li><a href="#">CSS</a></li>
					<li><a href="#">JavaScript</a></li>
					<li><a href="#">JQuery</a></li>
					<li><a href="#">Jsp</a></li>
				</ul>
			</div>

	</body>
</html>
```
![image](https://user-images.githubusercontent.com/54658614/228431464-4da6d184-8e6b-4caf-ba37-7438759c237f.png)

