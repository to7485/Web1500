# 실습 환경 구축하기

1. WEB1500 폴더 아래 JSP 폴더 생성후 안에 util과 work폴더 만들기

![image](https://user-images.githubusercontent.com/54658614/230702953-9d8cc484-f186-4dfb-b6e1-bf43cb5134ab.png)

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

## JSP 프로젝트 폴더 구조

![image](image/webcontent.png)

- **webapp** : HTML(.html), JSP, Javascript(.js), CSS(.css) 및 이미지와 같은 파일들이 위치한다. 해당 위치에 놓이는 파일들은 웹 애플리케이션이 배치 될 떄 그대로 옮겨진다.

- **WEB-INF** : 웹 애플리케이션의 설정 파일들이 해당 디렉토리 아래에 포함된다. WEB-INF 내에 있는 파일들은 클라이언트에서 요청할 수 없다. 따라서 html, js와 같은 정적 자원은 바로 읽을 수 없다.

- **WEB-INF/lib** : 애플리케이션 실행에 필요한 라이브러리 즉, jar 파일들을 모아두는 디렉토리이다. jar은 자바 아카이브 파일이란 의미로 java + ARchive의 합성어로 jar로 사용된다.


- WebContent에서 HTML파일이 아닌 JSP파일을 만든다.

![image](https://user-images.githubusercontent.com/54658614/230703065-42ef6943-1e5a-4176-9a52-c54664c3706a.png)

<hr>

# 웹 기초

## request와 response의 이해
- 서버는 클라이언트가 있기 때문에 동작을 한다. 서버로 요청(request)를 보내고, 서버에서는 요청의 내용을 읽고 처리한 뒤 클라이언트에 응답(response)을 보냅니다.
- 따라서 서버에는 요청을 받는 부분과 응답을 보내는 부분이 있어야 한다. request와 response은 이벤트 방식이라고 생각할 수 있다.

## HTTP
- HTTP는 HTML문서와 같은 리소스를 가져올 수 있도록 해주는 프로토콜(protocol)이다.
- HTTP는 웹에서 이루어지는 모든 데이터 교환의 기초이며, 클라이언트-서버 프로토콜이기도 하다.
- 클라이언트와 서버들은 개별적인 베세지 교환에 의해 통신한다. 보통 브라우저인 클라이언트에 의해 전송되는 메세지를 요청(request)라고 부르며, 서버에서 응답으로 전송되는 메세지를 응답(response)라고 부른다.

## 헤더(Header)
- HTTP헤더는 클라이언트와 서버가 요청 또는 응답으로 부가적인 정보를 전송할 수 있도록 해준다.
- HTTP헤더는 대소문자를 구분하지 않는 이름과 콜론':' 다음에 오는 값으로 이루어져있다. 값 앞에 붙은 빈 문자열은 무시된다.

### 일반 헤더(General header)
- request와 response모두에 적용되지만 body에서 최종적으로 전송되는 데이터와는 관련이 없는 헤더

### 요청 헤더(Request header)
- 패치될 resource나 클라이언트 자체에 대한 자세한 정보를 포함하는 헤더

### 응답 헤더(Response header)
- 위치 또는 서버 자체에 대한 정보(이름,버전 등)와 같이 응답에 대한 부가적인 정보를 갖는 헤더

### 엔티티 헤더(Entity header)
- 컨텐츠 길이나 MIME 타입과 같이 엔티티 바디에 대한 정보를 포함하는 헤더

![image](image/http_protocol.png)

- request와 response는 모두 header와 body를 가지고 있다.
- header는 request또는 response에 대한 정보를 가지고 있는곳
- body는 server와 client간에 주고받을 실제 데이터를 담아두는 공간이다.
- 개발자 도구의 Network탭에서 요청 중 하나를 클릭해보면 더 상세하게 request와 response을 살펴볼수 있다.

![image](image/headers.png)

- General은 공통 헤더이고, Request Headers는 요청 헤더, Response Headers는 응답헤더이다.
- Request Payload가 요청의 본문이다. 응답의 본문은 Preview나 Response탭에서 확인할 수 있다.

### HTTP 상태 코드
브라우저는 서버에 보내주는 상태 코드를 보고 request가 성공했는지 실패했는지를 판단한다.
- 2XX : 성공을 알리는 상태코드이다. 대표적으로 200(성공), 201(작성됨)이 많이 사용된다.
- 3XX : 리다이렉션(다른 페이지로 이동)을 알리는 상태코드이다. 어떤 주소를 입력했는데 다른 주소의 페이지로 넘어갈 때 이 코드가 사용된다. 대표적으로 301(영구이동),302(임시이동)이 있다. 304(수정되지 않음)는 요청의 응답으로 캐시를 사용했다는 뜻이다.
- 4XX : 요청 오류를 나타낸다. 요청 자체에 오류가 있을 때 표시된다. 대표적으로 400(잘못된 요청), 401(권한 없음), 403(금지됨), 404(찾을 수 없음)가 있다.
- 5XX : 서버오류를 나타낸다. 요청은 제대로 왔지만 서버에 오류가 생겼을 때 발생한다. 이 오류가 뜨지 않게 주의해서 프로그래밍 해야 한다. 500(내부 서버 오류), 501(불량 게이트웨이), 503(서비스를 사용할 수 없음)이 자주 사용된다.

## HTTP 요청 메서드
클라이언트가 서버에 데이터를 전송하여 응답을 얻고자 할 때 사용하는 방식
- GET : 서버 자원을 가져오고자 할 때 사용한다. 요청의 본문에 데이터를 넣지 않는다. 데이터를 서버로 보내야 한다면 쿼리스트링을 사용한다.
- POST : 서버에 자원을 새로 등록하고자 할 때 사용한다. 요청의 본문에 새로 등록할 데이터를 넣어 보낸다.
- PUT : 서버의 자원을 요청에 들어 있는 자원으로 치환하고자 할 때 사용한다. 요청의 본문에 치환할 데이터를 넣어 보낸다.
- PATCH : 서버 자원을 삭제하고자 할 때 사용한다. 요청의 본문에 데이터를 넣지 안흔ㄴ다.
- OPTIONS : 요청을 하기 전에 통신 옵션을 설명하기 위해 사용한다.

# 서블릿(Servlet)
- Servlet이란 자바를 기반으로 하는 웹 어플리케이션 프로그래밍 기술이다.

## Servlet의 역사
- 자바(JAVA) 언어를 개발한 Sun에서 웹 개발을 위해 만들었다.
- 그래서 자바 클래스 형태로 웹 애플리케이션을 작성하는데 그래서 .java가 확장자이다.
- 그렇게 작성된 클래스를 서블릿 클래스라고 한다.
- 서블릿(Servlet)은 JAVA 코드를 작성하고 나서 실행하면 클래스파일(.class)을 만들게 된다.
- 서블릿의 단점은 JAVA코드가 한줄만 변경되어도 다시 처음부터 실행해야 한다.

[출처] Servlet/JSP :: Servlet(서블릿)이란? JSP란? |작성자 Showshine

![image](https://user-images.githubusercontent.com/54658614/231054942-f29baabf-1500-48cb-96f4-7898dc4814b6.png)

출처 : Servlet Architecture (출처 : https://www.geeksforgeeks.org/servlet-architecture/ )

## Servlet의 개요
- 서블릿 기술에서 웹 어플리케이션을 구현하기 위해 작성해야 하는 코드는 'Servlet class'입니다.
- 이 클래스는 클래스 상태 그대로 실행되는 것이 아니라 '서블릿'으로 만들어진 다음에 실행이된다.
- 즉 서블릿은 서블릿 클래스로부터 만들어진 객체이다.
- 하지만 모든 서블릿 객체가 서블릿이라고 할 수는 없다. 웹 컨테이너는 서블릿 클래스를 가지고 서블릿 객체를 만든 다음에 그 객체를 초기화 하여 웹 서비스를 할 수 있는 상태로 만드는데 이런 작업을 거친 서블릿 객체만 서블릿이라고 할 수 있다.