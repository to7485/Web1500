# 게시판 만들기

## Ex_날짜_SpringBoard (SpringLegacy Project) 생성하기
- com.korea.bbs
- pom.xml, resources 패키지들 가져오기
- jsp에서 썼던 board.xml, util패키지 붙혀넣기

```java
package util;

public class Common {
	
	//일반게시판용
	public static class Board{
  
		////// 한줄 추가하기
		public final static String VIEW_PATH = "/WEB-INF/views/board/";
		/////
		//한페이지에 보여줄 게시물 개수
		public final static int BLOCKLIST = 10;
	
		//페이지 메뉴 수
		// <- 1 2 3 ->
		public final static int BLOCKPAGE = 3;
	}
	
	//공지사항 게시판용
	public static class Notice{
		//한 페이지에서 보여줄 게시물 수
		public final static int BLOCKLIST = 20;
		public final static int BLOCKPAGE = 5;
	}
}

```

## vo패키지에 BoardVO클래스 만들기

```java
package vo;

import lombok.Getter;
import lombok.Setter;

@Getter
@Setter
public class BoardVO {
	
	private int idx; //글 번호
	private int readhit; //조회수
	private int ref; //참조글 번호
	private int step; //순서
	private int depth; //댓글 깊이
	private int del_info; //삭제여부
	
	private String name; //작성자
	private String subject; //제목
	private String content; //내용
	private String pwd; //비밀번호
	private String  ip; //ip
	private String regdate; //작성날짜

}
```

## dao 패키지에 BoardDAO클래스 생성하기

```java
package dao;

import java.util.HashMap;
import java.util.List;

import org.apache.ibatis.session.SqlSession;

import vo.BoardVO;

public class BoardDAO {

	SqlSession sqlSession;
	
	public BoardDAO(SqlSession sqlSession) {
		this.sqlSession = sqlSession;
	}
	
	//게시글 추가
	public int insert(BoardVO vo) {
		int res = sqlSession.insert("b.board_insert", vo);
		return res;
	}
	
	//idx에 해당하는 상세 내용 한건 조회
	public BoardVO selectOne(int idx) {
		BoardVO vo = sqlSession.selectOne("b.board_one",idx);
		return vo;
	}
	
	//조회수 증가
	public int update_readhit(int idx) {
		int res = sqlSession.update("b.update_readhit",idx);
		return res;
	}
	
	//댓글추가를 위한 step+1
	public int update_step(BoardVO vo) {
		int res = sqlSession.update("b.board_update_step", vo);
		return res;
	}
	
	//댓글추가
	public int reply(BoardVO vo) {
		int res = sqlSession.insert("b.board_reply",vo);
		return res;
	}
	
	//게시글삭제(된것처럼 업데이트)
	public int del_update(BoardVO vo) {
		int res = sqlSession.update("b.del_update",vo);
		return res;
	}
	
	//전체 게시물 수 조회
	public int getRowTotal() {
		int count = sqlSession.selectOne("b.board_count");
		return count;
	}
}

```

## Context_3_dao클래스에 dao 객체 생성하기

```java
package context;

import org.apache.ibatis.session.SqlSession;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import dao.BoardDAO;

//bean객체 만들어줌
@Configuration
public class Context_3_dao {
	
	@Bean
	public BoardDAO boardDAO(SqlSession sqlSession) {
		return new BoardDAO(sqlSession);
	}

}
```
## com.korea.bbs 패키지에 BoardController 생성하기
```java
package com.korea.bbs;

import org.springframework.stereotype.Controller;

import dao.BoardDAO;

@Controller
public class BoardController {

	BoardDAO board_dao;
	
	public BoardController(BoardDAO board_dao) {
		this.board_dao = board_dao;
	}
	
}

```

## Servlet_Context에서 객체 생성하기

```java
package mvc;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.EnableWebMvc;
import org.springframework.web.servlet.config.annotation.ResourceHandlerRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

import com.korea.bbs.BoardController;

import dao.BoardDAO;


@Configuration
@EnableWebMvc
//@ComponentScan("com.korea.json")
public class Servlet_Context implements WebMvcConfigurer {

	@Override
	public void addResourceHandlers(ResourceHandlerRegistry registry) {
		registry.addResourceHandler("/resources/**").addResourceLocations("/resources/");
	}

	/*
	 * @Bean public InternalResourceViewResolver resolver() {
	 * InternalResourceViewResolver resolver = new InternalResourceViewResolver();
	 * resolver.setViewClass(JstlView.class); resolver.setPrefix("/WEB-INF/views/");
	 * resolver.setSuffix(".jsp"); return resolver; }
	 */
	
	@Bean
	public BoardController boardController(BoardDAO board_dao) {
		return new BoardController(board_dao); 
	}
	 
}

```

