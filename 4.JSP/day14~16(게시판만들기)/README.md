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
		</tr>
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
					<textarea name="content" rows="10"cols="50"style="resize:none;"> </textarea>
					textarea한정 content쪽에 쓰여진 부분을 value로 가져온다.
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

## BoardInsertAction 서블릿 만들기
```
package action;

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import dao.BoardDAO;
import vo.BoardVO;

/**
 * Servlet implementation class BoardInsertAction
 */
@WebServlet("/insert.do")
public class BoardInsertAction extends HttpServlet {
	private static final long serialVersionUID = 1L;

	/**
	 * @see HttpServlet#service(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		//insert.do?subject=aaa&name=홍길동...
		
		request.setCharacterEncoding("utf-8");
		
		String name = request.getParameter("name");
		String subject = request.getParameter("subject");
		String content = request.getParameter("content");
		String pwd = request.getParameter("pwd");
		String ip = request.getRemoteAddr();//ip
		
		BoardVO vo = new BoardVO();
		vo.setName(name);
		vo.setSubject(subject);
		vo.setContent(content);
		vo.setPwd(pwd);
		vo.setIp(ip);
		
		int res = BoardDAO.getInstance().insert(vo);
		
		if(res > 0) {
			//등록완료후 게시판의 첫 페이지로 복귀
			response.sendRedirect("board_list.do");
		}
	}

}

```

## BoardDAO에 insert메서드 추가하기
```
		//게시글 추가
		public int insert(BoardVO vo) {
			SqlSession sqlSession = factory.openSession(true);
			int res = sqlSession.insert("b.board_insert",vo);
			
			//sqlSession.commit(); insert,update,delete를 하고나면
			커밋을 반드시 해야 한다. 하지만 openSession(true)를 해주게 되면 오토커밋으로 된다.
			
			sqlSession.close();
			
			return res;
		}
```

## board.xml에 쿼리문 추가하기
```xml
<!-- 새글쓰기(댓글 아님) -->
<!-- insert, update, delete에서는 resultType을 기술할 수 없다(자동으로 정수형태로 지정) -->
<insert id="board_insert" parameterType="board">
	insert into board values(seq_board_idx.nextval,파라미터로 넣을때는 앞에 vo는 생략을 해도 되고 똑같은 이름을 써야 getter를 호출한다.
							 #{name},
							 #{subject},
							 #{content},
							 #{pwd},
							 #{ip},
							 sysdate,
							 0,
							 seq_board_idx.currval,
							 0,
							 0
							 )
</insert>
```
- 새글이 잘 추가가 되는지 확인하기
- 강사 ip로 접근해서 새글을 작성할 수 있다.

# 게시글을 하나 눌렀을 때 내용을 조회해볼수 있도록 하자.

## BoardViewAction
```
package action;

/**
 * Servlet implementation class BoardViewAction
 */
@WebServlet("/view.do")
public class BoardViewAction extends HttpServlet {
	private static final long serialVersionUID = 1L;

	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		request.setCharacterEncoding("utf-8");
		
		//view.do?idx=5
		int idx = Integer.parseInt(request.getParameter("idx"));
		
		//상세보기,조회수 증가를 위해 DB에 두번접근하려고 DAO를 미리 생성해놓는다.
		BoardDAO dao = BoardDAO.getInstance();
		
		//idx에 해당하는 게시글 한 건 조회하기 여러개가 아니기 때문에 굳이 리스트에 담을 필요가 없다.
		BoardVO vo = dao.selectOne(idx);  ->DAO에 selectOne(idx)메서드 만들러 가자

		//조회수 증가
		int res = dao.update_readhit(idx);



		//상세보기 페이지로 전환하기 위해 바인딩 및 포워딩
		request.setAttribute("vo", vo);
		
		RequestDispatcher disp = request.getRequestDispatcher("board_view.jsp");
		disp.forward(request, response);
	}

}
```
## BoardDAO에 selectOne메서드 만들기
```
//idx에 해당하는 상세 내용 한건 조회
public BoardVO selectOne(int idx) {
	SqlSession sqlSession = factory.openSession();
	BoardVO vo = sqlSession.selectOne("b.board_one",idx);
	sqlSession.close();
	return vo;
}

//조회수 증가
public int update_readhit(int idx) {
	SqlSession sqlSession = factory.openSession(true);
	int res = sqlSession.update("b.board_readhit",idx);
	sqlSession.close();
	return res;
}

```
## board.xml에 쿼리문 추가하기
```xml
<!-- idx에 해당하는 게시글 한 건 조회 -->
<select id="board_one" resultType="board" parameterType="int">
	select * from board where idx = #{idx}
</select>
<!-- 조회수 증가 -->
<update id="board_readhit" parameterType="int">
 update board set readhit = readhit + 1 
 where idx=#{idx}
</update>
```

