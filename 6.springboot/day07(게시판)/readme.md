# 게시판 만들기

## Ex_날짜_board 프로젝트 생성하기
- resources에 config패키지,mapper패키지 생성하기(config.xml,mapper.xml 복사하기)
  - static에 img폴더 복사해서 넣기
- java에 controller,dao,vo패키지 만들기



## 게시판DB생성하기
```sql
--일련번호
create sequence seq_board_idx;

--게시판 DB
CREATE TABLE board(
	idx number(3) primary key,      --게시판 일련번호
	name varchar2(100) not null,    --작성자
	subject varchar2(255) not null,  --게시판제목   
	content CLOB,                   --내용(씨롭.CharLargeObject)
	pwd varchar2(100),              --비번
	ip varchar2(100),                --ip
	regdate date,                    --작성일자
	readhit number(3) default 0,     --조회수
	--계층형 게시판을 운영하기 위한 추가정보들
	ref int,  --기준글번호(댓글을 달기위한 메인글)
	step int, --댓글순서(수직적)
	depth int, --댓글의깊이(댓글의 댓글 개념)
	del_info NUMBER(2) default 0
);

--샘플데이터 추가(메인글1 -> 댓글1)
--새글쓰기
insert into board values(
		seq_board_idx.nextVal, 
		'이름-일길동', 
		'제목-내가일등!',
		'내용-..는꿈', 
		'1234', 
		'192.0.0.1', 
		sysdate, 
		0, 
		seq_board_idx.currVal,  -- 현재 시퀀스값 
		0, 
		0,
		0);

--댓글쓰기
insert into board values(
		seq_board_idx.nextVal, 
		'이름-이길동', 
		'제목-난2등ㅋ',
		'내용-ㅋㅋㅋㅋㅋㅋ', 
		'1234', 
		'192.0.0.2', 
		sysdate, 
		0, 
		1, --메인글의 idx
		1, -- step
		1, -- depth
		0
);

--댓글의 댓글
insert into board values(
		seq_board_idx.nextVal, 
		'삼길동', 
		'오예3등',
		'난삼등', 
		'1234', 
		'192.0.0.3', 
		sysdate, 
		0, 
		1, --메인글의 idx
		2, -- step
		2, -- depth
		0
);

commit;

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
## common패키지 만들고 Common클래스 생성하기
```java
package com.example.boatd.common;

public class Common {
	//객체 관리를 편하게 하기 위한 클래스

	//일반게시판과 공지사항에서
	//한 페이지당 보여줘야 하는 갯수가 다른경우에 다음과 같이 
	//게시판별로 상수화 시켜놓으면 관리하기 편해진다.

	//일반게시판용 상수
	public static class Board{
		
		//한 페이지당 보여줄 게시물 수
		public final static int BLOCKLIST = 10;

		//한 화면에 보여지는 페이지 메뉴 수
		//<- 1 2 3 4 5 ->
		public final static int BLOCKPAGE = 5;
	}

	//댓글의 페이징 처리를 위한 LIST
	public static class Comment{
		//한 페이지당 보여줄 게시물 수
		public final static int BLOCKLIST = 5;

		//한 화면에 보여지는 페이지 메뉴 수
		//<- 1 2 3 4 5 ->
		public final static int BLOCKPAGE = 5;
	}
}

```

## util패키지에 Paging 클래스
```java
package com.example.boatd.util;
/*
        nowPage:현재페이지
        rowTotal:전체데이터갯수
        blockList:한페이지당 게시물수
        blockPage:한화면에 나타낼 페이지 메뉴 수
 */
