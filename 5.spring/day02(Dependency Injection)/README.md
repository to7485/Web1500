# 의존성 주입
- Dependency Injection : 각 객체 간의 의존관계를 스프링 컨테이너가 개발자가 정의한 Bean등록 정보를 바탕으로 자동으로 주입해주는 기능

## 의존성 주입의 종류
- 필드 주입(Field Injection)
- 수정자 주입(Setter Injection)
- 생성자 주입(Constructor Injection)

### 1. 변경에 유리한 코드 작성하기
```java
class Car{};
class SportCar extends Car{};
class Truck extends Car{};
```
- 변경사항이 발생했을 때 타입과 생성자 부분을 모두 변경해줘야 한다.
```java

SportCar car = new SportCar();
↓↓↓↓↓			↓↓↓↓↓↓
Truct car = new Truck();

```
- 다형성을 이용하면 수정을 해야 하는곳이 적어진다.
```java
Car car = new SportCar();
				↓↓↓↓↓↓
Car car = new Truck();
```
- 별도의 메서드를 만들어서 객체를 생성하면 수정 포인트를 더 줄일 수 있다.
```java
Car car = getCar(); //-> 사용하는곳은 여러군데일 수 있다.
//고칠 필요가 없다.

static Car getCar(){
	return new SportCar(); //-> 기능 제공
	//return new Truck();
}
```


## Ex_날짜_DI
- com.korea.test_di

## pom.xml 복사해오기, config패키지 복사해오기
- pom.xml의 내용을 복사해서 붙혀넣고 groupId와 ArtifactId수정하기
- root-context.xml,servlet-context.xml,web.xml 삭제하기

## dao 패키지에 BoardDAO 인터페이스 만들기

```JAVA
package dao;

import java.util.List;

public interface BoardDAO {

	//Dao(Data Access Object)는 기본적으로 CRUD기능을 가지고 있다.
	//나중에 사용할 추상메서드를 준비해보자.
	//파라미터를 받을 수도 있긴 한데 뭘 받을지 모르기 때문에 모든걸 다 받을 수 있는 		
	//Object타입으로 만들자!

	int insert( Object  ob);
	
	List<Object> selectList();
	
	int update(Object ob);
	
	int delete(int idx);
}
인터페이스만 가지고는 아무것도 못한다.
인터페이스를 구현하는 자식클래스를 만들어주자.
```

## dao 패키지에 BoardDAOImpl 클래스 만들기
```JAVA
package dao;

import java.util.List;

public class BoardDAOImpl implements BoardDAO {

	//원래 DAO는 DB에 연결하여 List를 가지고 오는 작업을 하지만
	//이 예제에서는 그 부분은 생략했다.

	@Override
	public int insert(Object ob) {
		// TODO Auto-generated method stub
		return 0;
	}

	DB에서 값이 넘어왔다고 가정하고 리스트에 담아서 넘겨보기
	@Override
	public List<Object> selectList() {
		List<Object> list = new ArrayList<Object>();
		list.add("참외");
		list.add("사과");
		list.add("수박");
		list.add("복숭아");
		return list;
	}
	
	//얘가 dao 이니까 servlet이 받아서 바인딩하고 포워딩을 해서 jsp에 출력을 해줘야 된다.
	//그 사이에 service라고 하는애가 받아준다.

	@Override
	public int update(Object ob) {
		// TODO Auto-generated method stub
		return 0;
	}

	@Override
	public int delete(int idx) {
		// TODO Auto-generated method stub
		return 0;
	}

}

```

## service 패키지에 BoardService 인터페이스 만들기
```JAVA
package service;

import java.util.List;

public interface BoardService {

	List<Object> selectList();
	
}
```

## 인터페이스를 구현할 BoardServiceImpl 클래스 만들기
```JAVA
package service;

import java.util.List;

import dao.BoardDAOImpl;

public class BoardServiceImpl implements BoardService {

	BoardDAOImpl dao;
	
	public BoardServiceImpl(BoardDAOImpl dao) {
		this.dao = dao;
	}
	
	@Override
	public List<Object> selectList() {
		
		return dao.selectList();
	}

}

```

