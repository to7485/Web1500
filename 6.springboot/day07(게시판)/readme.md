# 게시판 만들기

## Ex_날짜_board 프로젝트 생성하기
- resources에 config패키지,mapper패키지 생성하기(config.xml,mapper.xml 복사하기), application.yml파일 복사하기
  - static에 img폴더 복사해서 넣기
- java에 controller,dao,vo,mybatis패키지 만들기



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
	delInfo NUMBER(2) default 0
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

## mybatis패키지에 MybatisConfig클래스 복사하기
```java
package com.korea.board.mybatis;

import java.io.IOException;

import javax.sql.DataSource;

import org.apache.ibatis.session.SqlSessionFactory;
import org.mybatis.spring.SqlSessionFactoryBean;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import com.zaxxer.hikari.HikariConfig;
import com.zaxxer.hikari.HikariDataSource;

import lombok.RequiredArgsConstructor;

@Configuration
@RequiredArgsConstructor 
public class MyBatisConfig {
    private final ApplicationContext applicationContext; 

    @ConfigurationProperties(prefix = "spring.datasource.hikari")
    @Bean
    public HikariConfig hikariConfig() {return new HikariConfig();}

    @Bean
    public DataSource dataSource() {return new HikariDataSource(hikariConfig());}

    @Bean
    public SqlSessionFactory sqlSessionFactory() throws IOException {
        SqlSessionFactoryBean sqlSessionFactoryBean = new SqlSessionFactoryBean();
        sqlSessionFactoryBean.setDataSource(dataSource());
        sqlSessionFactoryBean.setMapperLocations(applicationContext.getResources("classpath*:/mapper/*.xml"));
        sqlSessionFactoryBean.setConfigLocation(applicationContext.getResource("classpath:/config/config.xml"));

        try {
            SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBean.getObject();
            sqlSessionFactory.getConfiguration().setMapUnderscoreToCamelCase(true);
            return sqlSessionFactory;
        } catch (Exception e) {
            e.printStackTrace();
        }
        return null;
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
	private int delInfo; //삭제여부
	
	private String name; //작성자
	private String subject; //제목
	private String content; //내용
	private String pwd; //비밀번호
	private String ip; //ip
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
package com.korea.board.util;
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
		
		//String vs StringBuffer
		//String :immutable(불변)
		//기존 문자열에 이어붙히면 버리고 메모리 새로 할당받음
		//문자열이 많은경우 성능이 좋아지지 않는다.
		
		//StringBuffer : mutable(변함)
		//기존의 버퍼 크기를 늘리며 유연하게 동작합니다.
		
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
//-----그룹페이지처리 이전 --------------------------------------------------------------------------------------------		
		if(isPrevPage){
			sb.append("<a href ='"+pageURL+"?page=");
			//sb.append(nowPage - blockPage);
			sb.append( startPage-1 );
			sb.append("'><img src='/img/btn_prev.gif'></a>");
		}
		else
			sb.append("<img src='/img/btn_prev.gif'>");
		
//------페이지 목록 출력 -------------------------------------------------------------------------------------------------
		sb.append(" ");
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
				sb.append("'>");
				sb.append(i);
				sb.append("</a>");
			}
		}// end for
		
		sb.append("&nbsp; ");
		
//-----그룹페이지처리 다음 ----------------------------------------------------------------------------------------------
		if(isNextPage){
			sb.append("<a href='"+pageURL+"?page=");
			
			sb.append(endPage + 1);
			/*if(nowPage+blockPage > totalPage)nowPage = totalPage;
			else
				nowPage = nowPage+blockPage;
			sb.append(nowPage);*/
			sb.append("'><img src='/img/btn_next.gif'></a>");
		}
		else
			sb.append("<img src='/img/btn_next.gif'>");
//---------------------------------------------------------------------------------------------------------------------	    
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
		<typeAlias type="com.korea.board.vo.BoardVO" alias="boardVO"/>
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


	//@RequestParam
	//request객체의 getParameter와 동일한 기능을 한다고 보면된다.
	//@RequestParam("변수명") 넘어온 데이터를 해당 변수명으로 바인딩한다.
	//HTTP파라미터 이름이 변수 이름과 같은면 ("변수명)은 생략이 가능하다.
	//만약 원하는 값이 넘어오지 않을 때 required 옵션을 주어 true는 필수, false는 필수가 아닌것으로 설정할 수 있다.
	//required = false일때 요청 파라미터에 값이 없으면 null이 저장된다.
	//필수 입력이 아니라서 null이 들어올때 defaultValue 속성을 통해 기본값을 설정할 수 있다.
	//ex) defaultValue="1"
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
		<th:block th:each="dto : ${list}">
			<tr th:object="${dto}">
				<td align="center" th:text="*{idx}"></td>

				<td>
					<!--depth가 0보다 크면 depth의 수만큼 반복하여 띄어쓰기를 해라-->
					<th:block th:if="${dto.depth > 0}">
						<span th:each="depth : ${#numbers.sequence(1,vo.depth)}">&nbsp;</span>
					</th:block>

					<!--depth가 0이 아니면 댓글이기 때문에 ㄴ을 써라-->
					<th:block th:if="${dto.depth ne 0}">ㄴ</th:block>

					<!-- delInfo가 -1이 아니면 삭제가 안된 글이기 때문에 누를수 있게 해주자.-->
					<th:block th:if="${dto.delInfo ne -1}">
						<a th:href="@{/board/view(idx=*{idx},page=${param.page})}">
							<font color="black" th:text="*{subject}"></font>
						</a>
					</th:block>

					<!-- delInfo가 -1면 삭제된 글이기 때문에 누를수 없게 해주자.-->
					<th:block th:if="${delInfo eq -1}">
						<font color="gray" th:text="*{subject}"></font>
					</th:block>
				</td>

				<!--delInfo가 -1이면 unknown으로 나오게 아니면 작성자명이 나오게 해주자.-->
				<td th:if="${vo.delInfo eq -1}" th:text="unknown"></td>
				<td th:unless="${vo.delInfo eq -1}" th:text="*{name}"></td>
				<td th:text="${#strings.substring(vo.regdate,0,10)}"></td>
				<td th:text="*{readhit}"></td>
			</tr>
		</th:block>
		<tr>
			<td colspan="5" align="center">
				<div id="pageMenu"></div>
			</td>
		</tr>
		<tr>
			<td colspan="5" align="right">
				<a th:href="@{/board/insert_form(page=${param.page})}">
					<img src="/img/btn_reg.gif" style="cursor:pointer;">
				</a>
			</td>
		</tr>
	</table>
</body>
<script src="https://code.jquery.com/jquery-3.7.0.min.js"></script>
<script th:inline="javascript">
	//pageMenu 변수에 controller에서 넘어온 pageMenu를 담아주자.
	let pageMenu = /*[[${pageMenu}]]*/

		//pageMenu라는 id를 가진 div에 pageMenu를 넣어주자.
		$("#pageMenu").html(pageMenu)
</script>

</html>
```