public class Paging {
	public static String getPaging(String pageURL,int nowPage, int rowTotal,int blockList, int blockPage){
		
		int totalPage/*전체페이지수*/,
            startPage/*시작페이지번호*/,
            endPage;/*마지막페이지번호*/

		boolean  isPrevPage,isNextPage;
		StringBuffer sb; //모든 상황을 판단하여 HTML코드를 저장할 곳
		
		
		isPrevPage=isNextPage=false;
		//입력된 전체 자원을 통해 전체 페이지 수를 구한다..
		totalPage = (int)(rowTotal/blockList);
		if(rowTotal%blockList!=0)totalPage++;
		

		//만약 잘못된 연산과 움직임으로 인하여 현재 페이지 수가 전체 페이지 수를
		//넘을 경우 강제로 현재페이지 값을 전체 페이지 값으로 변경
		if(nowPage > totalPage)nowPage = totalPage;
		

		//시작 페이지와 마지막 페이지를 구함.
		startPage = (int)(((nowPage-1)/blockPage)*blockPage+1);
		endPage = startPage + blockPage - 1; //
		
		//마지막 페이지 수가 전체페이지수보다 크면 마지막페이지 값을 변경
		if(endPage > totalPage)endPage = totalPage;
		
		//마지막페이지가 전체페이지보다 작을 경우 다음 페이징이 적용할 수 있도록
		//boolean형 변수의 값을 설정
		if(endPage < totalPage) isNextPage = true;
		//시작페이지의 값이 1보다 작으면 이전페이징 적용할 수 있도록 값설정
		if(startPage > 1)isPrevPage = true;
		
		//HTML코드를 저장할 StringBuffer생성=>코드생성
		sb = new StringBuffer();
//-----그룹페이지처리 이전 --------------------------------------------------------------------------		
		if(isPrevPage){
			sb.append( "<a href='#' onclick='comment_list(" );
			sb.append( startPage-1 );
			sb.append( ");'>◀</a>" );
		}
		else
			sb.append("◀");
		
//------페이지 목록 출력 ---------------------------------------------------------------------------
		sb.append("|");
		for(int i=startPage; i<= endPage ;i++){
			if(i>totalPage)break;
			if(i == nowPage){ //현재 있는 페이지
				sb.append("&nbsp;<b><font color='#91b72f'>");
				sb.append(i);
				sb.append("</font></b>");
			}
			else{//현재 페이지가 아니면
				sb.append( "<a href='#' onclick='comment_list(" );
				sb.append(i);
				sb.append( ");'>&nbsp;" );
				sb.append(i);
				sb.append( "&nbsp;</a>" );
			}
		}// end for
		
		sb.append("&nbsp;|");
		
//-----그룹페이지처리 다음 ----------------------------------------------------------------------------------
		if(isNextPage){
			sb.append( "<a href='#' onclick='comment_list(" );
			sb.append(endPage + 1);
			sb.append( ");'>▶</a>" );
		}
		else
			sb.append("▶");
//------------------------------------------------------------------------	    

		return sb.toString();
	}
	
public static String getPaging(String pageURL,int nowPage, int rowTotal, String search_param, int blockList, int blockPage){
		
		int totalPage/*전체페이지수*/,
            startPage/*시작페이지번호*/,
            endPage;/*마지막페이지번호*/

		boolean  isPrevPage,isNextPage;
		StringBuffer sb; //모든 상황을 판단하여 HTML코드를 저장할 곳
		
		
		isPrevPage=isNextPage=false;
		//입력된 전체 자원을 통해 전체 페이지 수를 구한다..
		totalPage = (int)(rowTotal/blockList);
		if(rowTotal%blockList!=0)totalPage++;
		

		//만약 잘못된 연산과 움직임으로 인하여 현재 페이지 수가 전체 페이지 수를
		//넘을 경우 강제로 현재페이지 값을 전체 페이지 값으로 변경
		if(nowPage > totalPage)nowPage = totalPage;
		

		//시작 페이지와 마지막 페이지를 구함.
		startPage = (int)(((nowPage-1)/blockPage)*blockPage+1);
		endPage = startPage + blockPage - 1; //
		
		//마지막 페이지 수가 전체페이지수보다 크면 마지막페이지 값을 변경
		if(endPage > totalPage)endPage = totalPage;
		
		//마지막페이지가 전체페이지보다 작을 경우 다음 페이징이 적용할 수 있도록
		//boolean형 변수의 값을 설정
		if(endPage < totalPage) isNextPage = true;
		//시작페이지의 값이 1보다 작으면 이전페이징 적용할 수 있도록 값설정
		if(startPage > 1)isPrevPage = true;
		
		//HTML코드를 저장할 StringBuffer생성=>코드생성
		sb = new StringBuffer();
//-----그룹페이지처리 이전 --------------------------------------------------------------------------------		
		if(isPrevPage){
			sb.append("<a href ='"+pageURL+"?page=");
			//sb.append(nowPage - blockPage);
			sb.append( startPage-1 );
			sb.append("&"+search_param);
			sb.append("'>◀</a>");
		}
		else
			sb.append("◀");
		
//------페이지 목록 출력 ---------------------------------------------------------------------------------
		sb.append("|");
		for(int i=startPage; i<= endPage ;i++){
			if(i>totalPage)break;
			if(i == nowPage){ //현재 있는 페이지
				sb.append("&nbsp;<b><font color='#91b72f'>");
				sb.append(i);
				sb.append("</font></b>");
			}
			else{//현재 페이지가 아니면
				sb.append("&nbsp;<a href='"+pageURL+"?page=");
				sb.append(i);
				sb.append("&"+search_param);
				sb.append("'>");
				sb.append(i);
				sb.append("</a>");
			}
		}// end for
		
		sb.append("&nbsp;|");
		
//-----그룹페이지처리 다음 --------------------------------------------------------------------------------
		if(isNextPage){
			sb.append("<a href='"+pageURL+"?page=");
			
			sb.append(endPage + 1);
			
			sb.append("&"+search_param);
			
			/*if(nowPage+blockPage > totalPage)nowPage = totalPage;
			else
				nowPage = nowPage+blockPage;
			sb.append(nowPage);*/
			sb.append("'>▶</a>");
		}
		else
			sb.append("▶");
//-------------------------------------------------------------------------------------------------	    

		return sb.toString();
	}	
}
```


## mapper패키지에 BoardMapper 클래스 생성하기
```java
package com.example.boatd.mapper;