## 게시글 하나를 조회하는 board_view.jsp 생성하기
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

	<table border="1">
		<caption>:::게시글 상세보기:::</caption>
		<tr>
			<th>제목</th>
			<td>${vo.subject }</td>
		</tr>
		<tr>
			<th>작성자</th>
			<td>${vo.name }</td>
		</tr>
		<tr>
			<th>작성일</th>
			<td>${vo.regdate }</td>
		</tr>
		<tr>
			<th>ip</th>
			<td>${vo.ip }</td>
		</tr>
		<tr>
			<th>내용</th>
			<!-- DB에서는 \N을 통해 줄을 바꾸는데 HTML에서는 안먹히기 때문에 pre태그를 사용한다.-->
			<td width="500px" height="200px"><pre>${vo.content}</pre></pre></td>
		</tr>
		<tr>
			<th>비밀번호</th>
			<td><input type=“passowrd” id=“c_pwd”></td>
		</tr>
		<tr>
			<td colspan="2">
			<!-- 목록보기 -->
				<img src="img/btn_list.gif" onclick="location.href='board_list.do'">
				
			<!-- 답변 -->
				<img src="img/btn_reply.gif" onclick="reply();">
				
			<!-- 글삭제 -->
				<img src="img/btn_delete.gif" onclick="del();">
			</td>
			글을 삭제하면 댓글이 달린 것 까지 ref가 망가지기 때문에 삭제기능은 
			강사와 같이 진행하자
		</tr>	
	</table> 
</body>	
</html>
```

![image](https://user-images.githubusercontent.com/54658614/235836647-9b9f0e81-fb11-41ec-81c6-eab141ef64f9.png)


**새로고침 할 때마다 조회수가 증가하는 조회수작을 막아보자.<br>
아이디 저장하는거 이후에 세션을 사용하는 몇 안되는 코드가 조회수를 저장할 때!**

## BoardViewAction으로 이동하여 코드를 수정해 조회수 조작을 막자.
```java
	//조회수 증가
	HttpSession session = request.getSession();
	우리는 세션에 set을 해준적이 없기 때문에 초기값은 null이 들어온다.
	바인딩 할 때 어떻게 반환될지 몰라서 object로 저장이 되기 때문에 getAttribute의 반환형도 Object형태이다.
	그렇기 때문에 String으로 형변환을 해준다.
	
	//세션에 show라는 이름으로 저장된 저장된 값을 줘봐라 처음에는 null일것
	String show = (String)session.getAttribute("show");

	if(show == null) {
		int res = dao.update_readhit(idx);
		세션에 뭘 저장했는지는 중요하지 않다 무언가를 갖고 있기만 하면 된다.
		session.setAttribute("show", "0"); //0이아닌 다른것을 넣어도 상관 없다.
	}
	이렇게 하면 다른 게시물에 들어가도 0을 갖고 있기 때문에 조회수 증가가 안됨 그래서 메인 화면으로 오면 session을 해제 해줘야 한다.

	//상세보기 페이지로 전환하기 위해 바인딩 및 포워딩
	request.setAttribute("vo", vo);

	RequestDispatcher disp = request.getRequestDispatcher("board_view.jsp");
	disp.forward(request, response);
```

문제는 목록으로 돌아가서 다른 게시물로 들어가도 session은 점유가 되고 있는 상태이기 때문에 조회수가 오르지 않는다.<br>
메인화면으로 오면 session을 해제 해줘야 한다.<br>

## BoardListAction으로 들어왔을 때 세션을 풀어주기
```
	//전체 게시글 조회
	List<BoardVO> list = BoardDAO.getInstance().selectList();

	//조회수를 위해 저장해뒀던 show라는 정보를 세션에서 제거 코드 추가하기
	request.getSession().removeAttribute("show");

	//바인딩
	request.setAttribute("list", list);