# 게시글 상세보기

## BoardController에서 매핑해주기
```java

	@GetMapping("view")
	public String view(Model model,int idx, @RequestParam(required = false) String page) {
		BoardVO vo = board_dao.selectOne(idx);
		
		//조회수 중가
		String show = (String)request.getSession().getAttribute("show");
		
		if(show == null) {
			int res = board_dao.update_readhit(idx);
			request.getSession().setAttribute("show", "0");
		}
		
		//상세보기 페이지로 전환하기 위해 바인딩 및 포워딩
		model.addAttribute("vo", vo);
		return "/board/board_view";
	}
```

## board.xml에 쿼리문 작성하기
```xml
<!--idx에 해당하는 게시글 정보 조회(상세보기)-->
<select id="board_one" resultType="boardVO">
	select * from board where idx = #{idx}
</select>

<!--조회수 업데이트-->
<update id="update_readhit">
	update board set readhit = readhit + 1
	where idx = #{idx}
</update>
```

## BoardMapper 인터페이스에 메서드 작성하기
```java
//게시글 한 건 조회
public BoardVO board_one(int idx);

//조회수 변경하기
public int update_readhit(int idx);
```

## BoardDAO에 메서드 작성하기
```java
//게시글 한건 조회
public BoardVO selectOne(int idx) {
	return boardMapper.board_one(idx);
}

//조회수 증가
public int update_readhit(int idx) {
	return boardMapper.update_readhit(idx);
}
```