import org.apache.ibatis.annotations.Mapper;

@Mapper
public interface BoardMapper {

}

```

## dao패키지에 BoardDAO클래스 생성하기
```java
package com.example.boatd.dao;

import org.springframework.stereotype.Repository;

import com.example.boatd.mapper.BoardMapper;

import lombok.RequiredArgsConstructor;

@Repository
@RequiredArgsConstructor
public class BoardDAO {

	private final BoardMapper boardMapper;
	
	
}

```

## controller패키지에 BoardController클래스 만들기

```java
package com.example.boatd.controller;

import org.springframework.stereotype.Controller;

import com.example.boatd.dao.BoardDAO;

import lombok.RequiredArgsConstructor;

@Controller
@RequiredArgsConstructor
public class BoardController {
	
	private final BoardDAO board_dao;
	
	
	
}

```

# 조회하기
- 페이지별 게시물 조회하기

## BoardMapper에 메서드 작성하기

```java
package com.example.boatd.mapper;

import java.util.HashMap;
import java.util.List;

import org.apache.ibatis.annotations.Mapper;

import com.example.boatd.vo.BoardVO;

@Mapper
public interface BoardMapper {

	//페이지 게시물 조
	public List<BoardVO> selectList(HashMap<String, Integer> map);
	
	//전체 게시물 수 조회
	public int getRowTotal();
}

```

## config.xml에 별칭 작성해주기

```xml
<?xml version="1.0" encoding="UTF-8"?>
	  
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0/EN" "https://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<typeAliases>
		<typeAlias type="com.korea.board.vo" alias="boardVO"/>
	</typeAliases>
