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
- resources에 img폴더와 js폴더 복사해서 넣기

![image](https://github.com/to7485/Web1500/assets/54658614/77a64e64-c7ef-4fa4-a077-5002fc4db488)


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

//전체 게시물 수 조회
public int getRowTotal() {
	int count = sqlSession.selectOne("b.board_count");
	return count;
}
```

## board.xml에 쿼리문 작성하기
- 게시글에 순위를 매겨서 10개씩 한 페이지에 보여줬었다.

```xml
<!-- 페이지별 게시물 조회 -->
<select id="board_list" resultType="board" parameterType="java.util.HashMap">
	select * from 
		(select rank() over(order by ref desc,step) no, b.*
		 from board b)
		 where no between #{start} and #{end}
</select>

<!-- 전체 게시물 개수 조회 -->
<select id="board_count" resultType="int">
	select count(*) from board
</select>
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
	
	@RequestMapping(value= {"/","board_list.do"})
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
		
		return Common.Board.VIEW_PATH+"board_list.jsp?page="+nowPage;
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
			<td colspan="5"><img src="resources/img/title_04.gif"></td>
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
					<img src="resources/img/btn_reg.gif"
					 onclick="location.href='insert_form.do'"
					 style="cursor:pointer;">
				</td>
			</tr>
	</table>

</body>
</html>
```

# 게시글 하나 눌러서 상세보기

## BoardController에서 매핑해주기
```java
@RequestMapping("view.do")
public String view(Model model, int idx, int page) {
	//view.do?idx=5

	BoardVO vo = board_dao.selectOne(idx); 

	//조회수 증가
	HttpSession session = request.getSession();
	String show = (String)session.getAttribute("show");

	if(show == null) {
		int res = board_dao.update_readhit(idx);
		session.setAttribute("show", "0");
	}

	//상세보기 페이지로 전환하기 위해 바인딩 및 포워딩
	model.addAttribute("vo", vo);

	return Common.Board.VIEW_PATH+"board_view.jsp?page="+page;
}
```

## dao 패키지에 메서드 만들기
```java
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
```

## board.xml에 쿼리문 작성하기
```xml
<!-- idx에 해당되는 게시글 정보 조회(상세보기) -->
<select id="board_one" resultType="board" parameterType="int">
	select * from board where idx=#{idx}
</select>

<!-- 조회수 업데이트 -->
<update id="update_readhit" parameterType="int">
	update board set readhit = readhit + 1
	where idx=#{idx}
</update>	
```

## board폴더에 board_view.jsp 만들기
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script src="resources/js/httpRequest.js"></script>
<script>
	function reply(){
		location.href="reply_form.jsp?idx=${vo.idx}&page=${param.page}";
	}
	
	function del(){
		if(!confirm("삭제하시겠습니까?")){
			return;
		}
		
		var pwd = ${vo.pwd}; //원본비밀번호
		var c_pwd = document.getElementById("c_pwd").value;
		
		if(pwd != c_pwd){
			alert("비밀번호 불일치");
			return;
		}
		
		var url = "del.do";
		var param = "idx=${vo.idx}";
		
		sendRequest(url,param,delCheck,"post");
	}
	
	function delCheck(){
		if(xhr.readyState == 4 && xhr.status == 200){
			var data = xhr.responseText;
			//"[{'param':'yes'}]" 문자열 형태로 데이터를 받아옴
			
			var json = eval(data);
			
			if(json[0].param == 'yes'){
				alert("삭제 성공");
				location.href="board_list.do?page=${param.page}";
			} else {
				alert("삭제 실패");
			}
		}
	}
	
</script>
</head>
<body>
	<table border="1">
		<caption>:::게시글 상세보기:::</caption>
		<tr>
			<th>제목</th>
			<td>${vo.subject}</td>
		</tr>
		<tr>
			<th>작성자</th>
			<td>${vo.name}</td>
		</tr>
		<tr>
			<th>작성일</th>
			<td>${vo.regdate}</td>
		</tr>
		<tr>
			<th>ip</th>
			<td>${vo.ip}</td>
		</tr>
		<tr>
			<th>내용</th>
			<td width="500px" height="200px"><pre>${vo.content}</pre></td>
		</tr>
		<tr>
			<th>비밀번호</th>
			<td><input type="password" id="c_pwd"></td>
		<tr>
			<td colspan="2">
			<!-- 목록보기 -->
				<img src="resources/img/btn_list.gif" onclick="location.href='board_list.do?page=${param.page}'">
				
				<c:if test="${vo.depth lt 1 }">
			<!-- 답변  -->
				<img src="resources/img/btn_reply.gif" onclick="reply();">
				</c:if>
			<!-- 삭제 -->
				<img src="resources/img/btn_delete.gif" onclick="del();">
			</td>
		</tr>
	
	</table>
</body>
</html>

```

# 글 추가하기
- jsp에서 jsp로 이동하지 못하는점을 유의하며 작성한다.

## BoardController에 매핑해주기
```java
@RequestMapping("insert_form.do")
public String insert_form() {
	return Common.Board.VIEW_PATH + "insert_form.jsp";
}
```

## insert_from.jsp 만들기
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script>
	function send_check(){
		var f = document.f; //f라는 name을 가진 form태그를 가져옴
		
		f.submit();
	}
</script>
</head>
<body>
<form name="f" method="post" action="insert.do">
	<table border="1">
		<caption>:::새 글 쓰기:::</caption>

		<tr>
			<th>제목</th>
			<td><input name="subject" style="width:370px;"></td>
		</tr>
		<tr>
			<th>작성자</th>
			<td><input name="name" style="width:370px;"></td>
		</tr>
		<tr>
			<th>내용</th><!-- 가로로 50글자 세로로 엔터 10번정도 칠수 있는 크기 -->
			<td><textarea name="content" rows="10" cols="50" style="resize:none;"></textarea></td>
		</tr>
		<tr>
			<th>비밀번호</th>
			<td><input name="pwd" type="password"></td>
		</tr>
		<tr>
			<td colspan="2">
			<img src="resources/img/btn_reg.gif" onclick="send_check();">
			<img src="resources/img/btn_back.gif" onclick="location.href='board_list.do'">
			</td>
		</tr>
	</table>
</form>

</body>
</html>

```

## BoardController에서 insert.do 매핑 만들기

```java
@RequestMapping("insert.do")
public String insert(BoardVO vo) {
	String ip = request.getRemoteAddr();//ip

	vo.setIp(ip);

	int res = board_dao.insert(vo);

	if(res > 0) {
		//등록완료후 게시판의 첫 페이지로 복귀
		return "redirect:board_list.do";
	}

	return null;
}
```

## BoardDAO클래스에 메서드 추가하기

```java
//게시글 추가
public int insert(BoardVO vo) {
	int res = sqlSession.insert("b.board_insert", vo);
	return res;
}
```

## board.xml에 쿼리문 추가하기

```xml
<!-- 새글 추가 -->
<insert id="board_insert" parameterType="board">
	insert into board values(
		seq_board_idx.nextVal, 
		#{name}, 
		#{subject}, 
		#{content}, 
		#{pwd}, 
		#{ip}, 
		sysdate, 
		0, 
		seq_board_idx.currVal, 
		0, 
		0, 
		0
	)
</insert>
```

# 삭제하기
- 삭제 버튼을 누르면 삭제된것 처럼 보이게 만들기

## BoardController에 매핑만들기
```
@RequestMapping("del.do")
@ResponseBody
public String delete(int idx) {
	BoardVO baseVO = board_dao.selectOne(idx);

	baseVO.setSubject("이미 삭제된 글입니다.");
	baseVO.setName("unknown");

	int res = board_dao.del_update(baseVO);
	if(res == 1) {
		return "[{'result':'yes'}]";
	} else {
		return "[{'result':'no'}]";
	}
}
```
## BoardDAO에 메서드 만들기

```java
//한가지 글 조회
public BoardVO selectOne(int idx) {
	BoardVO vo = sqlSession.selectOne("b.board_one",idx);
	return vo;
}

//게시글 삭제(가 된것처럼 업데이트 하기)
public int del_update(BoardVO vo) {
	int res = sqlSession.update("b.del_update",vo);
	return res;
}
```

## board.xml에 쿼리문 추가하기

```xml
<!-- idx에 해당하는 게시글 한 건 조회 -->
<select id="board_one" resultType="board" parameterType="int">
	select * from board where idx= #{idx}
</select>

<!-- 게시글 삭제(된 것 처럼 업데이트) -->
<update id="del_update" parameterType="board">
	update board set subject = #{subject},
					 name = #{name},
					 del_info = -1
					 where idx=#{idx}
</update>

```

# 댓글달기
- 상세보기 페이지에서 답글달기를 눌러서 댓글이 달리도록 해보자.

## board_view.jsp에 reply()메서드 추가하기
```JS
function reply(){
	location.href="reply_form.do?idx=${vo.idx}&page=${param.page}";
}
```

## BoardController에서 매핑하기
- idx와 page를 가져가야한다.
```java
@RequestMapping("reply_form.do")
public String reply_form(int idx, int page) {
	return Common.VIEW_PATH + "reply_from.jsp?idx="+idx+"&page="+page;
}
```
## reply_form.jsp 생성하기
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script>
	function send_check(){
		var f = document.f;
		
		f.submit();
	}
</script>
</head>			<!-- reply.do?subject=ooo&name=홍길동&content=aaaaaaa.... -->
<body>
	<form name="f"
		  method="post"
		  action="reply.do">
	<input type="hidden" name="idx" value="${param.idx}">
	<input type="hidden" name="page" value="${param.page}">
		  
	<table border="1">
		<caption>:::댓글 쓰기:::</caption>
		<tr>
			<th>제목</th>
			<td><input name="subject" style="width:370px;"></td>
		</tr>
		<tr>
			<th>작성자</th>
			<td><input name="name" style="width:370px;"></td>
		</tr>
		<tr>
			<th>내용</th>
			<td><textarea name="content" rows="10" cols="50" style="resize:none;"></textarea></td>
		</tr>
		<tr>
			<th>비밀번호</th>
			<td><input name="pwd" type="password"></td>
		</tr>
		<tr>
			<td colspan="2">
				<img src="img/btn_reg.gif" onclick="send_check();">
				<img src="img/btn_back.gif" onclick="location.href='board_list.do'">
			</td>
		</tr>
	</table>
	</form>
</body>
</html>
```

## send_check()메서드 작성하기
```js
function send_check(){
	var f = document.f;
	f.submit();
}
```

## BoardController에 reply.do 매핑 만들어주기
```java
@RequestMapping("reply.do")
public String reply(BoardVO vo,int idx, int page) {
	String ip = request.getRemoteAddr();


	//같은 레퍼런스를 가지고 있는 데이터들 중에서 지금 내가 추가하려고 하는
	//step값 이상인 애들을 +1을 해놔야 하기 때문에 insert를 먼저하지 않는다.

	//기준글의 idx를 이용해서 댓글을 달고싶은 게시글의 정보를 가져온다.
	BoardVO base_vo = board_dao.selectOne(idx);

	//기준글에 step이상 값은 step = step + 1 처리
	int res = board_dao.update_step(base_vo); //-> dao에 만들러 가기

	vo.setIp(ip);

	//댓글이 들어갈 위치 선정
	vo.setRef(base_vo.getRef());
	vo.setStep(base_vo.getStep()+1);
	vo.setDepth(base_vo.getDepth()+1);

	res = board_dao.reply(vo);

	if(res > 0) {
		return "redirect:board_list.do?page="+page;
	}

	return null;
}
```

## BoardDAO에 메서드 만들기
```java
//댓글추가를 위한 step+1
public int update_step(BoardVO vo) {
	int res = sqlSession.update("b.board_update_step",vo);
	return res;
}

//댓글추가
public int reply(BoardVO vo) {
	int res = sqlSession.insert("b.board_reply",vo);
	return res;
}
```

## board.xml에 쿼리문 추가하기
```xml
<!-- 댓글작성을 위한 STEP 증가 -->
<update id="board_update_step" parameterType="board">
	update board set step = step + 1
	where ref = #{ref} and step > #{step}
</update>

<!-- 댓글달기 -->
<insert id="board_reply" parameterType="board">
	insert into board values(
		seq_board_idx.nextVal,
		#{name},
		#{subject},
		#{content},
		#{pwd},
		#{ip},
		sysdate,
		0,
		#{ref},
		#{step},
		#{depth},
		0
	)
</insert>
```
# 로그인 구현하기
- 로그인을 해야만 글을 쓸 수 있도록 로그인 기능을 구현해보자.

## MemberVO클래스 생성하기

```java
package vo;

@setter
@getter
public class MemberVO {

	private int idx;
	private String name;
	private String id;
	private String pwd;
	private String email;
}

```

## MemberDAO클래스 생성하기

```java
package dao;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

import org.apache.ibatis.session.SqlSession;

import vo.MemberVO;

public class MemberDAO {

	SqlSession sqlSession;
	public MemberDAO(SqlSession sqlSession) {
		this.sqlSession = sqlSession;
	}
}
```

## Context_3_dao클래스에서 객체 생성하기
```
@Bean
public MemberDAO member_dao(SqlSession sqlSession) { 
	return new MemberDAO(sqlSession);
}
```

## login_form.jsp 생성하기

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<!-- ajax사용하기 위해 등록해주기
로그인시도할 때 아이디가 없으면 아이디가 없다고 경고, 비밀번호가 틀리면 비밀번호가 틀렸다고 경고를 따로 띄워주자.
'아이디나 비밀번호가 틀렸습니다' 라고 나오면 개발자가 귀찮아서 코드를 추가하지 않은것 -->
	<script src="resources/js/httpRequest.js"></script>
	<script>
		function send(f){
			var id = f.id.value.trim();
			var pwd = f.pwd.value.trim();
			
			//유효성체크
			if(id==''){
				alert("아이디를 입력해주세요");
				return;
			}
			
			if(pwd==''){
				alert("비밀번호를 입력하세요");
				return;
			}
			
			var url = "login.do";
			var param ="id="+encodeURIComponent(id)+"&pwd="+encodeURIComponent(pwd);
			
			sendRequest(url,param,myCheck,"POST");
		}
		
		//콜백메서드
		function myCheck(){
			if(xhr.readyState == 4 && xhr.status == 200){
				
			}
		}
	</script>
</head>
<body>
	<form>
		<table border="1" align="center">
			<caption>:::로그인:::</caption>
			<tr>
				<th>아이디</th>
				<td><input name="id"></td>
			</tr>
			<tr>
				<th>비밀번호</th>
				<td><input name="pwd" type="password"></td>
			</tr>
			<tr>
			
				<td colspan="2" align="center">
					<input type="button" value="로그인" onclick="send(this.form)">
				</td>
			</tr>
		</table>
	</form>

</body>
</html>
```

## BoardController에 MemberDAO 주입하고 매핑 추가하기

```java
@RequestMapping("login.do")
@ResponseBody
public String login(String id, String pwd) {

	MemberVO vo = member_dao.logincheck(id);

	//vo가 null인경우 id자체가 DB에 존재하지 않는다는 의미
	if(vo==null) {
		return "[{'param':'no_id'}]";
	} 

	if(!vo.getPwd().equals(pwd)) {
		//비밀번호가 일치하지 않을 때
		return "[{'param':'no_pwd'}]";
	}

	//아이디와 비빌번호 체크에 문제가 없다면 세션에 바인딩 한다.
	//세션은 서버의 메모리(램)를 사용하기 때문에 세션을 많이 사용할수록 브라우저가 느려지기 때문에
	//필요한 곳에서만 세션을 쓰도록 하자

	//세션유지시간(기본값 30분)
	//session.setMaxInactiveInterval(3600); //세션이 1시간 유지 초단위로 써줘야함;;
	session.setAttribute("id", vo); //세션도 autowired로 받아주자

	//로그인 성공한 경우
	return "[{'param':'clear'}]";
}
```

## MemberDAO에서 메서드 작성하기
```java
//로그인 체크
/*
 * 실수를 많이 하는부분 id랑 pwd를 둘다 받았다고 가정해보자 쿼리문이 select * from member where id=? and
 * pwd=? 이렇게 만드는 순간 id가 없어도 null이 들어오고 비밀번호가 틀려도 null이 들어온다. 아이디나 비밀번호가 잘못되었습니다
 * 라고 나옴;;
 */

//로그인 조회
public MemberVO logincheck(String id) {
	MemberVO vo = sqlSession.selectOne("m.idCheck",id);
	return vo;
}
	
```

## member.xml 만들고 쿼리문 작성하기

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="m">

	<select id="idCheck" resultType="member" parameterType="String">
		select * from member where id=#{id}
	</select>
</mapper>
```

## mybatis-config.xml에 매퍼 등록하기

```xml
<typeAliases>
	 <typeAlias type="vo.BoardVO" alias="board"/>
	 <typeAlias type="vo.MemberVO" alias="member"/>
</typeAliases>

<mappers>
	 <mapper resource="config/mybatis/mapper/board.xml" />
	 <mapper resource="config/mybatis/mapper/member.xml" /> 
</mappers>
```

## BoardController에 코드 추가하기
- 로그인이 되어있다면 session에 뭐라도 들어있을 것이고 글쓰기 창으로 이동
- null이라면 로그인을 하도록 로그인창으로 이동하도록 만들자
```
//글쓰기 화면으로 이동
@RequestMapping("insert_form.do")
public String insert_form() {
	HttpSession session = request.getSession();

	MemberVO show = (MemberVO)session.getAttribute("id");

	if(show == null) {
		return Common.VIEW_PATH + "login_form.jsp;";
	}
	return Common.VIEW_PATH + "insert_form.jsp";
}
```

## board_list.jsp에 코드 추가하기
- 세션이 있으면 로그아웃 버튼이, 세션이 없으면 로그인,회원가입 버튼이 보이게 하자.
```
<tr>
	<td colspan="5" align="right">
	<c:choose>
		<c:when test="${empty id }">
				<input type="button" value="로그인" onclick="location.href='login_form.do'">
				<input type="button" value="회원가입">
		</c:when>
		<c:when test="${not empty id }">
				<input type="button" value="로그아웃">
		</c:when>
	</c:choose>
	</td>
</tr>
```

## BoardContrller에 login_form.do 매핑 잡아주기
- 로그인창으로 이동만 해주는 매핑 만들기

```java
@RequestMapping("login_form.do")
public String login_form() {
	return Common.VIEW_PATH + "login_form.jsp";
}
```

## login_form.jsp의 콜백메서드 채워주기

```
function myCheck(){
	if(xhr.readyState == 4 && xhr.status == 200){
		var data = xhr.responseText;
		var json = eval(data);

		if(json[0].param == 'no_id'){
			alert("아이디가 존재하지 않습니다.");
		}else if(json[0].param == 'no_pwd'){
			alert("비밀번호가 맞지 않습니다.");
		}else {
			alert("로그인 성공");
			location.href="board_list.do";

		}
	}
}
```

# 로그아웃 기능 만들기
- 로그아웃 버튼을 누르면 로그아웃을 할 수 있도록 해보자

## board_list.jsp의 로그아웃 버튼에 매핑 정해주기
```
<c:when test="${not empty id }">
	<input type="button" value="로그아웃" onclick="location.href='logout.do'">
</c:when>
```

## BoardController에서 매핑 만들기
- 세션을 제거하되 vo만 제거해주도록 하자

```java
@RequestMapping("logout.do")
public String logout() {

	session.removeAttribute("id");
	//세션에 들어있는 모든 속성을 제거한다.
	//session.invalidate();

	return "redirect:board_list.do";
}
```

# 회원가입 기능 구현하기
- 아이디 중복체크를 하며 회원을 DB에 저장해보자

## member_insert_form.jsp 생성하기
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
	<form>
		<table border="1">
			<caption>:::회원가입:::</caption>
			
			<tr>
				<th>아이디</th>
				<td>
				<input name="id" id="id">
				<input type="button" value="중복체크" onclick="check_id()";>
				</td>
			</tr>
			<tr>
				<th>이름</th>
				<td>
				<input name="name">
				</td>
			</tr>
			<tr>
				<th>비밀번호</th>

				<td>
				<input name="pwd" type="password">
				</td>
			</tr>
			<tr>
				<th>이메일</th>
				<td>
				<input name="email">
				</td>
			</tr>
			<tr>
				<td colspan="2" align="center">
					<input type="button" value="가입" onclick="send(this.form);">
					<input type="button" value="취소" onclick="location.href='board_list.do'">
				</td>
			</tr>
		</table>
	</form>
</body>
</html>

```

## 회원가입 버튼과 연결하기
- 메인의 우측상단 버튼, 로그인창에 버튼을 만든 후 연결하기

### board_list.jsp의 버튼에 연결하기
```html
<c:when test="${empty id }">
		<input type="button" value="로그인" onclick="location.href='login_form.do'">
		<input type="button" value="회원가입" onclick="location.href='m_insert.do'">
</c:when>
```

### login_form.jsp에 버튼 추가하기
```java
<td colspan="2" align="center">
	<input  type="button" value="로그인" onclick="send(this.form)">
	<input type="button" value="회원가입" onclick="location.href='m_insert.do'">
	<input type="button" value="취소" onclick="location.href='board_list.do'">
</td>
```

## BoardController에서 매핑해주기

```java
@RequestMapping("m_insert.do")
public String m_insert() {
	return Common.VIEW_PATH + "member_insert_form.jsp";
}
```

## member_insert_form.jsp 코드 추가하기
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>

	<script src="resources/js/httpRequest.js"></script>
	
	<script type="text/javascript">
		
	//아이디 중복여부 체크
	var b_idCheck = false;

	function send(f){
		//입력내용 체크
		var id = f.id.value.trim();
		var pwd = f.pwd.value.trim();

		if( !b_idCheck ){
			alert("아이디 중복체크를 하세요");	
			return;
		}

		f.action = "insert.do";
		f.submit();

	}//send()			
		
		
	//아이디 중복체크를 위한 메서드
	function check_id(){

		//아이디 중복체크
		var id = document.getElementById("id").value.trim();

		if(id == ''){
			alert("아이디를 입력하세요");
			return;
		}
		완전히 새로고침을 하면 텍스트필드에 적혀있는 것도 다 날아간다. 무한으로 중복체크만 하게 될 것이다.

		//id를 Ajax를 통해서 서버로 전송
		var url = "check_id.do";//MemberCheckIdAction.java 서블릿

		//id에 @와 같은 특수문자가 들어가 있는 경우를 대비하여 인코딩하여 보낸다.
		var param = "id=" + encodeURIComponent(id);

		sendRequest( url, param, resultFn, "POST" );

	}//check_id()
		
	function resultFn(){
		if(xhr.readyState == 4 && xhr.status == 200){
			//"[{'res':'no'}]"
			var data = xhr.responseText;
			//문자열 구조인 data를 실제 JSON형태로 변환
			var json = (new Function('return'+data)();

			if(json[0].res == 'no'){
				alert("이미 사용중인 아이디 입니다.");
				return;
			} else {
				//회원가입이 가능한 경우
				alert("사용 가능한 아이디 입니다");
				b_idCheck = true;
			}
		}
	}//resultFn()
		
	</script>
	
</head>
<body>
	<form>
		<table border="1">
			<caption>:::회원가입:::</caption>
			
			<tr>
				<th>아이디</th>
				<td>
				<input name="id" id="id">
				<input type="button" value="중복체크" onclick="check_id()";>
				</td>
			</tr>
			<tr>
				<th>이름</th>
				<td>
				<input name="name">
				</td>
			</tr>
			<tr>
				<th>비밀번호</th>

				<td>
				<input name="pwd" type="password">
				</td>
			</tr>
			<tr>
				<th>이메일</th>
				<td>
				<input name="email">
				</td>
			</tr>
			<tr>
				<td colspan="2" align="center">
					<input type="button" value="가입" onclick="send(this.form);">
					<input type="button" value="취소" onclick="location.href='board_list.do'">
				</td>
			</tr>
		</table>
	</form>
</body>
</html>

```

## BoardContorller에 매핑 만들기

```java

```