```

# 댓글(답글)을 달아보는 작업을 해보자

![image](https://user-images.githubusercontent.com/54658614/235838024-4ccc39d5-ec34-4ebb-9b35-a50b477a84b4.png)

4번 댓글이 달리면서 step에 1을 먼저 넣어버리면 출력할 때 2번 댓글과 출력이 되는게 random으로 출력된다.<br>
그렇기 때문에 먼저 달린 댓글의 step을 2,3으로 바꾸고 4번 댓글에 step을 1로 설정을 해줘야 한다.<br>

## board_view에 reply()함수 만들기
```
몇번 게시물에 답글 달고 싶은데?
<script>
	function reply(){
		location.href="reply_form.jsp?idx=${vo.idx}";
	}
</script>
```

## reply_from.jsp 생성하기
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
		var f = document.f; //f라는 name을 가진 form태그를 가져옴
		
		f.submit();
	}
</script>
</head>			reply.do?subject=aaaa&name=홍길동&content=bbbbbbb&pwd=1111
<body>
	<form name="f"
	      method="post"
	      action="reply.do"> 
	boardView.jsp에서 idx를 보냈기 때문에 받아야 한다. 스크립트릿으로 받아야 할까?
	replyform.jsp?idx=000 으로 넘어온다. 저것도 reply.do로 같이 보내야 한다.
	el표기법 초반에 했던 파라미터 받는법
	<input type="hidden" name="idx" value="${param.idx}"> 
		<table border="1">	
			<caption>:::답 글 쓰기:::</caption>
			
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
				<td>
				    <textarea name="content" rows="10" cols="50" style="resize:none;"></textarea>
				</td>
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

## BoardReplyAction만들기
```java
/**
 * Servlet implementation class BoardReplyAction
 */
@WebServlet("/reply.do")
public class BoardReplyAction extends HttpServlet {
	private static final long serialVersionUID = 1L;

	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		
		int idx = Integer.parseInt(request.getParameter("idx"));
		String name = request.getParameter("name");
		String subject = request.getParameter("subject");
		String content = request.getParameter("content");
		String pwd = request.getParameter("pwd");
		String ip = request.getRemoteAddr();

		BoardDAO dao = BoardDAO.getInstance();

		같은 레퍼런스를 가지고 있는 데이터들 중에서 지금 내가 추가하려고 하는
		step값 이상인 애들을 +1을 해놔야 하기 때문에 insert를 먼저하지 않는다.
		
		//기준글의 idx를 이용해서 댓글을 달고싶은 게시글의 정보를 가져온다.
		BoardVO base_vo = dao.selectOne(idx);
		
		//기준글에 step이상 값은 step = step + 1 처리
		int res = dao.update_step(base_vo); -> dao에 만들러 가기
		
	}

}
```

## BoardDAO에 step+1을 위한 메서드 만들기
```java
//댓글추가를 위한 step+1
public int update_step(BoardVO vo) {
	SqlSession sqlSession = factory.openSession(true);
	int res = sqlSession.update("b.board_update_step", vo);
	sqlSession.close();
	return res;
}
```

## Mapper에 추가하기
```xml
<!-- 댓글작성을 위한 STEP 증가 -->
<update id="board_update_step" parameterType="board">
	update board set step = step + 1
	where ref=#{ref} and step > #{step}
</update>
```

## BoardReplyAction에 코드추가하기
```java
//기준글에 step이상 값은 step = step + 1 처리
	int res = dao.update_step(base_vo);

	//댓글 vo
	BoardVO vo = new BoardVO();
	vo.setName(name);
	vo.setSubject(subject);
	vo.setContent(content);
	vo.setPwd(pwd);
	vo.setIp(ip);

	//댓글이 들어갈 위치를 선정
	vo.setRef(base_vo.getRef());
	vo.setStep(base_vo.getStep()+1);
	vo.setDepth(base_vo.getDepth()+1);

	//댓글등록
	res = dao.reply(vo); -->dao에 메서드 만들러 가기

	if(res > 0) {
		response.sendRedirect("board_list.do");
	}

