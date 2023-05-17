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





