# 게시판 만들기
- 게시판을 만들고 댓글처리 및 페이징 기능들을 추가해보도록 하자.

## 게시판 DB생성하기
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
	depth int --댓글의깊이(댓글의 댓글 개념)
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
		1 -- depth
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
		2 -- depth
);

commit;
```

## ref,step,depth에 대한 설명(댓글이 생성되는 원리)
- 계층형 게시판은 기본적으로 다음과같은 정렬구조를 갖는다.
- 게시판에서 최신글 순서로 보고싶다면... ref는 큰 값(최신글)기준, step은 작은값 기준
```sql
select * from board order by ref desc, step asc;
```
![image](https://user-images.githubusercontent.com/54658614/235058338-53fedc83-5977-440d-900e-b12a1797794e.png)

## Ex_날짜_board 프로젝트 생성하기
- DB,Mybatis,Ajax등의 환경을 구성하기 위한 설정파일 및 라이브러리 추가하기

![image](https://user-images.githubusercontent.com/54658614/235058401-67223fa7-7343-4f0f-95d5-da4c155466ae.png)


## vo패키지에 BoardVO클래스 작성하기
```java
package vo;

public class BoardVO {
	private int idx; //게시글 번호
	private int readhit;//조회수
	private int ref;//참조글번호
	private int step;//댓글순서
	private int depth;//댓글깊이
	
	private String name;//작성자
	private String subject;//제목
	private String content;//내용
	private String pwd;//비밀번호
	private String ip;//ip
	private String regdate;//등록날짜
	
	public int getIdx() {
		return idx;
	}
	public void setIdx(int idx) {
		this.idx = idx;
	}
	public int getReadhit() {
		return readhit;
	}
	public void setReadhit(int readhit) {
		this.readhit = readhit;
	}
	public int getRef() {
		return ref;
	}
	public void setRef(int ref) {
		this.ref = ref;
	}
	public int getStep() {
		return step;
	}
	public void setStep(int step) {
		this.step = step;
	}
	public int getDepth() {
		return depth;
	}
	public void setDepth(int depth) {
		this.depth = depth;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public String getSubject() {
		return subject;
	}
	public void setSubject(String subject) {
		this.subject = subject;
	}
	public String getContent() {
		return content;
	}
	public void setContent(String content) {
		this.content = content;
	}
	public String getPwd() {
		return pwd;
	}
	public void setPwd(String pwd) {
		this.pwd = pwd;
	}
	public String getIp() {
		return ip;
	}
	public void setIp(String ip) {
		this.ip = ip;
	}
	public String getRegdate() {
		return regdate;
	}
	public void setRegdate(String regdate) {
		this.regdate = regdate;
	}	
}
```
## dao패키지에 BoardDAO클래스 생성하기
```java
package dao;

import java.util.List;

import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;

import service.MyBatisConnector;
import vo.BoardVO;

public class BoardDAO {

//마이바티스 서비스를 사용하기 위한 SqlSessionFactory얻어오기
	SqlSessionFactory factory;

	// single-ton pattern: 
	// 객체1개만생성해서 지속적으로 서비스하자
	static BoardDAO single = null;

	public static BoardDAO getInstance() {
		//생성되지 않았으면 생성
		if (single == null)
			single = new BoardDAO();
		//생성된 객체정보를 반환
		return single;
	}

	//생성자
	public BoardDAO() {
		super();
		//MyBatisConnector.java에서 설정해준 factory(어떤DB를 쓸것인가, mapper로 누구를 쓸것인가 등)를 DAO로 가져온다.
		factory = MyBatisConnector.getInstance().getFactory();
	}

	//전체 게시물 조회
	public List<BoardVO> selectList(){

		List<BoardVO> list = null;	

		SqlSession sqlSession = factory.openSession();

		list = sqlSession.selectList( "b.board_list" );

		sqlSession.close();

		return list;

	}//selectList()
}
```

## board.xml에 select문 추가하기
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="b">
	<select id="board_list" resultType="board">
		select * from board order by ref desc, step asc
	</select>
</mapper>
```

## sqlMapConfig.xml에 매퍼 추가하기
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

	별칭을 주면 매퍼에서 패키지 이름과 같이 쓰지 않고 별칭으로 쓸수 있다.
	
	<typeAliases>
		<typeAlias type="vo.BoardVO" alias="board"/>
	</typeAliases>