</configuration>
```

## board.xml에 쿼리문 작성하기
```xml
<?xml version="1.0" encoding="UTF-8"?>
	  
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0/EN" "https://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.korea.board.mapper.BoardMapper">
	<!-- 페이지별 게시물 조회 -->
	<select id="selectList" resultType="boardVO">
		select * from
		(select rank() over(order by ref desc,step) no, b.*
		from board b)
		where no between #{start} and #{end}
	</select>

	<!-- 전체 게시물 개수 조회 -->
	<select id="getRowTotal" resultType="int">
		select count(*) from board
	</select>
</mapper>
```

## BoardDAO에 메서드 작성하기
```java
package com.example.boatd.dao;

import java.util.HashMap;
import java.util.List;

import org.springframework.stereotype.Repository;

import com.example.boatd.mapper.BoardMapper;
import com.example.boatd.vo.BoardVO;

import lombok.RequiredArgsConstructor;

@Repository
@RequiredArgsConstructor
public class BoardDAO {

	private final BoardMapper boardMapper;
	
	public List<BoardVO> selectList(HashMap<String,Integer> map){
		return boardMapper.selectList(map);
	}
	
	public int getRowTotal() {
		return boardMapper.getRowTotal();
	}
}

```

## BoardController에 매핑 잡아주기

```java
import jakarta.servlet.http.HttpServletRequest;
import lombok.RequiredArgsConstructor;

@Controller
@RequiredArgsConstructor
@RequestMapping("/board/*")
public class BoardController {
	
	private final BoardDAO board_dao;
	
private final HttpServletRequest request;
	
	@GetMapping("board_list")
	public String list(Model model, @RequestParam("page") String page) {
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
				"board_list",
				nowPage, //현재 페이지 번호
				rowTotal, //전체 게시물 수
				Common.Board.BLOCKLIST, //한 페이지에 표기할 게시물 수
				Common.Board.BLOCKPAGE); //페이지 메뉴 수
		
		request.getSession().removeAttribute("show");
		
		model.addAttribute("list",list);
		model.addAttribute("pageMenu",pageMenu);
		
		return "board_list";	
	}
}	
```

## tamplates 에 board패키지 생성후 board_list.html 생성하기
```html
<!DOCTYPE html>
<html>

<head>
	<meta charset="UTF-8">
	<title>Insert title here</title>
	<style>
		a {
			text-decoration: none;
		}

		table {
			border-collapse: collapse;
		}
	</style>
</head>

<body>
	<table border="1" width="700">
		<tr>
			<td colspan="5"><img src="/img/title_04.gif"></td>
		</tr>
		<tr>
			<th>번호</th>
			<th width="350">제목</th>
			<th width="120">작성자</th>
			<th width="100">작성일</th>
			<th width="50">조회수</th>
		</tr>
		<th:block th:each="vo : ${list}">
			<tr th:object="${vo}">
				<td align="center" th:text"*{idx}" ></td>
				<td>
				<th:block th:each="depth : ${#numbers.sequence(1,*{depth})">&nbsp;</th:block>
				<th:block th:if="${*{depth} ne 0}">ㄴ</th:block>
				<th:block th:if="${*{del_info} ne -1}">
					<a href="/view?idx=*{idx}&page=*{page}">
						<font color="black" th:text="*{subject}"></font>
					</a>
				</th:block>
				<th:block th:if="${*{del_info} eq -1}">
					<font color="gray" th:text="*{subject}"></font>
				</th:block>
				</td>
				<td th:text="${name}"></td>
				<td th:if="${*{del_info} ne -1}" th:text="${#temporals.format(*{regdate},'yyyy.MM.dd')}"></td>
				<td th:if="${*{del_info} eq -1}"> unknown</td>
				<td th:text="*{readhit}"></td>
			</tr>
		</th:block>
		<tr>
			<td colspan="5" align="center" th:text="${pageMenu}"></td>
		</tr>
		<tr>
			<td colspan="5" align="right">
				<img src="/img/btn_reg.gif" onclick="location.href='insert_form'" style="cursor:pointer;">
			</td>
		</tr>
	</table>
</body>

</html>
```
