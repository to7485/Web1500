HTML은 집을 지을 때 뼈대를 갖춰주는 것<br>
CSS는 만들어진 집에 침대는 어디에 배치할지, 조명은 어디에 배치할지, 벽은 무슨색으로 칠할지, 조명은 어디에 배치할지 정하는것<br>

뼈대를 갖추고 디자인을 해야지 가구를 배치하고 벽지를 바르려고 하면 매우 불편할것.<br>

# CSS
- CSS(Cascading Style Sheet)는 HTML과 같은 마크업 언어로 작성된 문서에 색상, 폰트, 각 요소의 위치 변경, 백그라운드 및 간단한 애니메이션(transition) 등의 효과를 주어 보다 보기 좋게 꾸며 주는 역할을 합니다.

## HTML에서 CSS를 적용하는 방법

### HTML 요소 속성으로써 적용하는 방법
- HTML의 각 태그에서 style 속성으로 적용하는 방법이 있습니다. 
```
<p style="color: red">빨간색</p>
```
- 속성으로써 적용을 하게 되면 적용의 우선 순위가 가장 높으므로 바로 적용됩니다. 
- 다만 우선순위가 가장 높고, HTML의 요소 안에 분산되어 산재하므로 유지보수에 어려움이 있을 수 있습니다.

### HTML \<style\>~\</style\> 요소로 적용하는 방법
- 같은 HTML 문서에 <style> 태그 안에 스타일을 적용하는 방식 입니다. 
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
## 식별자
   
### class
- 스타일 변경만을 위해서 사용되는 속성
- 똑같은 태그가 있을때 구별하기 위한 식별자가 된다.
```html
<!DOCTYPE html>
	<html>
		<head>
			<meta charset="UTF-8">
			<title>class를 통한 css 적용</title>
			<style type="text/css">
			   p{color:#f00; }
			
			   /* class의 개념은 똑같은 태그가 있을때 구별하기 위한 식별자가 된다. */
			h2.text{color:#f00;}
            
         	 	/* p태그중에서 text라는 클래스 이름을 가진것에만 css를 적용해줘
			같은 클래스 이름을 갖고 있어도 좀더 세밀하게 설정할 수 있다.
			주의! 중간에 띄어쓰기를 넣지 말 것. */
			p.text{ color:#00f; }
            
         		/* .class명 <-- 특정클래스로 접근할 수 있다.*/
			.text2{ color:#0f0;}
			</style>
		</head>
		
		<body>
			<!-- class의 개념은 똑같은 태그가 있을때 구별하기 위한 식별자가 된다. -->
			<h2 class="text">클래스 연습중</h2>
			
			<p class="text2">
				힘든건 사랑이 아니다
			</p>
			
			<p class="text">
				내가 널 떠났어야 했는데
			</p>	
		</body>
	</html>
```
      
- 같은 문서에 CSS가 HTML 요소로 함께 존재하게 되는 형태입니다. 
- 여러페이지에 공통 스타일이 있는 경우 중복해서 작성해야 하는 불편함이 있고, 중복된 소스가 분산되어 산재하므로  유지 보수에 어려움이 따를 수 있습니다.
      
### 클래스 활용
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
- id의 경우 나중에 서버로 데이터를 넘겨야 하는 경우도 있기 때문에 중복이 되면 혼란을 야기할 수 있다.
```
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
## 테두리(border)
```
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
```
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
```
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
```
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

    
 ### 외부 파일로 적용하는 방법
- CSS를 외부 파일로 따로 분리하여 작성하는 방식 입니다.
```
<link rel=”stylesheet” type=”text/css” href='css 외부 파일 경로'>
```
```
예) 
<!DOCTYPE html>
   <html>
     <head>
       <meta charset='utf-8'>
       <link rel=”stylesheet” type=”text/css” href=”css/style/.css”>
     </head>
     <body>
       <p>빨간색 글씨</p>
     </body>
  </html> 
```
## dl CSS 응용 실습
- 아래와 같은 모습 만들어보기

![image](https://user-images.githubusercontent.com/54658614/227429600-87563155-f888-4604-b2ee-7f405a75e832.png)

    - 우리가 margin이나 padding을 주지 않아도 어느정도 값을 갖고있는 태그들이 있다.
    
![image](https://user-images.githubusercontent.com/54658614/227430215-3fd3cb4b-b965-4577-b9fc-a669704b47a1.png)


```
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
```
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

```
p{border : 1px solid black;
  width:300px;
  height:300px;
  background:yellow url(image/backgroundImage.gif) repeat-x;} 
```
![image](https://user-images.githubusercontent.com/54658614/227841459-c959f15c-a20a-4294-815c-449fb41ce459.png)
  
### 이미지 위치 지정하기
```
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
```
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
```
<!DOCTYPE html>
	<html>
		<head>
			<meta charset="UTF-8">
			<title>후손선택자</title>
			<style type="text/css">
			
			/*  > : 자식선택자는 직계자식에게만 속성을 적용한다. */
			#header > h1{ color:red;}
			#contents > h1{ color:blue;}

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
```
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

## 인라인요소 -\> 블록요소
```
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
```
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

```
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
```
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
```
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
```
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
```
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

