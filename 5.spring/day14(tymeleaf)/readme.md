# 타임리프(Thymeleaf)
- 타임리프는 컨트롤러가 전달하는 데이터를 이용해 동적으로 화면을 만들어주는 역할을 하는 view 템플릿 엔진이다.

## Thymeleaf가 제공해주는 템플릿
- Thymeleaf는 2개의 Markup Template Mode(HTML,XML)가 있고 3개의 Textual Template Mode(TEXT, Javascript, CSS)가 있고 1개의 no-op Template Mode(Raw)가 있다.
  - HTML
  - XML
  - TEXT
  - JavaScript
  - CSS
  - Raw

## 타임리프의 특징
- 서버상에서 동작하지 않아도 HTML파일의 내용을 바로 확인 가능하다.
  - JSP와 같은 경우 서버를 구동하지 않고 해당 파일을 열게되면 JSP코드와 HTML이 섞여있어서 정상적인 확인이 불가능했다.
  - 반면 타임리프는 화면 구성을 서버 가동없이 쉽게 파악할 수 있어 개발에 수정할때마다 서버 재가동이 필요없어지기 때문에 개발에 용이하다.
- 순수 HTML구조를 유지한다.

## Thymeleaf사용을 위한 라이브러리 추가하기
- 스프링 MVC에서 타임리프는 뷰 영역에 해당한다. 스프링 MVC의 구성 요소에 대해 설명할 때 뷰 영역과 관련된 구성 요소는 ViewResolver와 View였다. 타임리프는 thymeleaf-spring5 모듈을 제공하는데 이 모듈에 타임리프 연동을 위한 ViewResolver와 View 구현 클래스가 존재한다. 스프링 MVC에서 타임리프가 제공하는 ViewResolver를 사용하도록 설정하면 결과를 타임리프 템플릿을 이용해서 생성할 수 있다.

- 스프링 MVC와 타임리프를 연동하려면 먼저 타임리프의 스프링 연동 모듈을 의존에 추가해야 한다. 다음의 세 가지(thymeleaf-spring5, thymeleaf-extras-java8time, thymeleaf-layout-dialect) 의존 설정을 추가한다.
```
<dependency>
	<groupId>org.thymeleaf</groupId>
	<artifactId>thymeleaf-spring5</artifactId>
	<version>3.0.15.RELEASE</version>
</dependency>
<dependency>
	<groupId>org.thymeleaf.extras</groupId>
	<artifactId>thymeleaf-extras-java8time</artifactId>
	<version>3.0.4.RELEASE</version>
</dependency>
<dependency>
	<groupId>nz.net.ultraq.thymeleaf</groupId>
	<artifactId>thymeleaf-layout-dialect</artifactId>
	<version>3.1.0</version>
</dependency>
```
- thymeleaf-spring5 모듈은 스프링 MVC에서 타임리프를 뷰로 사용하기 위한 기능을 제공한다. 
- thymeleaf-extras-javaStime은 LocalDateTime과 같은 자바 8의 시간 타입을 위한 추가 기능을 제공한다.
- thymeleaf-layout-dialect은 페이지 레이아웃 기능을 사용하기 위한 추가 기능을 제공합니다.
- 의존을 추가했다면 스프링 MVC가 타임리프를 사용하도록 ViewResolver를 설정한다. 이를 위한 설정 코드는 다음과 같다.