## board_view.html 생성하기
```html
<!DOCTYPE html>
<html>

<head>
	<meta charset="UTF-8">
	<title>Insert title here</title>
</head>

<body>
	<table border="1">

		<caption>:::게시글 상세보기:::</caption>
		<tr>
			<th>제목</th>
			<td th:text="${vo.subject}"></td>
		</tr>
		<tr>
			<th>작성자</th>
			<td th:text="${vo.name}"></td>
		</tr>
		<tr>
			<th>작성일</th>
			<td th:text="${vo.regdate}"></td>
		</tr>
		<tr>
			<th>ip</th>
			<td th:text="${vo.ip}"></td>
		</tr>
		<tr>
			<th>내용</th>
			<td width="500px" height="200px" th:text="${vo.content}"></td>
		</tr>
		<tr>
			<th>비밀번호</th>
			<td><input type="password" id="c_pwd"></td>
		</tr>
		<tr>
			<td colspan="2">
				<!--목록보기-->
				<a th:href="@{/board/board_list(page=${param.page})}">
					<img src="/img/btn_list.gif">
				</a>
				<th:block th:if="${vo.depth lt 1 }">
					<!-- 답변  -->
					<img src="/img/btn_reply.gif" onclick="reply();">
				</th:block>
				<!-- 삭제 -->
				<img src="/img/btn_delete.gif" onclick="del();">
			</td>
		</tr>
	</table>
</body>

</html>
```
# 새글 추가하기

## BoardController에 매핑해주기
```java
@GetMapping("insert_form")
public String insert_form(Model model,@RequestParam(required = false) String page) {
	model.addAttribute("vo", new BoardVO());
	return "/board/insert_form";
}
```
## insert_form.html 생성하기
```html
<!DOCTYPE html>
<html>

<head>
	<meta charset="EUC-KR">
	<title>Insert title here</title>
</head>

<body>
	<form name="f" th:action="@{/board/insert}" th:object="${vo}" method="get">
		 <input type="hidden" name="page" th:value="${param.page}"/> //BoardController에서 넘어온 page 받아주기
		<table border="1">
			<caption>:::새 글 쓰기:::</caption>

			<tr>
				<th>제목</th>
				<td><input th:field="*{subject}" style="width:370px;"></td>
			</tr>
			<tr>
				<th>작성자</th>
				<td><input th:field="*{name}" style="width:370px;"></td>
			</tr>
			<tr>
				<th>내용</th><!-- 가로로 50글자 세로로 엔터 10번정도 칠수 있는 크기 -->
				<td><textarea th:field="*{content}" rows="10" cols="50" style="resize:none;"></textarea></td>
			</tr>
			<tr>
				<th>비밀번호</th>
				<td><input th:field="*{pwd}" type="password"></td>
			</tr>
			<tr>
				<td colspan="2">
					<img src="/img/btn_reg.gif" id="send_check" style="cursor:pointer;">
					<a th:href="@{/board/board_list(page=${param.page})}">
						<img src="/img/btn_back.gif" style="cursor:pointer;">
					</a>
				</td>
			</tr>
		</table>
	</form>
</body>
<script src="https://code.jquery.com/jquery-3.7.0.min.js"></script>
<script>
	const $sendBtn = $("img#send_check");

	$sendBtn.on("click", function () {
		$("form[name='f']").submit();
	});
</script>
</html>
```