```

## BoardDAO에 답글 추가 메서드 작성하기
```java
	//댓글추가
	public int reply(BoardVO vo) {
		SqlSession sqlSession = factory.openSession(true);
		int res = sqlSession.insert("b.board_reply",vo);
		sqlSession.close();
		return res;
	}
```
## Mapper 작성하기
```xml
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
 		#{depth}
 	)
 </insert>
```

##  board_view.jsp 수정하여 다답글 못달게 하기
- 원본글에서는 답변버튼이 살아있어야 하지만 답글을 눌렀을 때는<br>답변버튼이 아예없으면 된다.
- depth가 0인지 아닌지를 통해 알 수 있다.
```
<c:if test="${ vo.depth lt 1 }">
	<!-- 답변 -->
	<img src="img/btn_reply.gif" onclick="reply();">
</c:if>
```

# 삭제하기
- 실제로 지워지는것이 아닌 지워진것 처럼 보이게 할 것이다.
- DB에서 완전히 삭제를 하는게 아닌 컬럼을 하나 추가해서<br>안지워진 글이면 0 지워진것처럼 보이게 하려면 -1

## del_info컬럼 추가하기
```SQL
alter table board add(del_info NUMBER(2));
```

![image](https://user-images.githubusercontent.com/54658614/236111447-8299f19b-31ad-4d27-ab9a-8d4ee7a26991.png)

DEL_INFO 컬럼이 추가된걸 볼 수 있다.

![image](https://user-images.githubusercontent.com/54658614/236111513-275a5e7c-9a68-4347-b981-0dccb9f10630.png)

이미 만들어진 게시글에 대해서는 어쩔 수 없지만 앞으로 만들어진 글에 대해서는 DEL_INFO에 대한 정보도 넣어야 한다.

## VO에 DEL_INFO 추가하기
```
private int del_info; //삭제여부

public int getDel_info() {
		return del_info;
	}
public void setDel_info(int del_info) {
	this.del_info = del_info;
}
```

## board_view.jsp에 del()메서드 구현하기
```html
<head>
<meta charset="UTF-8">
<title>Insert title here</title>

<!-- Ajax사용을 위한 js참조  -->
<script src="js/httpRequest.js"></script>

<script>
	function reply(){
		location.href="reply_form.jsp?idx=${vo.idx}";
	}
	
	function del(){
		if( !confirm("삭제하시겠습니까?") ) {
			return;
		}
		//var pwd = "${vo.pwd}"; 따옴표로 감싸지 않으면 문자열 비밀번호는 삭제할 수 없다.
		var pwd = ${vo.pwd}; //원본 비밀번호
		var c_pwd = document.getElementById("c_pwd").value;
		
		if(pwd != c_pwd){
			alert("비밀번호 불일치");
			return;
		} 
		
		var url = "del.do";
		var param = "idx=${vo.idx}";
		
		sendRequest(url,param,delCheck,"post");
	}
	
	//삭제여부를 판단하는 콜백메서드
	function delCheck(){
		if(xhr.readyState == 4 && xhr.status == 200){
			
		}
	}
</script>
</head>
```

## BoardDeleteAction 생성하기
```java
package action;

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import dao.BoardDAO;
import vo.BoardVO;

/**
 * Servlet implementation class BoardDeleteAction
 */
@WebServlet("/del.do")
public class BoardDeleteAction extends HttpServlet {
	private static final long serialVersionUID = 1L;