## float
- float는 원래 '뜨다'라는 의미이다. 웹페이지에서 이미지를 어떻게 띄워서 텍스트와 함께 배치할 것인가에 대한 속성
- inherit : 부모 요소에서 상속
- left : 외쪽에 부유하는 블록 박스를 생성
- right : 오른쪽에 부유하는 블록 박스를 생성
- none : 요소를 부유시키지 않음
```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>float(왼쪽, 오른쪽 정렬)</title>
		
		<style type="text/css">
			*{margin:0;padding:0;}
			
			#container{ margin:10px auto; /*좌우를 auto로 결정하면 가운데로 알아서 정렬이 된다  */
						width:500px;
						border:5px solid #caa;
						padding:10px; /* 너비를 유지하면서 padding이 적용되기 때문에 실제 크기는 500px보다 클 수 있다. */
						background: #fdd;
						overflow:hidden;}/* 자식태그가 float속성을 가지고 있다면 그 부모는 대부분 overflow:hidden을 가지고 있어야한다. */
						
			h1 {text-align: center;}
			
			/* float : 블록요소의 객체를 왼쪽이나 오른쪽으로 나란히 정렬하기 위한 속성
			float은 '부유하다'의 의미를 가지고 있으므로, 부모영역에 영향을 받지 않고 공중에 떠오른 형태가 된다. */
			
			.div1 { width:200px;
				   height:100px;
				   padding:15px;
				   background:#dda;
				   float:left;}
				   
			.div2 { width:200px;
				   height:100px;
				   padding:15px;
				   background:#ba7;
				   float:right;}
		</style>
	</head>
	
	<body>
		<div id="container">
			<h1>float연습</h1>
			
			<div class="div1"><p>float을 통한 정렬 예제</p></div>
			<div class="div2"><p>float을 통한 정렬 예제</p></div>
		</div>
	</body>
</html>
```
![image](https://user-images.githubusercontent.com/54658614/228128504-69c6961d-eb32-4135-b992-2252e2ec08cf.png)

## 외부 스타일 시트
- 웹사이트에 공통적으로 적용해야 하는 스타일이 있다면 하나의 파일을 만들어 한번만 정의해놓고 사용하는 방식입니다.
- 외부에 작성된 이러한 스타일 시트는 .CSS확장자를 사용하여 저장됩니다.
- 스타일을 적용할 웹 페이지의 \<head\>태그에 \<link\>태그를 사용하여 외부 스타일 시트를 포함해야만 스타일이 적용됩니다

```
<!DOCTYPE html>
	<html>
		<head>
			<meta charset="UTF-8">
			<title>외부스타일시트 참조하기</title>
		</head>
		
		<body>
			<h1>외부 스타일시트 연습중</h1>
			<p>안녕하세요</p>
			
			<ul>
				<li>메뉴1</li>
				<li>메뉴2</li>
				<li>메뉴3</li>
			</ul>
			<a href="#">a링크</a>
			
			<table border="1">
				<tr>
					<td>테이블1</td>
					<td>테이블2</td>
				</tr>
				<tr>
					<td>테이블3</td>
					<td>테이블4</td>
				</tr>
			</table>
		</body>	
	</html>
```

WebContent 폴더 아래에 css폴더를 하나 만들자

![image](https://user-images.githubusercontent.com/54658614/228129585-39062a4f-50bc-4d6d-8202-f27d2a1dc1c6.png)
 
폴더에서 우클릭하여 CSS파일을 만들어야 한다 없으면 other로 들어가서 만들자.

![image](https://user-images.githubusercontent.com/54658614/228129728-1c8493e7-e2de-428f-876b-f1387aa524e2.png)

![image](https://user-images.githubusercontent.com/54658614/228129826-35a72044-ca7b-4169-8da9-d146875b5300.png)

```
@charset "UTF-8";

*{padding:0; margin:0;}
a{text-decoration:none;}
li{list-style:none;}
table{border-collapse:collapse;}/*테이블 테두리를 한겹으로*/
```
외부 스타일 시트 적용하기
```
<!DOCTYPE html>
	<html>
		<head>
			<meta charset="UTF-8">
			<title>외부스타일시트 참조하기</title>
			
			<link rel="stylesheet" href="css/reset.css">
		</head>
		
		<body>
			<h1>외부 스타일시트 연습중</h1>
			<p>안녕하세요</p>
			
			<ul>
				<li>메뉴1</li>
				<li>메뉴2</li>
				<li>메뉴3</li>
			</ul>
			<a href="#">a링크</a>
			
			<table border="1">
				<tr>
					<td>테이블1</td>
					<td>테이블2</td>
				</tr>
				<tr>
					<td>테이블3</td>
					<td>테이블4</td>
				</tr>
			</table>
		</body>	
	</html>
```

![image](https://user-images.githubusercontent.com/54658614/228130457-65872c6d-6272-4452-b7e3-5b1be5461c5c.png)

## position
```
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

## layout
```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>레이아웃 만들기</title>
		<link rel="stylesheet" href="css/reset.css"/>
		<style type="text/css">
		
			#a1{margin-left:10px;}
			
			ul{ overflow : hidden;}
			li{ float : left}
			
			li{ border-right : 1px solid gray;
			    border-top : 1px solid gray;
			    border-bottom: 1px solid gray;}
			    
			  a{display : block;
			    padding : 2px 10px;}
			  
			  li:nth-child(1){border-left:1px solid gray;}
			

		</style>
	</head>
	
	<body>
			<div id="a1">
				<ul>
					<li><a href="#">HTML</a></li>
					<li><a href="#">CSS</a></li>
					<li><a href="#">JavaScript</a></li>
					<li><a href="#">JQuery</a></li>
					<li><a href="#">Jsp</a></li>
				</ul>
			</div>

	
	</body>
</html>
```
![image](https://user-images.githubusercontent.com/54658614/228135131-3d805920-7c10-4a63-aaa2-4d787b05daa5.png)