# 조회하기
- 페이지별 게시물 조회해보기

## BoardDAO에서 매퍼 호출하기
```java
//페이지별로 게시글 조회
public List<BoardVO> selectList(HashMap<String, Integer> map){
	List<BoardVO> list = sqlSession.selectList("b.board_list", map);
	return list;
}
```

## BoardController에서 조회하기 위한 매핑 만들기

```java
package com.korea.bbs;

import java.util.HashMap;
import java.util.List;

import javax.servlet.http.HttpServletRequest;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

import dao.BoardDAO;
import util.Common;
import util.Paging;
import vo.BoardVO;

@Controller
public class BoardController {
	
	@Autowired
	HttpServletRequest request;

	BoardDAO board_dao;
	
	public BoardController(BoardDAO board_dao) {
		this.board_dao = board_dao;
	}
	
	@RequestMapping(value= {"/","list.do"})
	public String list(Model model, String page) {
		
		int nowPage = 1;
		
		//board_list.do <-- null
		//board_list.do?page= <--empty
		
		if(page != null && !page.isEmpty()) {
			nowPage = Integer.parseInt(page);
		}
		
		//한 페이지에 표시될 게시물의 시작과 끝번호 계산
		//page가 1이면 1~10까지 계산되야함
		//page가 2이면 11~20까지 계산되야함
		int start = (nowPage-1) * Common.Board.BLOCKLIST + 1;
		int end = start + Common.Board.BLOCKLIST - 1;
		
		HashMap<String, Integer> map = new HashMap<String, Integer>();
		map.put("start",start);
		map.put("end",end);
		
		//페이지 번호에 따른 전체 게시글 조회
		List<BoardVO> list = board_dao.selectList(map);
		
		//전체 게시물 수 조회
		int rowTotal = board_dao.getRowTotal();
		
		//페이지 메뉴 생성하기
		String pageMenu = Paging.getPaging(
				"board_list.do",
				nowPage, //현재 페이지 번호
				rowTotal, //전체 게시물 수
				Common.Board.BLOCKLIST, //한 페이지에 표기할 게시물 수
				Common.Board.BLOCKPAGE); //페이지 메뉴 수
		
		request.getSession().removeAttribute("show");
		
		model.addAttribute("list",list);
		model.addAttribute("pageMenu",pageMenu);
		
		return Common.Board.VIEW_PATH+"board_list.jsp";
	}
	
}

```
## views 안에 board폴더 만들고 board_list.jsp 만들기
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib prefix="fn" uri="http://java.sun.com/jsp/jstl/functions" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<style>
	a{text-decoration: none;}
	table{border-collapse:collapse;}
</style>
</head>
<body>
	<table border="1" width="700">
		<tr>
			<td colspan="5"><img src="img/title_04.gif"></td>
		</tr>
		<tr>
			<th>번호</th>
			<th width="350">제목</th>
			<th width="120">작성자</th>
			<th width="100">작성일</th>
			<th width="50">조회수</th>
			<!-- 게시물들을 보여줄 for each -->
			<c:forEach var="vo" items="${list}">
			<tr>
				<td align="center">${vo.idx }</td>
				
				<!-- 댓글일 경우에는 들여쓰기 -->
				<td>
					<c:forEach begin="1" end="${vo.depth}">&nbsp;</c:forEach>
					<!-- 댓글기호 -->
					<c:if test="${vo.depth ne 0 }">ㄴ</c:if>
					
					<!-- 삭제되지 않은 글이라면 클릭 가능 -->
					<c:if test="${vo.del_info ne -1 }">
						<a href="view.do?idx=${vo.idx}&page=${param.page}">
						<font color="black">${vo.subject }</font>
						</a>
					</c:if>
					
					<!-- 삭제된 게시물은 클릭할 수 없도록 처리 -->
					<c:if test="${vo.del_info eq -1 }">
						<font color="gray">${vo.subject}</font>
					</c:if>
				</td>
				
				<td>${vo.name }</td>
				
				<!-- 삭제가 되지 않은 게시물은 정상적으로 표시 -->
				<c:if test="${vo.del_info ne -1 }">
				<td>${ fn:split(vo.regdate, ' ')[0] }</td>
				</c:if>
				
				<!-- 삭제가 된 게시물은 unknown으로 표시 -->
				<c:if test="${vo.del_info eq -1 }">
				<td>unknown</td>
				</c:if>
				<td>${vo.readhit }</td>
			</tr>
			</c:forEach>
			<tr>
				<td colspan="5" align="center">
					${pageMenu}
				</td>
			</tr>
			
			<tr>
				<td colspan="5" align="right">
					<img src="img/btn_reg.gif"
					 onclick="location.href='insert_form.jsp'"
					 style="cursor:pointer;">
				</td>
			</tr>
	</table>

</body>
</html>
```