	/**
	 * @see HttpServlet#service(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// del.do?idx=00
		int idx = Integer.parseInt(request.getParameter("idx"));
		
		BoardDAO dao = BoardDAO.getInstance();
		
		//삭제하고자하는 원본 게시물
		//삭제하고자하는 원본 게시물의 정보를 얻어온다.
		BoardVO baseVO = dao.selectOne(idx);
		
		baseVO.setSubject("삭제된 글입니다");
		baseVO.setName("unknown");
		
		int res = dao.del_update(baseVO); -> dao에 del_update메서드 작성하기
		
		if(res > 0) {
			response.getWriter().print("[{'param':'yes'}]");
		} else {
			response.getWriter().print("[{'param':'no'}]");
		}
	}

}

```

## boardDAO에 메서드 작성하기
```java
//게시글삭제(된것처럼 업데이트)
public int del_update(BoardVO vo) {
	SqlSession sqlSession = factory.openSession(true);
	int res = sqlSession.update("b.del_update",vo);
	sqlSession.close();
	return res;
}
```

## del_info컬럼이 추가되었으니 쿼리문을 다 수정해주자.
```xml
<!-- 새글쓰기(댓글 아님) -->
<!-- insert, update, delete에서는 resultType을 기술할 수 없다(자동으로 정수형태로 지정) -->
<insert id="board_insert" parameterType="board">
	insert into board values(
		seq_board_idx.nextval,
		#{name},
		 #{subject},
		 #{content},
		 #{pwd},
		 #{ip},
		 sysdate,
		 0,
		 seq_board_idx.currval,
		 0,
		 0,
		 0
		 )
</insert>
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

 <!-- 게시글 삭제(된 것 처럼 업데이트) -->
  <update id="del_update" parameterType="board">
  	update board set
  	subject=#{subject},
  	name=#{name},
  	del_info= -1
  	where idx=#{idx}
  </update>
```

## board_view.jsp로 와서 콜백메서드 마저 작성하기
```
function delCheck(){
	if(xhr.readyState == 4 && xhr.status == 200){
		var data = xhr.responseText;
		//"[{'param':'yes'}]"
		var json = eval(data);

		if(json[0].param == 'yes'){
			alert("삭제 성공");
			location.href="board_list.do";
		} else {
			alert("삭제 실패");
		}
	}
}
```
## 삭제된 글은 더이상 클릭할 수 없도록 처리하기
```html
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
					<!-- 댓글기호 --> 
					<c:if test="${vo.depth ne 0 }">ㄴ</c:if>
					
					<!-- 삭제되지 않은 글일 경우 클릭이 가능 -->
					<c:if test="${vo.del_info ne -1 }">
						<a href="view.do?idx=${vo.idx}">
						<font color="black">${vo.subject }</font>
						</a>
					</c:if>
					
					<!-- 삭제된 게시글을 클릭할 수 없도록 처리 -->
					<c:if test="${vo.del_info eq -1 }">
						<font color="gray">${vo.subject }</font>
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

					◀ 1 2 3▶
				</td>
			</tr>
			
			<tr>
				<td colspan="5" align="right">
					<img src="img/btn_reg.gif"
					 onclick="location.href='insert_form.jsp'"/>
					
				</td>
			</tr>
	</table>

</body>
</html>
```
# 페이징 처리하기
- 한 페이지에 10개 정도의 글만 보이게 설정할 것이다.
- 밑에는 숫자 세 개가 보이고 4페이지를 보고 싶으면 화살표를 눌러서 보이게 하자.

## 페이징 처리를 위한 쿼리문

```sql
-- 페이징 처리를 위한 쿼리문
select * from (select rank() over(order by ref desc,step) no, b.* from board b)
where no between 1 and 10;

오라클 홈페이지나 디비버를 켜고 숫자를 바꿔서 10개씩 조회되는걸 보자.
```
## util 패키지에 Common 클래스 생성
- 여러 가지 기능들을 편리하게 관리하기 위해서 설정용 클래스를 따로 만들어서 관리를 해주자


```java
package util;

	 public class Common{

	//일반게시판용
	public static class Board{ --> 내부 클래스
	
	밖에 있는 클래스는 내부클래스를 멤버변수처럼 사용할 수 있다. 사용하려면 new로 인스턴스를 만들어야한다.
	내부클래스는 자신의 밖에 있는 클래스의 자원을 직접 사용할 수 있다.
	
	내부클래스는 밖에 있는 클래스의 자원을 마음대로 사용할 수 있지만, 중첩클래스는 static 키워드가 안붙었다면 사용할 수 없다.
	
	Outer 클래스의 객체가 없어도 Inner 클래스의 객체 생성이 가능하다. (하단에 객체생성법 참고)
	
		//한페이지에 보여줄 게시물 개수
		public final static int BLOCKLIST = 10;

	//BLOCKLIST를 사용하려면 -> Common.Board.BLOCKLIST를 호출하면 사용할 수 있다.

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
## BoardListAction 수정하기
- 한 페이지에 10개만 보여주게 코드 수정하기

```java
int nowPage = 1;

나중에 board_list.do가 다시 호출되면 page값을 가져오게 할 것.
하지만 처음에는 값이 없기 때문에 null
String page = request.getParameter("page");

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

한 페이지에 10개씩만 select가 되게 할 것이다.
이제 아래의 쿼리문에 1과 10부분에 start와 end를 넣어주면 되는데 직접 mapper로 넣을 수 있는 방법이 없다.
HashMap을 이용하자.

HashMap을 import 할 때 패키지 경로를 잘 보자 쓸 데가 있다!

HashMap<String, Integer> map = new HashMap<String, Integer>();
map.put("start",start);
map.put("end",end);

//전체 게시글 조회 -> 페이지 번호에 따른 게시글 조회
이제 앞으로는 모~든 목록을 조회하는 쿼리문을 필요가 없다 수정을 해주자.
List<BoardVO> list = BoardDAO.getInstance().selectList(map);
```

## BoardDAO이동하고 메서드 수정하기
```java
//페이지별로 게시글 조회라고 수정해주기
public List<BoardVO> selectList(HashMap<String, Integer> map){
	SqlSession sqlSession = factory.openSession();
	List<BoardVO> list = sqlSession.selectList("b.board_list", map);
	sqlSession.close();
	return list;
}
```

## Board.xml 수정하기
```xml
<!-- 페이지별 게시글 조회 -->
<select id="board_list" resultType="board" parameterType="java.util.HashMap">
	select * 
	from (select rank() over(order by ref desc,step) no, b.*from board b)
	where no between #{start} and #{end}
</select>

```
## BoardListAction에서 실행하기
- board_list.do?page= 번호 -> 수동으로 이동해주기
- 총 게시물의 수를 알아야 밑에 보여지는 페이지 숫자의 개수를 조절할 수 있다.

## BoardDAO에 전체 게시물의 개수를 조회하는 메서드 만들기
```
//전체 게시물 수 조회
public int getRowTotal() {
	SqlSession sqlSession = factory.openSession();
	int count = sqlSession.selectOne("b.board_count"); 
	//쿼리문의 결과가 숫자 하나만 넘어옴으로 selectOne으로 해줘야 한다.
	sqlSession.close();					
	return count;
}
```

## board.xml 에 전체 게시물 count해주는 쿼리문 추가하기
```
<!-- 전체 게시물 개수 조회 -->
 <select id="board_count" resultType="int">
 	select count(*) from board
 </select>

```

## util 패키지에 page.java 페이징 처리를 위한 클래스 추가하기
```java
package util;
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
		
		String vs StringBuffer
		String :immutable(불변)
		기존 문자열에 이어붙히면 버리고 메모리 새로 할당받음
		문자열이 많은경우 성능이 좋아지지 않는다.
		
		StringBuffer : mutable(변함)
		기존의 버퍼 크기를 늘리며 유연하게 동작합니다.
		
		
		
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
			sb.append("'>◀</a>");
		}
		else
			sb.append("◀");
		
//------페이지 목록 출력 -------------------------------------------------------------------------------------------------
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
				sb.append("'>");
				sb.append(i);
				sb.append("</a>");
			}
		}// end for
		
		sb.append("&nbsp;|");
		
//-----그룹페이지처리 다음 ----------------------------------------------------------------------------------------------
		if(isNextPage){
			sb.append("<a href='"+pageURL+"?page=");
			
			sb.append(endPage + 1);
			/*if(nowPage+blockPage > totalPage)nowPage = totalPage;
			else
				nowPage = nowPage+blockPage;
			sb.append(nowPage);*/
			sb.append("'>▶</a>");
		}
		else
			sb.append("▶");