## BoardController에 insert매핑 만들어주기
```java
@GetMapping("insert")
public RedirectView insert(BoardVO vo,@RequestParam(required = false) String page) {
	String ip = request.getRemoteAddr();
	vo.setIp(ip);
	
	int res = board_dao.insert(vo);
	
	if(res > 0) {
		return new RedirectView("/board/board_list?page="+page);
	}
	return null;
}
```
## board.xml에 쿼리문 추가하기
```xml
<!-- 새글 추가 -->
<insert id="board_insert">
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
## BoardMapper에 메서드 작성하기
```java
//게시글 추가하기
public int board_insert(BoardVO vo);
```

## BoardDAO에 메서드 추가하기
```java
//게시글 추가
public int board_insert(BoardVO vo) {
	return boardMapper.board_insert(vo);
}
```

![image](https://github.com/to7485/Web1500/assets/54658614/7a7a2566-0f38-45bf-8969-0c8f9e31596c)


## 삭제하기
- 삭제 버튼을 누르면 삭제된것 처럼 보이게 하기

### board_view.html에 메서드 작성하기
```html
		<tr>
			<th>비밀번호</th>
			<td><input type="password" id="c_pwd"></td>
		</tr>
		<tr>
			<td colspan="2">
				<!--목록보기-->
				<a th:href="@{/board/board_list(page=${param.page})}">
					<img src="/img/btn_list.gif">
				</a>
				<th:block th:if="${vo.depth lt 1 }">
					<!-- 답변  -->
					<img src="/img/btn_reply.gif" onclick="reply();">
				</th:block>
				<!-- 삭제 -->
				<img src="/img/btn_delete.gif" id="del" style="cursor:pointer;">
			</td>
		</tr>
	</table>
</body>
<script src="https://code.jquery.com/jquery-3.7.0.min.js"></script>
<script th:inline="javascript">
	let idx = /*[[${vo.idx}]]*/''
	let pwd =  /*[[${vo.pwd}]]*/''
	let page =  /*[[${param.page}]]*/''



	$("img[id='del']").on("click", function () {
		if (!confirm("삭제하시겠습니까?")) {
			return;
		}


		let $c_pwd = $("input[type='password']").val();

		if ($pwd != $c_pwd) {
			alert("비밀번호 불일치");
			return;
		}

		$.ajax({
	            url: '/board/del',
	            type: 'POST',
	            data: JSON.stringify({'idx': $idx}),
	            dataType: "json",
	            contentType: "application/json; charset=utf-8",
	            success: function (data) {
	                alert(data["param"])
	                window.location.href = "/board/board_list?page=" + $page;
	            }
	        })
	})
</script>

</html>
```

### BoardController에 매핑 만들기
```java
@PostMapping("del")
    @ResponseBody
    public String del(@RequestBody String body) {
        ObjectMapper om = new ObjectMapper();

        Map<String, String> data = null;

        try {
            data = om.readValue(body, new TypeReference<Map<String, String>>() {
            });
        } catch (Exception e) {
            e.printStackTrace();
        }
        
        
        int intIdx = Integer.parseInt(data.get("idx"));
        
        BoardDTO dto = boardDAO.selectOne(intIdx);
        
        dto.setSubject("deleted");
        dto.setName("unknown");
        
        int res = boardDAO.del_update(dto);
        
        if(res > 0) {
            return "{\"param\": \"success\"}";
        }
        return "{\"param\": \"fail\"}";
    }
```

### board.xml에 쿼리문 작성하기
```xml
<!-- 게시글 삭제(된 것 처럼 업데이트) -->
<update id="del_update">
	update board set subject = #{subject},
	name = #{name},
	delInfo = -1
	where idx=#{idx}
</update>
```

### BoardMapper에 메서드 작성하기
```java
//게시글 삭제
public int del_update(BoardVO vo);
```

### BoardDAO에 메서드 작성하기
```java
//게시글 삭제
public int del_update(BoardVO vo) {
	return boardMapper.del_update(vo);
}
```

## 답글 달기

### board_view.html 수정하기
- reply()메서드 작성하기
```html
function reply(){
	location.href="reply_form?idx=${vo.idx}&page=${param.page}";
}
```

### boardController에 매핑하기
```java
@GetMapping("reply_form")
public String reply_form(int idx, int page, Model model) {
	model.addAttribute("dto",new BoardDTO);
	return "reply_from.jsp?idx="+idx+"&page="+page;
}
```

### reply_form.html 생성하기
```html
<!DOCTYPE html>
<html>

