## 의존성 주입

## Ex_날짜_SpringMVC
- com.korea.test_di

## pom.xml 복사해오기, config패키지 복사해오기
- root-context.xml,servlet-context.xml,web.xml 삭제하기

## dao 패키지에 BoardDAO 인터페이스 만들기

![image](https://github.com/to7485/Web1500/assets/54658614/467e3eac-259c-4d53-8549-56c25f04a188)

```
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
```
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
```
package service;

import java.util.List;

public interface BoardService {

	List<Object> selectList();
	
}
```

## 인터페이스를 구현할 BoardServiceImpl 클래스 만들기
```
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

![image](https://github.com/to7485/Web1500/assets/54658614/eddf47a4-8c04-4287-a336-5f1d91034078)

## bean 객체를 생성해보자
- config 패키지에 RootContext.java에 객체 생성하기
```
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
		BoardServiceImpl service = new BoardServiceImpl(new BoardDAOImpl());
		return service;
	}
}

```

## BoardController 클래스 생성하기
```
//↓↓↓ 스프링으로 하여금 해당클래스가 Controller라는 것을 인식시키기 위해
//꼬리표(어노테이션 :프로그램 주석)이 "반드시" 필요하다.

@Controller    
public class BoardController {
	//root-context.xml은 Controller를 제외한 모든 객체를 만든다.
	//유일하게 컨트롤러만이 servlet-context.xml에서 만들어진다.
	BoardServiceImpl service;
	
	public BoardController(BoardServiceImpl service;) {
		System.out.println("--BoardController()의 생성자--");
		this.service = service;
	}

	public void setService(BoardServiceImpl service) {
		//셋터인젝션을 사용하기 위한 셋터메서드 생성
		this.service = service;
	}
	
	생성자 주입 or setter 주입 중에 하나 사용하기	
	
}
```


## config 패키지에 ServletContext.java에 객체 만들기
```
package config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import com.korea.test.BoardController;

import dao.BoardDAOImpl;
import service.BoardServiceImpl;

@Configuration
@ComponentScan("com.korea.test_di")
public class ServletContext {
	
	@Bean
	public BoardController helloController() {
		return new BoardController(new BoardServiceImpl(new BoardDAOImpl()));
	}
}
```

## BoardController에서 바인딩 및 포워딩해주기
```
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
```
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