	<environments default="">
		<environment id="">
			<transactionManager type="JDBC" />
			<dataSource type="JNDI">
				<property name="data_source" 
				value="java:comp/env/jdbc/oracle_test" />
			</dataSource>
		</environment>
	</environments>
	<mappers>
		<mapper resource="config/mybatis/mapper/board.xml" />

	</mappers>
</configuration>
```

## action패키지에 BoardListAction서블릿 생성하기

```java
package action;

import java.io.IOException;
import java.util.List;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import dao.BoardDAO;
import vo.BoardVO;

/**
 * Servlet implementation class BoardListAction
 */
@WebServlet("/board_list.do")
public class BoardListAction extends HttpServlet {
	private static final long serialVersionUID = 1L;

	/**
	 * @see HttpServlet#service(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		request.setCharacterEncoding("utf-8");
		
		//전체목록 가져오기
		List<BoardVO> list = BoardDAO.getInstance().selectList();
		
		//바인딩
		request.setAttribute("list", list);
		
		RequestDispatcher disp = request.getRequestDispatcher("board_list.jsp");
		disp.forward(request, response);
			
	}

}
```
## board_list.jsp 생성하기
```
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
				<td><font color="black">${vo.subject }</td>
				<td>${vo.name }</td>
				<td>${vo.regdate}</td> 시간을 생략해주는 코드
				<td>${vo.readhit }</td>
			</tr>
	</table>
</body>
```

## board_list.jsp 파일 보기좋게 수정하기
- depth가 있는경우 댓글로 인식을 할 수 있다.
- 댓글인 경우 들여쓰기를 적용해서 보여주자
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib prefix="fn" uri="http://java.sun.com/jsp/jstl/functions" %>
<!DOCTYPE html>
<html>

<head>
<title>목록보기</title>


<meta http-equiv="Content-Type" content="text/html;">
<style>
	a{text-decoration:none;}
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
					<!-- 댓글기호 --> 심심하니까 ㄴ 하나 넣어주자
					<c:if test="${vo.depth ne 0 }">ㄴ</c:if>

					<a href="view.do?idx=${vo.idx}">
					<font color="black">${vo.subject }</font>
					</a>
				</td>

				<td>${vo.name }</td>
				<td>${ fn:split(vo.regdate, ' ')[0] }</td> 시간을 생략해주는 코드
				<td>${vo.readhit }</td>
			</tr>
			</c:forEach>
			<tr>
				<td colspan="5" align="center">
			나중에 페이징 처리 해줄껀데 미리 자리를 잡아놓기 위해 넣어주자
					◀ 1 2 3 ▶
				</td>
			</tr>

			<tr>
				<td colspan="5" align="right">
					<img src="img/btn_reg.gif"
					 onclick="location.href='insert_form.jsp'"
					 style="cursor:pointer;"> 
					이미지 위에 마우스를 올리면 손모양으로 바뀜
				</td>
			</tr>
	</table>

</body>
</html>
```

## 게시글 추가를 위해 insert_form.jsp 만들기
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

		if (f.subject.value == '')
		{
			alert('제목을 입력하세요');
			f.subject.focus();
			return false;
		}
		
		if (f.name.value == '')
		{
			alert('작성자를 입력하세요');
			f.name.focus();
			return false;
		}
		
		if (f.content.value.trim() == '')
		{
			alert('한글자 이상 입력하세요');
			f.content.focus();
			return false;
		}
		
		if (f.pwd.value == '')
		{
			alert('비밀번호를 입력하세요');
			f.pwd.focus();
			return false;
		}
		
		f.submit();
	}
		
</script>
</head>
<body>
	<form name="f"
		method="post"
		action="insert.do"
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
				<th>내용</th>
				rows : 엔터 몇번친 공간
				cols : 가로로 몇글자정도 입력할 공간
				<!-- 가로로 50글자 세로로 엔터 10번정도 칠수 있는 크기 -->
				<td>
					<textarea name="content" rows="10"cols="50"style="resize:none;"></textarea>
				</td>
			</tr>
			<tr>
				<th>비밀번호</th>
				<td><input name="pwd" type="password"></td>
			</tr>
			<tr>
				<td colspan="2">
		이미지 태그는 폼태그의 공식적인 자식이 아니라서 this.form을 넣어도 의미가 없다.
				<img src="img/btn_reg.gif" onclick="send_check();">
				<img src="img/btn_back.gif" onclick="location.href='list.do'">
				</td>
			</tr>
		</table>
	</form>

</body>
</html>
```

