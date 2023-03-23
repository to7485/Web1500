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
- id의 경우 나중에 서버로 데이터를 넘겨야 하는 경우도 있기 때문에 혼란을 야기할 수 있다.
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
			.box7{ border-style:groove;} /* 두께감을 가지는 테두리 */
			.box8{ border-style:ridge;}
			.box9{ border-style:inset;} /* 두께감은 없지만 그림자는 있음 */
			
			.box10{ border:20px outset red;}  /* 한번에 스타일주기 (두께, 스타일, 색깔)  */
			
			.div1{ border:20px green solid;
				width:200px;
				height:100px;}
			
			.div2{ border-bottom:10px solid blue;
				   border-right:10px solid blue;}
			
			/* 상,우,하,좌 순으로 각각 다른 두께의 테두리를 주고 싶다면 반드시 border-로 시작하는 속성을 사용해야 한다. */
			.div3{ border-width:10px 20px 30px 40px;
			       border-style:solid;
			       border-color:green;}
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
	</body>
	</html>
```
![image](https://user-images.githubusercontent.com/54658614/227111864-c03a45d0-44c9-4066-a2cf-70c9c9091afe.png)
      
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
  
  
  
  
  
  
  
  
  
  