## Servlet_Context에 코드 수정하기
```java
package mvc;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.DefaultServletHandlerConfigurer;
import org.springframework.web.servlet.config.annotation.EnableWebMvc;
import org.springframework.web.servlet.config.annotation.ResourceHandlerRegistry;
import org.springframework.web.servlet.config.annotation.ViewResolverRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;
import org.thymeleaf.extras.java8time.dialect.Java8TimeDialect;
import org.thymeleaf.spring5.SpringTemplateEngine;
import org.thymeleaf.spring5.templateresolver.SpringResourceTemplateResolver;
import org.thymeleaf.spring5.view.ThymeleafViewResolver;

import com.korea.test3.HomeController;

import nz.net.ultraq.thymeleaf.layoutdialect.LayoutDialect;


@Configuration
@EnableWebMvc
//@ComponentScan("com.korea.test3")
//어노테이션에도 상속관계가 있다
/*
 *@Component
 *	ㄴ@Controller
 *	ㄴ@Service
 *	ㄴ@Repository 
 * */
//컴포넌트의 자식객체가 들어있으면 사실 Controller가 아니어도 만들어 질 수 있다.
public class ServletContext1 implements WebMvcConfigurer {
	
	@Autowired
	ApplicationContext applicationContext;
	
	@Override
	public void configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer) {
		configurer.enable();
	}

	@Override
	public void addResourceHandlers(ResourceHandlerRegistry registry) {
		registry.addResourceHandler("/resources/**").addResourceLocations("/resources/");
	}
	
	@Bean
	public SpringResourceTemplateResolver templateResolver() {
		SpringResourceTemplateResolver templateResolver = new SpringResourceTemplateResolver();
		templateResolver.setApplicationContext(applicationContext);
		//templateResolver.setPrefix("/WEB-INF/views/");
		//templateResolver.setSuffix(".html");
		templateResolver.setCacheable(false);
		return templateResolver;
	}
	
	//타임리프의 템플릿 엔진을 선정한다. 템플릿 파일을 읽어올 때 선언한 TemplateResolver를 사용한다.
	@Bean
	public SpringTemplateEngine templateEngine() {
		SpringTemplateEngine templateEngine = new SpringTemplateEngine();
		templateEngine.setTemplateResolver(templateResolver());
		templateEngine.setEnableSpringELCompiler(true);
		templateEngine.addDialect(new Java8TimeDialect()); //자바8의 시간타입을 지원하기 위한 Dialect 추가
		templateEngine.addDialect(new LayoutDialect()); //개선된 레이아웃 추가 기능을 위해 LayoutDialect 역시 추가
		return templateEngine;
	}

	@Bean
	public ThymeleafViewResolver thymeleafViewResolver() {
		ThymeleafViewResolver resolver = new ThymeleafViewResolver();
		resolver.setContentType("text/html");
		resolver.setCharacterEncoding("utf-8");
		resolver.setTemplateEngine(templateEngine());
		return resolver;
	}
		
	@Override
	public void configureViewResolvers(ViewResolverRegistry registry) {
		registry.viewResolver(thymeleafViewResolver());
	}

//	  @Bean 
//	  public InternalResourceViewResolver resolver() {
//	  InternalResourceViewResolver resolver = new InternalResourceViewResolver();
//	  resolver.setViewClass(JstlView.class); resolver.setPrefix("/WEB-INF/views/");
//	  resolver.setSuffix(".jsp"); return resolver; }
	
	@Bean
	public HomeController homeController() {
		return new HomeController();
	}	 
}

```

## src/main/java/controller/BasicController.java

```java
package controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
@RequestMapping("/tpl")
public class BasicController {
	
	@GetMapping("/ex01")
	public String ex01(Model model) {
		model.addAttribute("data", "타임리프 예제입니다.");
		return "ex01";
	}
}
```

## src/webapps/WEB-INF/view/ex01.html

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="utf-8">
    <title>Title</title>
</head>
<body>
    <p th:text="${data}">Hello Thymeleaf!!</p>
</body>
</html>
```

## Thymeleaf 기본문법
- 타임리프는 크게 변수식, 페이지식, 링크 식의 세 가지 식과 선택 변수 식을 제공한다.

- <b>변수 식: </b> ${OGNL}
- <b>메시지 식:</b> #{코드}
- <b>링크 식 :</b> @{링크}
- <b>선택 변수 식 :</b> \*{OGNL}

- 변수 식은 OGNL에 해당하는 변수를 값으로 사용한다. 타임리프에는 템플릿을 변환할 때 필요한 데이터를 가진 컨텍스트가 존재하는데 변수명을 사용해서 이 컨텍스트에 보관된 객체에 접근한다. 스프링 MVC에 연동할 경우 <b>컨트롤러에서 생성한 모델 속성 이름이 변수명이 된다.</b>











