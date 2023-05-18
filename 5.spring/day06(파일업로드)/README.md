# 파일 업로드하기

## Ex_날짜_SpringFileUpload 프로젝트 생성하기
1. pom.xml(경험상 안에 있는 내용만 긁어서 복사해 오는것이 오류가 안남) , resources에 있는 패키지 복사해오기 
  - artifactId,Name 바꿔주기
2. Context_3_dao나 ServletContext에 만들어져있는 객체 지우기
3. Context파일 하나 복사해서 Context_4_fileupload.java 만들기

### mvnrepository.com에서 fileupload를 위한 라이브러리 다운로드받기
- pom.xml에 추가하기

1. Apache Commons FileUpload

![image](https://github.com/to7485/Web1500/assets/54658614/6e65e5fd-1261-4969-ba75-281c8001efd0)

```
<!-- https://mvnrepository.com/artifact/commons-fileupload/commons-fileupload -->
<dependency>
    <groupId>commons-fileupload</groupId>
    <artifactId>commons-fileupload</artifactId>
    <version>1.3.1</version>
</dependency>

```

2. Apache Commons IO

![image](https://github.com/to7485/Web1500/assets/54658614/5dfb2388-4b8c-4717-b432-306be98003e4)

```
<!-- https://mvnrepository.com/artifact/commons-io/commons-io -->
<dependency>
    <groupId>commons-io</groupId>
    <artifactId>commons-io</artifactId>
    <version>2.4</version>
</dependency>
```

## Context_4_fileupload.java에 객체 생성하기
```
package context;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.multipart.commons.CommonsMultipartResolver;



@Configuration
public class Context_4_fileupload {
	
	//xml로 만드려면 패키지까지 다 적었어야 함 ㅋㅋ
	//org.springframework.web.multipart.commons.CommonsMultipartResolver 이렇게
	
	@Bean
	public CommonsMultipartResolver multipartResolver() {
		CommonsMultipartResolver multipartResolver = new CommonsMultipartResolver();
		multipartResolver.setDefaultEncoding("utf-8");
		
		//최대 업로드 용량을 약 10mb로 지정함
		multipartResolver.setMaxUploadSize(10485760);
		return multipartResolver;
	}
}

```
## WebInitializer에 Context_4번 등록하기
```
@Override
protected Class<?>[] getRootConfigClasses() {
	return new Class[] { 
			Context_1_dataSource.class,
			Context_2_mybatis.class,
			Context_3_dao.class,
			Context_4_fileupload.class };
}
```


## FileUploadController 클래스 생성하기
```
package com.korea.fileupload;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class FileUploadController {

	static final String VIEW_PATH = "/WEB-INF/views/";
	
	@RequestMapping(value= {"/","/insert_form.do"})
	public String insert_form() {
		return VIEW_PATH+"insert_form.jsp";
	}
}
```
## ServletContext에 자동탐색으로 객체를 만들자
- 당장 db를 사용할것이 아니기 때문에 DAO를 받을일이 없다.

```
package mvc;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.EnableWebMvc;
import org.springframework.web.servlet.config.annotation.ResourceHandlerRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
@EnableWebMvc
@ComponentScan("com.korea.fileupload")
//어노테이션에도 상속관계가 있다
/*
 *@Component
 *	ㄴ@Controller
 *	ㄴ@Service
 *	ㄴ@Repository 
 * */
//컴포넌트의 자식객체가 들어있으면 사실 Controller가 아니어도 만들어 질 수 있다.
public class ServletContext implements WebMvcConfigurer {

	@Override
	public void addResourceHandlers(ResourceHandlerRegistry registry) {
		registry.addResourceHandler("/resources/**").addResourceLocations("/resources/");
	}

	
//	  @Bean 
//	  public InternalResourceViewResolver resolver() {
//	  InternalResourceViewResolver resolver = new InternalResourceViewResolver();
//	  resolver.setViewClass(JstlView.class); resolver.setPrefix("/WEB-INF/views/");
//	  resolver.setSuffix(".jsp"); return resolver; }

}

```
## views 폴더에 isnert_form.jsp 만들고 실행해보기
- Context파일들과 ServletContext 객체가 만들어지면서 오류가 없다면 jsp가 잘 실행이 될 것이다.
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	안녕하세요
</body>
</html>
```

![image](https://github.com/to7485/Web1500/assets/54658614/c7e2c3ee-4662-4e79-be59-b5f4e6966323)

## insert_form.jsp에 사진을 업로드하기 위한 코드 작성하기
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script>
	function send(f){
		f.action = "upload.do";
		f.submit();
	}
</script>
</head>
<body>
	<form method="post" enctype="multipart/form-data">
		제목 : <input name="title"><br>
		사진 : <input type="file" name="photo"><br>
		<input type="button" value="전송" onclick="send(this.form)">
	</form>
</body>
</html>
```

## vo 패키지에 Photo클래스 생성하기
- jsp를 할 때는 파일의 이름만 받았었지만, 파일을 직접 받아주기 위한 속성이 하나 더 있어야 한다.
```
package vo;

import org.springframework.web.multipart.MultipartFile;

import lombok.Getter;
import lombok.Setter;

@Setter
@Getter
public class PhotoVO {
	//<input name="title"><br>
	//<input type="file" name="photo"><br>

	private String title; //우리가 설정한 제목
	private String filename; //실제 파일 제목
	private MultipartFile photo; //사진이든 동영상이든 일단 정보를 담아주자.(용량, 이름과 같은 정보를 알 수 없다)
}

```

## FileUploadController에서 매핑받아주기
```
@RequestMapping("upload.do")
public String upload(PhotoVO vo) {
	//우선 우리가 지정한 경로에 파일이 잘 업로드가 되는지 확인해보자.
	String savePath = "c:/myupload";

	//업로드된 파일의 정보
	//MultipartRequest 클래스가 없어서 MultipartFile가 받는다.
	MultipartFile photo = vo.getPhoto();
	String filename = "no_file";

	//!photo.isEmpty() 내용이 뭐라도 들어있다.
	if(!photo.isEmpty()) {
		//photo.getOriginalFilename() : 업로드된 실제 파일명
		filename=photo.getOriginalFilename();

		//파일을 저장할 경로 지정
		File saveFile = new File(savePath, filename);

		if(!saveFile.exists()) {//경로가 없다면...
			//폴더를 만들어라
			saveFile.mkdirs();
		}	
	}

	vo.setFilename(filename); //vo.getPhoto();로 얻어온 파일정보에서 파일이름을 뽑아서 넣어주자
	return "";
}
```

## 실행하고 결과 확인하기
- 파일이 폴더형식으로 만들어져 있는걸 확인할 수 있다.

![image](https://github.com/to7485/Web1500/assets/54658614/403e48c6-206d-4700-8309-5f3f631ec3d7)

똑같은 파일을 한번 더 등록해보자. 이름이 똑같기 때문에 2개가 안만들어지는걸 볼 수 있다.<br>
왜 폴더 형태로 나오는가? savepath는 c드라이브까지의 경로가 저장되어 있다.<br>
filename은c:/myupload/abc.jpg 이런식으로 경로를 찾고 있는건데 if문에서 존재하냐고 물어보면 폴더를 만든다.<br>
그런데 File 클래스는 폴더밖에 못만든다. 이미지가 되야하는것도 이미지라고 생각을 하지 못하고 폴더로 만들어 버린다.<br>
폴더로 생성한 후 파일로 바꿔주자!<br>

## FileUploadController 코드 추가하기
- 폴더형태가 아니라 실제 이미지 모양으로 확인을 해보자
```
if(!saveFile.exists()) {
    saveFile.mkdirs();
    } else {
	//동일한 이름의 파일일 경우 폴더형태로 변환이 불가하므로
	//업로드 시간을 붙여서 이름이 중복되는 것을 방지
	//currentTimeMillis 메서드는 자바가 만들어진 1970년부터 2022년 현재까지의 시간을 100분의 1초로 저장하고 있다.

	long time = System.currentTimeMillis();
	filename = String.format("%d_%s",time,filename);
	saveFile = new File(savePath, filename);
    }
    
    //물리적으로 파일을 업로드 하는 코드
	try {
	photo.transferTo(saveFile);
	} catch (IllegalStateException e) {
	// TODO Auto-generated catch block
	e.printStackTrace();
	} catch (IOException e) {
	// TODO Auto-generated catch block
	e.printStackTrace();
	}


```
폴더가 이미지 파일로 바뀌었는지 확인해보고<br>

이름이 중복된 파일을 올리면 앞에 시간이 붙는지도 확인해보자<br>

# 이미지 출력하기
- 이미지 파일을 절대경로에 저장하고 jsp에 출력해주는 작업을 해보자

절대경로를 어떻게 가져왔었는지 생각해보자.<br>
String webPath = "/resources/upload/"; 우리가 실제로 접근해야 하는 폴더 (이게 절대 경로가 될것이다)<br>
String savePath = "";<br>
없으면 mkdir로 새로 만들것이기 때문에 따로 resources에다가 직접 만들지는 않을 것이다.<br>
```
ServletContext application = request.getServletContext();
application.getRealPath(savePath)
```
절대경로를 가져오려면 application 객체를 만들어놓고 application.getRealPath(webPath)이런식으로 가져와야 된다.<br>
그래서 메서드마다 request를 등록하기 보다 컨트롤러에 @Autowired를 추가해보자.<br>

## FileUploadController상단에 코드 추가하기
```
@Autowired //자동 주입 어노테이션
//servlet-context, session, request, 같은 jsp,servlet에서 제공을 해주는 객체이기 때문에스프링에서도 지원을 한다.
//jsp에서 servlet에서 지원해주는 객체를 따로 생성하는 과정없이 자동으로  만들어둘수 있게 해주는 어노테이션.
//무조건은 아니지만 코드를 손보면 원하는 객체를 다 만들 수 있다.

HttpServletRequest request;

```
## ServletContext에서 컨트롤러 객체 수동으로 만들어보기
```
@Bean
public FileUploadController controller() {
	return new FileUploadController();
}
```

## 절대경로 잡아주기
```
@RequestMapping("upload.do")
public String upload(PhotoVO vo) {

	String webPath = "/resources/upload/";
	String savePath= request.getServletContext().getRealPath(webPath);
	System.out.println(savePath);

	//콘솔에 절대경로가 잘 출력되는지 보고 절대경로가서 이미지파일이 있는지 확인해보자

	//업로드된 파일의 정보
	//MultipartRequest 클래스가 없어서 MultipartFile가 받는다.
	MultipartFile photo = vo.getPhoto();
	String filename = "no_file";

	//!photo.isEmpty() 내용이 뭐라도 들어있다.
	if(!photo.isEmpty()) {
		//photo.getOriginalFilename() : 업로드된 실제 파일명
		filename=photo.getOriginalFilename();

		//파일을 저장할 경로 지정
		File saveFile = new File(savePath, filename);

		if(!saveFile.exists()) {//경로가 없다면...
			//폴더를 만들어라
			saveFile.mkdirs();
		} else {
			//동일한 이름의 파일일 경우 폴더형태로 변환이 불가하므로
			//업로드 시간을 붙여서 이름이 중복되는 것을 방지
			//currentTimeMillis 메서드는 자바가 만들어진 1970년부터 2022년 현재까지의 시간을 100분의 1초로 저장하고 있다.

			long time = System.currentTimeMillis();
			filename = String.format("%d_%s",time,filename);
			saveFile = new File(savePath, filename);
		}

		//물리적으로 파일을 업로드 하는 코드
		try {
			photo.transferTo(saveFile);
		} catch (IllegalStateException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}

	vo.setFilename(filename);
	
	모델로 바인딩 하는게 좋지만 오토와이어드도 잘 되었는지 확인하기 위해서 한번 사용해보자
	request.setAttribute("vo", vo);
	
	return VIEW_PATH+"input_result.jsp";	
}
```

## views에 input_result.jsp 만들어서 출력하기
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	제목 : ${vo.title }<br> <!-- webapp까지의 경로 -->
	<img src="${pageContext.request.contextPath}/resources/upload/${vo.filename}"
		width="200"> 
</body>
</html>
```

![image](https://github.com/to7485/Web1500/assets/54658614/128258f1-cd74-44a0-acd4-3bbc7a57daff)


# 방명록에 파일업로드 기능 추가하기
- 방명록 프로젝트의 pom.xml에 파일업로드를 위한 라이브러리를 복사하기
```
<!-- /commons-fileupload -->
<dependency>
	<groupId>commons-fileupload</groupId>
	<artifactId>commons-fileupload</artifactId>
	<version>1.3.1</version>
</dependency>

<!--commons-io -->
<dependency>
	<groupId>commons-io</groupId>
	<artifactId>commons-io</artifactId>
	<version>2.4</version>
</dependency>
```
- context_4_fileupload.java를 복사해서 프로젝트에 붙혀넣기
- WebInitializer에 context_4_fileupload.class 추가하기
- visit.xml에 새 글 추가할 때 파일이름도 같이 보내주자.
```
<!-- 새글 추가하기 -->
<insert id="visit_insert" parameterType="visit">
	insert into visit values(
			seq_visit_idx.nextVal,
			#{name},
			#{content},
			#{pwd},
			#{ip},
			sysdate,
			#{filename}
	)
</insert>
```
- 방명록 테이블에 파일이름 저장할 컬럼을 추가하자
```
--파일이름을 저장할 컬럼 추가
alter table visit add filename varchar2(100);
```
- vo에도 filename 변수 추가하기
```
private String name,content,pwd,ip,regdate, filename;
	
private MultipartFile photo; //파일의 정보를 알아올 수 있는 클래스
```
- 글쓰기를 했을 때 파일 첨부가 한줄이 더 있어야 한다.(insert_form.jsp)

```
<form method="post" enctype="multipart/form-data">
		<table border="1" align="center">
			<caption>::새글 작성하기::</caption>
			
			<tr>
				<th>작성자</th>
				<td><input name="name" style="width:250px;"></td>
			</tr>
			<tr>
				<th>내용</th>
				<td>
					<textarea row="5" cols="50" name="content" style="resize:none;"wrap="on"></textarea>
				</td>
			</tr>
			
			<tr>
				<th>비밀번호</th>
				<td><input name="pwd" type="password"></td>
			</tr>
			<tr>
				<th>이미지 첨부</th>
				<td><input type="file" name="photo"></td>
			</tr>
			<tr>
				<td colspan="2" align="center">
				<input type="button" value="등록하기" onclick="send(this.form);">
				<input type="button" value="목록으로" 	onclick="location.href='visit_list.do'">
			</td>
			</tr>
		
		</table>
	
	</form>
```

- Controller에서 절대경로를 잡아주자
```
//새 글 작성
	@RequestMapping("/insert.do")
	public String insert(VisitVO vo) {
		//insert.do?name=일길동&content=내용&pwd=111
		String ip = request.getRemoteAddr();
		vo.setIp(ip);
		
		String webPath="/resources/upload/";
		String savePath = request.getServletContext().getRealPath(webPath);
		System.out.println(savePath);
		
		//업로드된 파일의 정보
		MultipartFile photo = vo.getPhoto();
		
		String filename="no_file";
		
		//업로드된 파일이 존재한다면
		if(!photo.isEmpty()) {
			//업로드될 실제 파일명
			filename=photo.getOriginalFilename();
			
			File saveFile = new File(savePath, filename);
			if(!saveFile.exists()) {
				saveFile.mkdirs();
			} else {
				//동일파일명 방지
				long time = System.currentTimeMillis();
				filename = String.format("%d_%s", time,filename);
				saveFile = new File(savePath, filename);
			}
			
			try {
				//업로드를 위한 실제 파일을 물리적으로 기록
				photo.transferTo(saveFile);
			} catch (Exception e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
		}//if
			
		vo.setFilename(filename); 
		
		int res = visit_dao.insert(vo);
		
		//sendRedirect("list.do");
		
		return "redirect:visit_list.do"; 
	}
```
- visit_list.jsp에서 filename이 no_file이 아닐때만 이미지를 보여주도록 작성해보자
```
<div class="type_content"><pre>${vo.content}</pre><br>
    <c:if test="${vo.filename ne 'no_file' }">
	<img src="${pageContext.request.contextPath}/resources/upload/${vo.filename}" width="200">
    </c:if>
</div>
```

![image](https://github.com/to7485/Web1500/assets/54658614/c59f2bda-8b2a-42cd-ad9e-d419c1df0f5f)


