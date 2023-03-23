# 실습 환경 구축하기

1. WEB1500 폴더 아래 web 폴더 생성후 안에 util과 work폴더 만들기

![image](https://user-images.githubusercontent.com/54658614/226801868-03bcf23c-42d1-461f-892c-63e71c3668b2.png)

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

#### 구조
- 당분간 src는 사용할 일이 없을것이다.

![image](https://user-images.githubusercontent.com/54658614/226806767-e6c537e0-c70a-4e47-9036-b8cb89b6a14f.png)

#### html 파일 생성
- WebContent 바로 아래 만들어야 한다. META-INF,WEB-INF아래에 만들지 말것

![image](https://user-images.githubusercontent.com/54658614/226807075-bce4e9ec-05c3-4fbb-be78-69d3f3ded79e.png)

파일 이름 정하고 finish

![image](https://user-images.githubusercontent.com/54658614/226807216-138da8b7-3bf1-4189-87d7-b11a7272ab71.png)


## HTML

## HTML 주석(Comments)
- 주석은 브라우저에서 출력이 되지 않는 설명문장 입니다. 
- 소스코드에 대한 설명을 할때 사용합니다.
- \<!-- 로 시작해서 --\>로 마무리 합니다.
- 주석은 여러줄 입력 할수 있습니다.

```
예)
<!DOCTYPE html>
<html>
	<head>
	<!-- Hyper Text Markup Language
	웹 브라우저의 디자인을 위해 고안된 언어 -->
		<meta charset="UTF-8">
		<title>웹표준의 시작</title>
	</head>
	
	<body>
	<!-- body태그에 작성된 내용이 실제 웹페이지에 표시되는 내용 -->
		안녕하세요
	</body>
</html>
```

### 정의
- HTML은 Hyper Text Markup Language의 약자입니다.
- HTML은 웹페이지를 만드는 대표적인 마크업 언어입니다
- HTML은 웹페이지의 구조를 표현합니다.
- HTML은 여러 요소로 구성되어 있습니다
- HTML은 브라우저에 어떻게 내용을 표시할지 알려주는 역할을 합니다.

### 기본 구조

![image](https://user-images.githubusercontent.com/54658614/226808086-fb78d205-363b-4f50-a8b3-739b11d256b3.png)


```
<!DOCTYPE html>
<html>
  <head>
    <title>Page Title</title>
  </head>
  <body>

  </body>
</html>
```

### 설명
- \<!DOCTYPE html\> 
	- HTML5 문서임을 선언합니다. 
	- doctyle에 따라 HTML 종류와 버전이 결정되며 그에 따라 브라우저에서 각 요소 표현 방식이나 호환되는 자바스크립트, CSS 적용 방식이 달라질 수 있습니다.
	- 현재는 거의 모든 브라우저가 HTML5를 지원하므로 HTML5 문서로만 지정해서 사용할 수 있도록 합니다.

	- 문서에서 한번 만 정의되고 페이지의 가장 상단에 위치합니다.
	- DOCTYPE -> doctype과 같이 대소문자 구분 없이 정의해도 인식을 합니다.

- \<html\> ~ \</html\>
	- HTML페이지에서 root 요소입니다(가장 상위 요소)

- \<head\>~\</head\> 
	- 주로 HTML페이지에서 meta 정보를 포함합니다
	- 예) \<meta charset=’utf-8’\>

	- 또한 초기 페이지 렌더링시에 불러와야 할 외부 링크를 정의합니다.(css, javascript)
	```
	<link rel="stylesheet" type="text/css" href="style.css">
	<script src="common.js"></script>
	```

- \<title\>~\</title\>
	- HTML 페이지의 제목을 나타냅니다. 
	- 브라우저 상단의 웹페이지 탭에 제목으로 노출이 됩니다. 


- \<body\>~\</body\>
	- HTML문서에서 실질적으로 보이는 영역을 정의하는 구간입니다.
	- 예) 이미지, 본문내용, 링크, 테이블, 제목 등등


## HTML 요소 

![image](https://user-images.githubusercontent.com/54658614/226809179-689d77ca-eca9-4989-8b6c-c8888bdcfc9a.png)


- HTML 요소는 일반적으로 태그라는 명칭이 익숙할 수 있습니다.
-  HTML 요소는 일반적으로 시작 태그와 닫힘 태그로 정의가 됩니다. 다만 시작 태그와 닫힘태그가 없는 유일한 태그도 있습니다.
```
   (예 - 줄바꿈 태그 <br> 또는 <br />)
   <태그> 내용 </태그>
```

## HTML 속성(attributes)
- 모든 HTML 요소들은 속성(attributes)를 가지고 있습니다. 
- 속성(attributes)는 HTML 요소에 대한 추가적인 정보를 제공 합니다.
- 특정 태그에서의 속성이나 사전 정의된 속성은 적용된 기능으로서 추가정보를 활용합니다.

- 예) 특정기능으로써 속성 예시 a태그, input 태그, img 태그
```
<a href='https://www.naver.com' target='_blank'>네이버</a>
<input type='text' name='subject'>
<img src='이미지 경로'>
```

- 예) 추가적인 정보로써 속성 예시
```
<div data-sido='인천광역시'>인천광역시</div>
```

#### HTML 속성 예시
- href  - 이동할 페이지 링크를 지정할 수 있습니다.
```
<a href='https://www.naver.com'>네이버</a>
```

s- rc - 이미지 경로를 지정할 수 있습니다.
```
<img src='photo.jpg'>
```
- width, height - 요소의 너비, 높이를 지정합니다.
```
<img src='photo.jpg' width='500' height='600'>
```

- alt -이미지 대체 문구(이미지가 노출이 되지 않는 경우에 대체 노출되는 문구)
```
<img src='photo.jpg' alt='배경사진'>
```

- style - 요소에 직접 스타일을 지정할 수 있습니다(CSS 인라인 형태)
```
<p style='color: red;'>빨간색 글씨</p>
```
- lang - 웹페이지의 언어를 선언할 수 있습니다. 다만 <html>태그에만 지정할 수 있습니다.
```
<html lang="ko">
```
- title - 툴팁 형태로 노출되는 추가 정보를 지정할 수 있습니다.
```
<p title='툴팁 메세지로 노출됩니다.'>안녕하세요</p>
```
	
### HTML 속성 권장사항
- 소문자로 사용
- 속성은 소문자, 대문자 상관 없이 인식을 하나 소문자로 쓰는것을 권장합니다(W3C 권장사항)
- (title, TITLE 상관없이 인식 가능)
		
## HTML의 태그들

### HTML 헤더(Headings)태그

### 정의
- 헤더는 글의 제목이나 부제목을 표기할 때 사용합니다.
- 태그는 h1~h6까지 있으며, 숫자가 작을수록 크기가 큽니다.

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>heading tag</title>
	</head>
	
		<body>
			<!--h1이 32px
			일반적으로 1에서 3정도 까지 주로 사용한다.  -->
			<h1>Heading 1 tag </h1>
			<h2>Heading 2 tag </h2>
			<h3>Heading 3 tag </h3>
			<h4>Heading 4 tag </h4>
			<h5>Heading 5 tag </h5>
			<h6>Heading 6 tag </h6>	
		</body>
</html>	
```
### 주의사항
- h1~6 태그의 용도로 헤더 태그를 사용하여야 하며 단순히 글자 크기를 크게하거나(font-size) 강조(bold)표기 용도로 사용하지 말아야 합니다.

## HTML Block & Inline 요소
- 모든 HTML 요소들은 각 태그(요소)에 따른 기본 출력 값(display value)를 가지고 있습니다.

- 출력 값은 block과 inline이 있습니다.

### Block-level 요소
- 항상 줄개행을 합니다.
- 공간을 지정할 수 있습니다. 즉, width, height(너비와 높이)를 가질 수 있습니다.(CSS에서 지정)
- 아래 위 또는 왼쪽 오른쪽에 공백(margin)을 지정할 수 있습니다.
- 대표적으로 \<div\> 태그는 block-level 요소 입니다.
- block-level 태그(요소) 
```
<address>
<article>
<aside>
<blockquote>
<canvas>
<dd>
<div>
<dl>
<dt>
<fieldset>
<figcaption>
<figure>
<footer>
<form>
<h1>
<h6>
<header>
<hr>
<li>
<main>
<nav>
<noscript>
<ol>
<p>
<pre>
<section>
<table>
<tfoot>
<ul>
<video>
```

### Inline-level 요소
- 줄개행을 하지 않습니다.
- 공간을 지정할 수 없습니다. 요소 안에 있는 내용만큼의 공간만 차지합니다.
- 위 아래 공백(margin)을 지정할 수 없으나, 내부 공백(padding)은 지정할 수 있습니다.
- 대표적으로 \<span\>태그는 inline-level 요소 입니다.
```
<a>
<abbr>
<acronym>
<b>
<bdo>
<big>
<br>
<button>
<cite>
<code>
<dfn>
<em>
<i>
<img>
<input>
<kbd>
<label>
<map>
<object>
<output>
<q>
<samp>
<script>
<select>
<small>
<span>
<strong>
<sub>
<sup>
<textarea>
<time>
<tt>
<var>
```

#### block, inline 태그 실습

### HTML 문단(Paragrahs) 태그
- 하나의 문단을 표기 표기하는 용도로 사용하며 \<p\>~\</p\> 형태로 사용합니다.
- 문단을 나누는 용도로 사용하는 태그이므로 \<p\> 태그 전 후로 공백이 추가 됩니다.

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>블록요소와 인라인 요소</title>
	</head>
	
	<body>
		안녕하세요
		반갑습니다.
		저		는 홍길동		입니다.
		
		<!--블록요소는 현재 브라우저의 너비만큼 너비를 가득 채우는 영역을 잡고 있다.  -->
		<p><b>단락</b>을 구별하는 p태그는 블록요소입니다.</p>
		
		<!--strong을 쓰는게 권장 사항  -->
		<p><strong>단락</strong>과 단락 사이는 마진이라는 여백을 통해서 구별되고 있다.</p>
		<p>
			<i>링크태그</i>
			<!--i 와 em 둘다 기울여 쓰기 이지만 em을 권장한다.  -->
			<em>링크태그2</em>
			<!--<a>태그는 인라인 요소 : 내가 가지고 있는 내용만큼만 요소를 잡는다  -->
			<a href="https://www.naver.com/">네이버</a>로가기
		</p>
		<p>
			<a href="ex01_first.html"> ex01로 이동</a>
			<a href="#"> 페이지4로 이동</a>
			
			<!--수평선 태그 : 주로 주제별로 단락을 구별하는 용도로 사용  -->
			<hr>
			
			<!-- br은 강제개행 태그  -->
			첫번째 줄<br>
			두번째 줄<br>
			세번째 줄<br>
		</p>
	</body>
</html>
```

## HTML 서식(Text Formatting) 태그
- 텍스트 서식을 표현할수 있는 태그

- \<b\>    굵은 텍스트 정의
- \<em\>  강조된 텍스트 정의
- \<i\>     기울임 꼴 텍스트 정의
- \<small\>	더 작은 텍스트 정의
- \<strong\> 중요한 텍스트 정의
-\<sub\>	아래 첨자 텍스트 정의
-\<sup\>	윗 첨자 텍스트 정의
-\<ins\>	첨가 텍스트 정의
-\<del\>	지운 텍스트 정의
-\<mark\>	마킹 / 강조된 텍스트 정의

## HTML 링크(Links)
- HTML에서 링크는 하이퍼링크입니다.

- 하이퍼링크란 
- 하이퍼링크는 하이퍼텍스트 문서 안에서 직접 모든 형식의 자료를 연결하고 가리킬 수 있는 참조 고리이다. 이를테면 동영상, 음악, 그림, 프로그램, 파일, 글 등의 특정 위치를 지정할 수 있다. 이는 하이퍼텍스트의 핵심 개념이며, HTML을 비롯한 마크업 언어에서 구현하고 있다. 

- 즉, 하이퍼텍스트 문서는 문서에 다른 문서(다른 HTML)나 이미지, 그림등의 링크를 다수 포함할 수 있고 이동할 수 있는 것을 말하여 그 링크를 하이퍼링크라고 합니다.
 

### href 속성
HTML <a>태그는 하이퍼링크를 정의합니다. a 태그에서 가장 중요한 속성(attribute)는 href 이며 페이지를 이동할 링크(URL)을 지정할 수 있습니다.
```
<a href='https://www.naver.com'>네이버</a>
```

### 경로
- 절대경로
	- 절대 경로란 특정문서 페이지 또는 이미지등 자원에 접근할 수 있는 전체 URL을 의미 합니다.
	- photo.jpg 파일이 서버에서 /web/public/img/photo.jpg에 위치 해 있다면 \<a href='/web/public/img/photo.jpg'\>사진\</a\>과 같이 표현할 수 있습니다.

- 상대경로
	- 서버에서 /web/public/img/photo.jpg에 위치해 있고 현재 html 경로가 /web/public이라면 \<a href='img/photo.jpg'\> 형태로 표현할 수 있습니다.(현재 경로 기준)

	- 상대경로를 표현하는 방식
	- ./ 현재 파일이 열려 있는 경로
	- ../ 현재 파일이 열려 있는 경로보다 1단계 상위 경로
	- ../../ 현재 파일이 열려 있는 경로보다 2단계 상위 경로
	
## HTML 이미지(Images)
- HTML <img>태그는 웹페이지에서 이미지를 표시하기 위해 사용합니다.

프로젝트에 WebContent에 image 폴더 복사해서 넣기

![image](https://user-images.githubusercontent.com/54658614/227101972-884fc663-2d4f-47d6-9940-4c084687c08f.png)


### 필수 속성
\<img\>태그에는 다음 2개의 필수 속성이 있습니다.
- src - 이미지 경로를 지정할수 있습니다.
- alt - 이미지 대체 문구(이미지가 노출이 되지 않는 경우에 대체 노출되는 문구)

### width, height 속성
- 이미지의 너비와 높이를 지정할 수 있습니다. 다만 이미지의 사이즈는 속성으로 지정하기 보다는 CSS Style로 width, height를 지정하는것이 좋습니다.

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>이미지태그</title>
	</head>
		
		<body>
			<!--alt는 이미지에 대한 대체 텍스트, 이미지에 대한 설명  -->
			<!--img태그는 인라인 태그  -->
			<img alt="흙산" src="image/background.jpg">
			<!--title속성은 툴팁(도움말)으로써 마우스 오버시 화면에 표시  -->
			<img src="image/float.jpg" alt="구름떼" title="새털구름">
		</body>
</html>
```
## HTML 테이블(Tables)
- HTML 테이블은 table, tr, th, td, thead, tbody, tfoot 등으로 구성되어 있으며 
- tr은 데이터의 행 td, 
- th는 데이터의 열로 생각할 수 있습니다.
- th는 테이블 헤더로 테이블 각 열을 대표하는 셀
- 또한 테이블 태그는 thead - 헤더영역, tbody - 본문영역, tfoot - 본문영역으로 구분하여 사용할 수 있습니다. 
```
<!DOCTYPE html>
	<html>
		<head>
			<meta charset="UTF-8">
			<title>테이블(표)</title>
		</head>
		
		<body>
			<!-- 1행 1열 -->
			<table border = "1"> <!-- border : 선의 두께 -->
				<tr><!-- 행 -->
					<td>1행 1열</td> <!-- 열 -->
				</tr>
				
				
			</table>
			<hr>
			
			<!-- 1행 2열 -->
			
			<table border = "1"> 
				<tr><!-- 행 -->
					<td>1행 1열</td> 
					<td>1행 2열</td> 
				</tr>	
			</table>
			<hr>
			
			<!-- 2행 1열 -->
			
			<table border = "1"> 
				<tr><!-- 행 -->
					<td>1행 1열</td> <!-- 열 --> 
				</tr>
				<tr><!-- 행 -->
					<td>2행 1열</td>  
				</tr>		
			</table>
			
			<hr>
			
			<table border = "1"> 
				<tr>
					<td>1행 1열</td> 
					<td>1행 2열</td>
				</tr>
				<tr>
					<td>2행 1열</td>
					<td>2행 2열</td>  
				</tr>		
			</table>
		</body>
	</html>
```
테이블 열 합치기
```
<!DOCTYPE html>
	<html>
		<head>
			<meta charset="UTF-8">
			<title>테이블 열 합치기</title>
		</head>
		
		<body>
			<table border = "1">
				<tr>
					<td colspan="3">메뉴판</td>
				</tr>
				
				<tr>
					<td>짜장</td>
					<td>짬뽕</td>
					<td>탕수육</td>
				</tr>
			</table>
		</body>
	</html>
```
테이블 행 합치기

```
<!DOCTYPE html>
	<html>
		<head>
			<meta charset="UTF-8">
			<title>테이블 행 합치기</title>
		</head>
		
		<body>
			<table border="1">
				<tr>
					<td rowspan="2">메뉴판</td>
					<td>짜장</td>
				</tr>
					
					<td>짬뽕</td>
				<tr>
				
				</tr>
			</table>
		</body>
	</html>
```
## HTML 리스트(Lists)태그

- 순서없는 리스트(Unordered HTML List)
- 순서 없는 리스트는 <ul>태그로 시작하며 리스트 항목들은 <li>~</li> 태그를 사용합니다.

```
예)
<ul>
	<li>홍길동</li>
	<li>김길동</li>
	<li>마루치</li>
</ul>


<ul>
	<li>
	<a href="#">링크 테스트</a>
	</li>
	<li>
	<img alt="스마일" src="image/acid2Test.jpg">
	</li>
	<li>일반 텍스트</li>

</ul>
```

- 리스트 구분 기본 값은 disc로 색이 채워진 둥근 점 모양입니다.
- 리스트 구분 값은 css의 list-style-type으로 지정할 수 있습니다.
	- disc - 기본 값, 채워진 둥근 점
	- circle - 책이 미워진 둥근 점
	- square - 사각형 모양 점
	- none - 구분값 없음

- 적용방식
```
<ul style="list-style-type:disc 또는 circle, square, none 중 하나 입력">
```

### 순서 있는 리스트(Ordered HTML List)
순서 있는 리스트는 \<ol\>태그로 시작하며 리스트 항목들은 \<li\>~\</li\> 태그를 사용합니다.

- 리스트 구분 값은 순서가 있는 숫자나 문자로 표현이 되며 다음과 같은 타입으로 지정하실 수 있습니다.

- \1 - 기본값이며 숫자 순서로 표시됩니다.
- A - 대문자 알파벳 순서로 표시됩니다.
- a - 소문자 알파벳 순서로 표시됩니다.I - 대문자 로마 숫자 형식으로 표시됩니다.
- i - 소문자 로마 숫자 형식으로 표시됩니다.

- 적용방식
```
<ol type="1 또는 A, a, I, i 중 하나 입력">
```

- 시작번호 지정할 경우 start="시작번호"로 지정하며 숫자를 변경할 경우 \<li value="변경숫자"\>로 입력합니다.

```
예) 
<!-- ol(Ordered List) : 순위, 게시판 등의 순서목록 -->
<ol>
	<li>임창정</li>
	<li>힘든건 사랑이 아니다</li>
	<li>노래 짱좋아</li>
</ol>
```

#### 깊이가 있는 리스트
```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>깊이를 가지는 목록들</title>
	</head>
	
	<body>
		<ul>
			<li> 
				HTML
				<ul>
					<li>마루치</li>
					<li>아라치</li>
					
					<li>
						파란해골13호
						<ul>
							<li>14호</li>
							<li>15호</li>
						</ul>
					</li>
				</ul>
			</li>
		</ul>
		
		<ol>
			<li>나는 OL이다.</li>
			<li>
				OL의 하위 요소
				<ol>
					<li>날슈</li>
					<li>달하</li>
				</ol>
			</li>
		</ol>
	</body>
</html>
```
#### 깊이가 있는 리스트 실습
```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>깊이가 있는 ul 문제</title>
	</head>
	
	<body>
		<ul>
			<li><a href="#">개인뱅킹</a>
				<ul>
					<li><a href="#">조회</a></li>
					<li><a href="#">이체</a></li>
					<li><a href="#">신규/해지</a></li>
					<li><a href="#">공과금/법원</a></li>
					<li><a href="#">뱅킹보안센터</a></li>
				</ul>			
			</li>
			<li><a href="#">자산관리</a>
				<ul>
					<li><a href="#">나의지출</a></li>
					<li><a href="#">이체</a></li>
					<li><a href="#">신규/해지</a></li>
					<li><a href="#">공과금/법원</a></li>
				</ul>
			</li>
			<li><a href="#">예금/신탁</a></li>
			<li><a href="#">대출</a></li>
			<li><a href="#">펀드</a></li>
			<li><a href="#">외환</a></li>
		</ul>
	</body>
</html>
```

### 설명 리스트(Description List)
- 용어에 대한 설명을 위한 구조로 구성되어 있는 리스트 입니다.
- \<dl\>~\</dl\> 태그이며 하나의 행을 구성합니다. 
- 각 행은 \<dt\>~\</dt\>(항목명)와 \<dd\>~\</dd\>(항목 설명)으로 구성되어 있습니다.

```
예)
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>dl(정의목록)</title>
	</head>
	
	<body>
		<!-- dl은 ul, ol과 같이 목록화 한 내용을 표기하지만
			일반적으로 사전적인 명확한 뜻을 가지고 있는 것들 위주로 사용하는 편(영단어) -->
		<dl>
			<dt>Bear</dt>
			<dd>곰</dd>
			<dd>참다,견디다</dd>
		</dl>
	</body>
</html>
```	
### \<div\>태그
- div 태그는 Division의 약자로, 레이아웃을 나누는데 주로 쓰입니다.
- 다른 태그와 다르게 특별한 기능을 갖고 있지는 않지만, 가상의 레이아웃을 설계하는데 쓰이며 주로 CSS와 연동하여 쓰입니다.

#### div 실습
```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>div(영역지정 태그)</title>
	</head>
	
	<body>
		<!-- div : 특정영역별로 묶어서 표시되어야 할 태그들을 하나의 영역으로
			합쳐서 관리할 수 있도록 해 주는 태그 -->
		<div>
			<a href="#">메뉴1</a>
			<a href="#">메뉴2</a>
			<a href="#">메뉴3</a>
		</div>
		
		<div>
			<a href="#">메뉴4</a>
			<a href="#">메뉴5</a>
			<a href="#">메뉴6</a>
		</div>
			<p>나는 p태그다</p>
			<p>나는 p태그다2</p>
	</body>
</html>
```
	
	