![image](image/service.png)

## bean 객체를 생성해보자
- config 패키지에 RootContext.java에 객체 생성하기
```JAVA
package config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import dao.BoardDAOImpl;
import service.BoardServiceImpl;

@Configuration
public class RootContext {

	@Bean
	public BoardDAOImpl dao() {
		return new BoardDAOImpl();
	}
	
	@Bean
	public BoardServiceImpl service(BoardDAOImpl dao) {
		return new BoardServiceImpl(dao);
	}
}

```

## BoardController 클래스 생성하기
```JAVA
//↓↓↓ 스프링으로 하여금 해당클래스가 Controller라는 것을 인식시키기 위해
//꼬리표(어노테이션 :프로그램 주석)이 "반드시" 필요하다.

@Controller    
public class BoardController {
	//root-context.xml은 Controller를 제외한 모든 객체를 만든다.
	//유일하게 컨트롤러만이 servlet-context.xml에서 만들어진다.
	BoardServiceImpl service;

	//생성자 인젝션을 위한 service객체를 파라미터로 받는 생성자
	public BoardController(BoardServiceImpl service;) {
		System.out.println("--BoardController()의 생성자--");
		this.service = service;
	}

	//셋터인젝션을 사용하기 위한 셋터메서드 생성
	public void setService(BoardServiceImpl service) {
		this.service = service;
	}
	
	생성자 주입 or setter 주입 중에 하나 사용하기	
	
}
```


## config 패키지에 ServletContext.java에 객체 만들기
```JAVA
package config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.EnableWebMvc;
import org.springframework.web.servlet.config.annotation.ResourceHandlerRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;
import org.springframework.web.servlet.view.InternalResourceViewResolver;
import org.springframework.web.servlet.view.JstlView;

import com.korea.test.BoardController;

import dao.BoardDAOImpl;
import service.BoardServiceImpl;

@Configuration
@EnableWebMvc
//@ComponentScan("com.korea.test") 자동탐색
public class ServletContext implements WebMvcConfigurer {

	@Override
	public void addResourceHandlers(ResourceHandlerRegistry registry) {
		registry.addResourceHandler("/resources/**").addResourceLocations("/resources/");
	}

	@Bean
	public InternalResourceViewResolver resolver() {
		InternalResourceViewResolver resolver = new InternalResourceViewResolver();
		resolver.setViewClass(JstlView.class);
		resolver.setPrefix("/WEB-INF/views/");
		resolver.setSuffix(".jsp");
		return resolver;
	}

	//ConstructorInjection
	@Bean
	public TestController testController(BoardServiceImpl service) {
		return new TestController(service);
	}

	//setterInjetcion
	@Bean
	public TestController testController(BoardServiceImpl service) {
		TestController testController = new TestController();
		testController.setService(service);
		return testController;
	}
}

```

## BoardController에서 바인딩 및 포워딩해주기
```JAVA
package com.korea.test;

import java.util.List;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

import service.BoardServiceImpl;

@Controller
public class BoardController {
	
	BoardServiceImpl service;
	
	public BoardController(BoardServiceImpl service) {
		this.service = service;
	}
	
	@RequestMapping("/board/list.do")
	public String select(Model model) {
		//서비스를 통해 dao의 selectList()메서드를 호출할 수 있다.
		List<Object> list = service.selectList();
		
		model.addAttribute("list",list);
		
		return "board_list";
	}
}

```

## board_list.jsp에서 과일목록 출력하기
```JAVA
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>과일목록</h1>
	
	<c:forEach var="vo" items="${list }">
		${ vo }<br>
	</c:forEach>
</body>
</html>
```

컨트롤러 한군데 에서 여러개의 매핑이 가능하기 때문에 파일의 개수를 많이 줄일 수 있다.
