# 방명록 만들기

## Ex_날짜_visit 프로젝트 만들기
- pom.xml과 resources에 있는 패키지들 가져오기
    - Artifact Id, Name 수정해주기
- web,root,servlet.xml 삭제하기
- mapper이름 visit으로 바꾸고 mybatis-config.xml에 참조하기

## 방명록 테이블 작성하기
```sql
--방명록 DB

--시퀀스
create sequence seq_visit_idx;

create table visit(
  idx NUMBER(3) PRIMARY KEY,
  name VARCHAR2(50), --작성자
  content VARCHAR2(1000), --내용
  pwd VARCHAR2(50), --비번
  ip VARCHAR2(20), --ip
  regdate DATE --작성일
);

--샘플데이터
insert into visit values(
  seq_visit_idx.nextVal,
  '일길동',
  '내가 1빠',
  '1111',
  '192.1.1.1',
  sysdate
);

insert into visit values(
  seq_visit_idx.nextVal,
  '이길동',
  '내가 2빠',
  '1111',
  '192.1.1.1',
  sysdate
);

commit;
```

## vo 패키지에 VisitVO 만들기
```java
package vo;

import lombok.Getter;
import lombok.Setter;

@Getter
@Setter
public class VisitVO {

	private int index;
	private String name, content, pwd, ip, regdate;
}
```

## dao패키지에 VisitDAO클래스 만들기
```java
package dao;

import org.apache.ibatis.session.SqlSession;

public class VisitDAO {

	SqlSession sqlSession;
	
	public VisitDAO(SqlSession sqlSession) {
		this.sqlSession = sqlSession;
	}
    
    //방명록 전체 조회
	public List<VisitVO> selectList(){
		List<VisitVO> list = sqlSession.selectList("v.visit_list");
		return list;
	}
}

```

## Context_3_dao클래스에 dao 객체 생성하기
```java
package context;

import org.apache.ibatis.session.SqlSession;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import dao.VisitDAO;

@Configuration
public class Context_3_dao {
	

	@Bean
	public VisitDAO visit_daoBean(SqlSession sqlSession) {
		return new VisitDAO(sqlSession);
	}
}

```

## visit.xml에 쿼리문 작성하기
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="v">

	<select id="visit_list" resultType="visit">
		select * from visit order by idx DESC
	</select>
</mapper>
```

## com.korea.visit패키지에 VisitController클래스 만들기
```java
package com.korea.visit;

import org.springframework.stereotype.Controller;

import dao.VisitDAO;

@Controller
public class VisitController {

	VisitDAO visit_dao;
	
	public VisitController(VisitDAO visit_dao) {
		this.visit_dao = visit_dao;
	}
}

```
## ServletContext에 Controller 객체 생성하기
```java
package mvc;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.EnableWebMvc;
import org.springframework.web.servlet.config.annotation.ResourceHandlerRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

import com.korea.visit.VisitController;

import dao.VisitDAO;

@Configuration
@EnableWebMvc
//@ComponentScan("com.korea.visit")
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
	
	@Bean
	public VisitController visitController(VisitDAO visit_dao) {
		return new VisitController(visit_dao);
	}
	 
}

```

## util 패키지 만들어서 MyCommon클래스 만들기
- 경로를 설정해주자
```java
package util;

public class MyCommon {

	public static String VIEW_PATH = "/WEB-INF/views/visit/";
}

```
## Controller에 코드 작성하기
```java
package com.korea.visit;

import java.util.List;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

import dao.VisitDAO;
import vo.VisitVO;

@Controller
public class VisitController {

	VisitDAO visit_dao;
	
	public VisitController(VisitDAO visit_dao) {
		this.visit_dao = visit_dao;
	}
	
	@RequestMapping(value= {"/","visit_list.do"})
	public String select(Model model) {
		List<VisitVO> list = visit_dao.selectList();
		model.addAttribute("list",list);
		return MyCommon.VIEW_PATH+"visit_list.jsp";
	}
}
```

## visit 폴더 생성하고 visit_list.jsp 만들기
```jsp
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
	<div id="main_box">
		<h1>방명록 리스트</h1>	<!-- 스프링에서는 jsp에서 jsp로 이동할 수 없다. -->
		<input type="button" value="글쓰기" onclick="location.href='insert_form.do'">
	</div>
	
	<c:forEach var="vo" items="${list}">
		<div class="visit_box">
		<div class="type_content">${vo.content}</div>
		<div class="type_name">작성자 : ${vo.name}(${vo.ip})</div>
		<div class="type_regdate">작성일 ${vo.regdate }</div>
		<div>
		<form>
			<input type="hidden" name="idx" value="${vo.idx }">
			비밀번호 <input type="password" name = pwd">
			<input type="button" value="수정" onclick="modify(this.form)">
			<input type="button" value="삭제" onclick="del(this.form)">
		</form>
		</div>
		</div>

	</c:forEach>
