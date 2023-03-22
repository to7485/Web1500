# 실습 환경 구축하기


## HTML

### 정의
- HTML은 Hyper Text Markup Language의 약자입니다.
- HTML은 웹페이지를 만드는 대표적인 마크업 언어입니다
- HTML은 웹페이지의 구조를 표현합니다.
- HTML은 여러 요소로 구성되어 있습니다
- HTML은 브라우저에 어떻게 내용을 표시할지 알려주는 역할을 합니다.

### 기본 구조
```
<!DOCTYPE html>
<html>
  <head>
    <title>Page Title</title>
  </head>
  <body>
    <h1>My First Heading</h1>
    <p>My first paragraph.</p>
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
