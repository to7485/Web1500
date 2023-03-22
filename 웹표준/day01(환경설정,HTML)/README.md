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