</body>
</html>
```
### 실행하면 다음과 같은 결과를 얻을 수 있다.

![image](https://github.com/to7485/Web1500/assets/54658614/cbee2d1c-1a8c-41b8-b0ab-2158a3b1a804)

## webapp/resources/css 안에 visit.css 파일 만들기

![image](https://github.com/to7485/Web1500/assets/54658614/bb296b91-4b3d-4f90-9a0f-39da3b58c492)

## jsp에서 css파일 참조하기
```
<head>
<meta charset="UTF-8">
<title>Insert title here</title>

				<!-- webapp폴더까지의 경로 -->
<link rel="stylesheet" href="${pageContext.request.contextPath}/resources/css/visit.css">

</head>
```

## css파일에 코드 작성하기
```
@charset "UTF-8";
*{margin:0; padding:0;}

#main_box{
	width:500px;
	margin:auto;
	/* background-color: #ccc; */
}

h1{
	text-align: center;
	margin-top: 10px;
	margin-bottom: 10px;
	color: #0080ff;
	text-shadow: 2px 2px 2px black;
}

.visit_box{
	margin:auto;
	width:500px;
	margin-top: 30px;
	box-shadow: 2px 2px 2px black;
	border: 1px solid blue;
}

.type_content{
	min-height: 100px;
	height:auto;
	background:#fcc;
}

.type_name{
	background: #cfc;
}

.type_regdate{
	background: #ccf;
}
```

## VisitController에 새글작성을 위한 jsp로 이동하기 위한 매핑 지정하기
```
@RequestMapping("insert_form.do")
public String insert_form() {
	return MyCommon.VIEW_PATH+"visit_insert_form.jsp";
}
```

## visit폴더에 visit_insert_form.jsp 작성하기
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
		
		//유효성체크 했다 치자
		
		f.action="insert.do"
		f.submit();
	}
</script>
</head>
<body>
	<form>
		<table border="1" align="center">
			<caption>::새글 작성하기::</caption>
			
			<tr>
				<th>작성자</th>
				<td><input name="name" style="width:250px;"></td>
			</tr>
			<tr>
				<th>내용</th>
				<td>						wrap속성 : 
					<textarea row="5" cols="50" name="content" style="resize:none;"wrap="on"></textarea>
				</td>
			</tr>
			
			<tr>
				<th>비밀번호</th>
				<td><input name="pwd" type="password"></td>
			</tr>
			<tr>
				<td colspan="2" align="center">
				<input type="button" value="등록하기" onclick="send(this.form);">
				<input type="button" value="목록으로" onclick="location.href='visit_list.do'">
			</td>
			</tr>
		
		</table>
	
	</form>
</body>
</html>
```

## VisitController 에서 insert.do 매핑으로 넘어온 파라미터 받아주기
```java
//새 글 작성
@RequestMapping("/insert.do")
public String insert(VisitVO vo, HttpServletRequest request) {
	//insert.do?name=일길동&content=내용&pwd=111
	String ip = request.getRemoteAddr();
	vo.setIp(ip);

	int res = visit_dao.insert(vo);

	//sendRedirect("list.do");
	return "redirect:visit_list.do"; 
}
```

## VisitDAO에 insert 메서드 만들기
```java
//방명록에 새글 추가하기
public int insert(VisitVO vo){
	int res = sqlSession.insert("v.visit_insert",vo);
	return res;
}
```

## visit.xml에 insert쿼리문 추가하기
```xml
<!-- 새글 추가하기 -->
<insert id="visit_insert" parameterType="visit">
	insert into visit values(
			seq_visit_idx.nextVal,
			#{name},
			#{content},
			#{pwd},
			#{ip},
			sysdate
	)
</insert>
```