<head>
	<meta charset="EUC-KR">
	<title>Insert title here</title>
	<script>
	function send_check(){
		var f = document.f;
		f.submit();
	}
	</script>
</head>

<body>
	<form name="f" th:action="@{/board/reply}" th:object="${dto}" method="get">
		 <input type="hidden" name="page" th:value="${param.page}"/> //BoardController에서 넘어온 page 받아주기
		<table border="1">
			<caption>:::새 글 쓰기:::</caption>

			<tr>
				<th>제목</th>
				<td><input th:field="*{subject}" style="width:370px;"></td>
			</tr>
			<tr>
				<th>작성자</th>
				<td><input th:field="*{name}" style="width:370px;"></td>
			</tr>
			<tr>
				<th>내용</th><!-- 가로로 50글자 세로로 엔터 10번정도 칠수 있는 크기 -->
				<td><textarea th:field="*{content}" rows="10" cols="50" style="resize:none;"></textarea></td>
			</tr>
			<tr>
				<th>비밀번호</th>
				<td><input th:field="*{pwd}" type="password"></td>
			</tr>
			<tr>
				<td colspan="2">
					<img src="/img/btn_reg.gif" id="send_check" style="cursor:pointer;">
					<a th:href="@{/board/board_list(page=${param.page})}">
						<img src="/img/btn_back.gif" style="cursor:pointer;">
					</a>
				</td>
			</tr>
		</table>
	</form>
</body>
<script src="https://code.jquery.com/jquery-3.7.0.min.js"></script>
<script>
	const $sendBtn = $("img#send_check");

	$sendBtn.on("click", function () {
		$("form[name='f']").submit();
	});
</script>
</html>
```

### BoardController에 매핑하기
```java
@GetMapping("reply")
public RedirectView reply(BoardDTO dto, int idx, @RequestParam(defaultValue= "1")int page) {
	String ip = request.getRemoteAddr();
	
	//기준글의 idx를 이용하여 댓글을 달고싶은 게시글의 정보 가져오기
	BoardDTO base_dto = board_dao.selectOne(idx);
	
	//기준글에 step 이상값은 step = step + 1 처리
	int res = board_dao.update_step(base_dto);
	
	dto.setIp(ip);
	
	//답글이 들어갈 위치 선정
	dto.setRef(base_dto.getRef());
	dto.setStep(base_dto.getStep()+1);
	dto.setDepth(base_dto.getDepth()+1);
	
	res = board_dao.reply(dto);
	
	return new RedirectView("/board/board_list?page="+page);
}
```

### BoardMapper에 메서드 만들기
```java
//step증가
public int board_update_step(BoardDTO dto);

//답글 추가
public int board_reply(BoardDTO dto);
```

### BoardDAO에 연결하기
```java
//step 증가
public int update_step(BoardDTO dto) {
	int res = boardMapper.board_update_step(dto);
	return res;
}

//답글추가
public int reply(BoardDTO dto) {
	int res = boardMapper.board_reply(dto);
	return res;
}
```

### 쿼리문 추가하기
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

## 로그인 기능 만들기
### board_list.html에 코드 추가하기
```html
<table border="1" width="700">
<tr>
	<td colspan="5" align="right">
		<th:block th:if="${session.id == null}">
			<input type="button" value="로그인" name="login_form">
			<input type="button" value="회원가입" name="join">
		</th:block>
		<th:block th:unless="${session.id == null}">
			<input type="button" value="로그아웃" name="logout">
		</th:block>
</tr>
<tr>
	<td colspan="5"><img src="/img/title_04.gif"></td>
</tr>

js코드 추가
let $loginFormButton = $("input[name='login_form']")
	
	$loginFormButton.on("click", function(){
		window.location.href="/board/login_form";
	})
