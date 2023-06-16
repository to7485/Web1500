# 타임리프(Thymeleaf)
- 타임리프는 컨트롤러가 전달하는 데이터를 이용해 동적으로 화면을 만들어주는 역할을 하는 view 템플릿 엔진이다.
- 스프링부트에서는 공식적으로 VIEW영역에서 JSP의 사용을 권장하지 않습니다.

![image](https://github.com/to7485/Web1500/assets/54658614/d5d4ff17-44c6-4513-b2bc-4b55e7242a63)

- 가능하다면 JSP를 피하고 Thymeleaf와 같은 템플릿 엔진을 사용하라고 권장하고 있다.


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

※OGNL : 자바 언어가 지원하는 범위보다 더 단순한 식을 사용하면서 속성(자바빈즈에서 발견되는 setProperty와 getPeroperty 메소드를 통해)을 가져오고 설정하는 것을 허용하고 자바 클래스의 메소드를 실행하는 오픈 소스 표현식 언어(EL)이다. 

- 변수 식은 OGNL에 해당하는 변수를 값으로 사용한다. 타임리프에는 템플릿을 변환할 때 필요한 데이터를 가진 컨텍스트가 존재하는데 변수명을 사용해서 이 컨텍스트에 보관된 객체에 접근한다. 스프링 MVC에 연동할 경우 <b>컨트롤러에서 생성한 모델 속성 이름이 변수명이 된다.</b>

```
<p>아이디: <span th:text="${member.id}">id</span></p>
```

- 메시지 식은 외부 메시지 자원에서 코드에 해당하는 문자열을 읽어와 출력한다. 지정한 경로에 위치한 프로퍼티 파일을 메시지 자원으로 사용한다. 스프링 MVC 연동을 하면 \<spring:message\>와 동일하게 스프링이 제공하는 MessageSource로 부터 코드에 해당하는 메시지를 읽어온다. 

```
<title th:text="#{message.register}">title</title>
```

- 링크 식은 링크 문자열을 생성한다. 링크 식이 절대 경로면 JSTL의 \<c:url\>태그와 동일하게 웹 어플리케이션 컨텍스트 경로를 기준으로 링크를 생성한다.

```
<a href="#" th:href="@{/members}">목록</a>
```

- 링크의 일부를 식으로 변경하고 싶다면 경로에 {변수}를 사용할 수 있다. 

```
<a href="#" th:href="@{/members/{memId}(memId=${mem.id})}">상세</a>
```

- 위 코드에서 링크 식의 {memId}는 경로 변수이다. 경로 변수 memId에 넣을 값을 뒤에 붙인 괄호 안에 지정한다. 위 코드에서 뒤에 붙인 (memId\=${memId})는 경로 변수 memId의 값으로 ${mem.id}를 사용한다는 것을 뜻한다.

- 선택 변수식 th:object 속성과 관련되어 있다. th:object 속성은 특정 객체를 선택하는데 선택 변수식은 th:object로 선택한 객체를 기준으로 나머지 경로를 값으로 사용한다. 
- 예를 들어 아래 코드에서 \*{name}은 \<div\> 태그의 th:object에서 선택한 member 객체를 기준으로 name경로를 선택한다. 따라서 \*{name}은 ${member.name}과 같은 경로가 된다.

```
<div th:object="${member}">
	<span th:text="*{name}">name</span>
</div>
```

### 타임리프 식 객체
- 타임리프는 식에서 사용할 수 있는 객체를 제공한다. 이 식 객체를 이용하면 문자열 처리나 날짜 형식 변환 등의 작업을 할 수 있다. "#객체명"을 사용해서 식 객체를 사용한다. 
- 다음은 dates, 식 객체를 이용해서 Date 타입 변수 값을 형식에 맞게 출력하는 예이다.

```
<span th:text="${#dates.format(date, 'yyyy-MM-dd')}">date</span>
```

- 각 식 객체는 기능이나 속성을 제공한다. dates 식 객체의 경우 format을 비롯해 날짜 형식 포맷팅을 위한 다양한 기능을 제공한다. 

#### 타임리프가 제공하는 주요 식 객체

- #strings : 문자열 비교, 문자열 추출 등 String 타입을 위한 기능 제공
- #numbers : 포맷팅 등 숫자 타입을 위한 기능 제공
- #dates, #calendars, #temporals : Date타입과 Calendar 타입, LocalDateTIme 타입을 위한 기능 제공
- #lists, #sets, #maps : List, Set, Map을 위한 기능 제공



### th:text

- 뷰 영역에서 사용할 MemberDto(커맨드 객체, 데이터 전달용 객체 Data Transfer Object)를 생성해서 사용합니다.

- th:text : 식의 값을 태그 몸페로 출력한다. '\<'나 '&'와 같은 HTML 특수 문자를 '&lt;'과 '&amp;'와 같은 엔티티 형식으로 변환한다.
- th:utext : 식의 값을 태그 몸체로 출력한다. '\<'나 '&'와 같은 HTML 특수 문자를 그대로 출력한다.