//---------------------------------------------------------------------------------------------------------------------	    

		return sb.toString();
	}
}

```

## BoardListAction에 내용 추가하기
```
//페이지 번호에 따른 전체 게시글 조회
List<BoardVO> list = BoardDAO.getInstance().selectList(map);

//전체 게시물 수 조회
int rowTotal = BoardDAO.getInstance().getRowTotal();

//페이지 메뉴 생성하기
String pageMenu = Paging.getPaging(
		"board_list.do",
		nowPage, //현재 페이지 번호
		rowTotal, //전체 게시물 수
		Common.Board.BLOCKLIST, //한 페이지에 표기할 게시물 수
		Common.Board.BLOCKPAGE); //페이지 메뉴 수
		
request.getSession().removeAttribute("show");

//바인딩
request.setAttribute("list", list);

request.setAttribute("pageMenu", pageMenu);

```

## board_list.jsp에 임시로 놨던 페이지를 수정하기
```
<tr>
	<td colspan="5" align="center">
		◀1 2 3 ▶ -> ${pageMenu} 로 수정하기
	</td>
</tr>
```

## 페이지에 실제 이미지로 넣기
```
//-----그룹페이지처리 이전 		
		if(isPrevPage){
			sb.append("<a href ='"+pageURL+"?page=");
			//sb.append(nowPage - blockPage);
			sb.append( startPage-1 );
			sb.append("'><img src='img/btn_prev.gif'></a>");
		}
		else
			sb.append("<img src='img/btn_prev.gif'>");
		