```

### BoardController에 코드 매핑해주기
```java
@GetMapping("login_form")
public String login_form(@ModelAttribute("dto") MemberDTO dto) {
	return "/board/login_form";
}
```

### login_form.html 작성하기
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>Insert title here</title>
</head>
<body>
	<form th:action="@{/board/login}" th:object="${dto}" method="get">
		<table border="1" align="center">
			<caption>:::로그인:::</caption>
			<tr>
				<th>아이디</th>
				<td><input th:field="*{id}"></td>
			</tr>
			<tr>
				<th>비밀번호</th>
				<td><input th:field="*{pwd}" type="password"></td>
			</tr>
			<tr>
				<td colspan="2" align="center">
					<input type="button" value="로그인" name="login">
				</td>
			</tr>
		</table>
	</form>
</body>
<script src="https://code.jquery.com/jquery-3.7.0.min.js"></script>
<script>
	let $loginButton = $("input[name='login']")

	
	$loginButton.on("click", function(){
		let $id = $("input[name='id']").val()
		let $pwd = $("input[name='pwd']").val()
		
		if($id == ''){
			alert("아이디를 입력해주세요")
			return;
		}
		
		if($pwd == ''){
			alert("비밀번호를 입력해주세요")
			return;
		}
		
		
		$.ajax({
			url:'/board/login',
			type:'POST',
			data:JSON.stringify({'id' : $id, 'pwd' : $pwd}),
			dataType:'json',
			contentType:'application/json; charset=utf-8',
			success: function(data){
				if(data["param"] == 'no_id'){
					alert('아이디가 존재하지 않습니다.');
					return;
				} else if(data["param"] == 'no_pwd'){
					alert('비밀번호가 일치하지 않습니다.');
					return;
				} else {
					alert('로그인 성공');
					window.location.href = "/board/board_list"
				}
				
				
			}
		})
		
	})
</script>
</html>
```

### BoardController에 매핑하기
```java
@PostMapping("login")
@ResponseBody
public String login(@RequestBody String body) {
	ObjectMapper om = new ObjectMapper();
	
	Map<String, String> data = null;
	
	try {
		data = om.readValue(body, new TypeReference<Map<String,String>>(){
		});
	} catch (Exception e) {
		// TODO: handle exception
	}
	
	String id = data.get("id");
	String pwd = data.get("pwd");
	
	MemberDTO dto = member_dao.loginCheck(id);
	
	if(dto == null) {
		return "{\"param\":\"no_id\"}";
	}
	
	if(!dto.getPwd().equals(pwd)) {
		return "{\"param\":\"no_pwd\"}";
	}
	
	session.setAttribute("id", dto);
	
	return "{\"param\":\"clear\"}";
	
}
```

### MemberDAO 생성하기
```java
package com.korea.board.dao;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Repository;

import com.korea.board.dto.MemberDTO;
import com.korea.board.mapper.MemberMapper;

@Repository

public class MemberDAO {

	@Autowired
	private MemberMapper memberMapper;
	
	//로그인체크
	public MemberDTO loginCheck(String id) {
		return memberMapper.loginCheck(id);
	}	
}
```

### MemberMapper 생성하기
```java
package com.korea.board.mapper;

import org.apache.ibatis.annotations.Mapper;

import com.korea.board.dto.MemberDTO;

@Mapper
public interface MemberMapper {

	//로그인체크
	public MemberDTO loginCheck(String id);
}
```

### member.xml 생성하기
```xml
<?xml version="1.0" encoding="UTF-8"?>
	  
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0/EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.korea.board.mapper.MemberMapper">

		<select id="loginCheck">
			select * from member where id=#{id}
		</select>
</mapper>
```
## 로그아웃하기
### board_list.html에 코드 작성하기
```html
//로그아웃버튼
let $logoutButton = $("input[name='logout']")

$logoutButton.on("click", function(){
	window.location.href='/board/logout';
})
```

### BoardController에 매핑 잡아주기
```java
@GetMapping("logout")
public RedirectView logout() {
	session.removeAttribute("id");
	return new RedirectView("/board/board_list");
}
```