//------페이지목록출력-------------------------------------------------------------------
		sb.append(" ");
		for(int i=startPage; i<= endPage ;i++){
			if(i>totalPage)break;
			if(i == nowPage){ //현재 있는 페이지 색깔 바꿔주기↓
				sb.append("&nbsp;<b><font color='#ff0000'>");
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
		
//------그룹 페이지 처리 다음-------------------------------------------------------
		if(isNextPage){
			sb.append("<a href='"+pageURL+"?page=");
			
			sb.append(endPage + 1);
			/*if(nowPage+blockPage > totalPage)nowPage = totalPage;
			else
				nowPage = nowPage+blockPage;
			sb.append(nowPage);*/
			sb.append("'><img src='img/btn_next.gif'></a>");
		}
		else
			sb.append("<img src='img/btn_next.gif'>");
//---------------------------------------------------------------------------------------
```

만약 100페이지쯤 글을 보고 있었는데 글을 하나 보고 목록보기를 눌렀을 때 처음 페이지로 돌아가면<br> 이어서 보기가 매우 불편할것이다.

# 모든곳에 페이지 같이 넘기기

## board_list.jsp 에 페이지 같이 넘기기
```
<!-- 삭제되지 않은 글이라면 클릭 가능 -->
	<c:if test="${vo.del_info ne -1 }">
		<a href="view.do?idx=${vo.idx}&page=${param.page}">
		<font color="black">${vo.subject }</font>
		</a>
	</c:if>
```
## board_view.jsp에서도 페이지 번호 같이 넘겨주기
```
<!-- 목록보기 -->
<img src="img/btn_list.gif" onclick="location.href='board_list.do?page=${param.page}'">

삭제하고 나서도 1페이지로 돌아가지 않도록 delCheck() 메서드에서 location.href 수정해주기

//삭제여부를 판단하는 콜백메서드
function delCheck(){
	if(xhr.readyState == 4 && xhr.status == 200){
		var data = xhr.responseText;
		//"[{'param':'yes'}]"
		var json = eval(data);

		if(json[0].param == 'yes'){
			alert("삭제 성공");
			location.href="board_list.do?page=${param.page}";
		} else {
			alert("삭제 실패");
		}
	}
}
```

## board_view.jsp의 reply()메서드 수정하기
- 100페이지의 어떤 글에 답글을 달았는데 1페이지로 가버리면 곤란하다.
```
function reply(){
	location.href="reply_form.jsp?idx=${vo.idx}&page=${param.page}";
	}
```

## reply_form.jsp 수정하기
-  등록하기 누르면 1페이지로 돌아가면 안되니 page도 파라미터로 넘기자
```
<form name="f"
	  method="post"
	  action="reply.do">

	  <input type="hidden" name="idx" value="${param.idx }">
	  <input tpye="hidden" name="page" value="${param.page }">
	  <table border="1">
```
## reply.do 수정하기
```
if(res > 0) {
	int page = Integer.parseInt(request.getParameter("page"));
	response.sendRedirect("board_list.do?page="+page);
}
```
