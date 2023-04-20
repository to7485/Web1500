# 아이디 중복 체크
- 테이블에서 id를 unique로 설정해놨기 때문에 중복되는 데이터를 넣으면 500에러가 발생한다.
- 하지만 고객의 입장에서 500에러가 뜨면 웹사이트가 고장이 났다고 생각할 것이다.

## 테이블 생성
```
--일련번호 관리객체
create sequence seq_myUser_idx;

--회원테이블
create table myUser(
  idx number(3) primary key, --일련번호
  name varchar2(100) not null, --이름
  id varchar2(100) not null, unique, --아이디(중복방지 unique)
  pwd varchar2(100) not null --비밀번호
);

--커밋
commit;
```

Ajax를 사용하기 위해 js폴더를 가져오고<br>
DB와 연결을 하기 위해 context.xml과 service패키지, 라이브러리들을 가져오자

## vo패키지에 UserVO 클래스 생성하기
```java
package vo;

public class UserVO {
	//회원관리객체
	int idx;//일련번호
	String name;//이름
	String id;//아이디
	String pwd;//패스워드
	
	
	public int getIdx() {
		return idx;
	}
	public void setIdx(int idx) {
		this.idx = idx;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public String getId() {
		return id;
	}
	public void setId(String id) {
		this.id = id;
	}
	public String getPwd() {
		return pwd;
	}
	public void setPwd(String pwd) {
		this.pwd = pwd;
	}
	
}
```

## dao패키지에 UserDAO클래스 생성하기

![image](https://user-images.githubusercontent.com/54658614/233263328-0e563340-1e5f-4398-8693-99c7d70cf5d3.png)


```java
public class UserDAO {
	// single-ton pattern: 
	// 객체1개만생성해서 지속적으로 서비스하자

	//_singleton 템플릿
	static MemberDAO single = null;

	public static MemberDAO getInstance() {
		//생성되지 않았으면 생성
		if (single == null)
			single = new MemberDAO();
		//생성된 객체정보를 반환
		return single;
	}
우리의 목적은 서블릿에서 전체 목록을 조회해서 바인딩을 해서 jsp로 포워딩해서 보여주는것!
	
		//회원정보 조회
		//_select_list 템플릿
		public List< UserVO > selectList() {

			List< UserVO > list = new ArrayList<MemberVO>();
			Connection conn = null;
			PreparedStatement pstmt = null;
			ResultSet rs = null;
			
			//idx내림차순으로 검색
			String sql = "select * from myshop order by idx order by desc";

			try {
				//1.Connection얻어온다
				conn = DBService.getInstance().getConnection();
				//2.명령처리객체정보를 얻어오기
				pstmt = conn.prepareStatement(sql);

				//3.결과행 처리객체 얻어오기
				rs = pstmt.executeQuery();

				while (rs.next()) {

					UserVO vo = new UserVO();
					
					//현재레코드값=>Vo저장
					vo.setIdx(rs.getInt("idx"));
					vo.setName(rs.getString("name"));
					vo.setId(rs.getString("id"));
					vo.setPwd(rs.getString("pwd"));
				
					//ArrayList추가
					list.add(vo);

				}

			} catch (Exception e) {
				// TODO: handle exception
				e.printStackTrace();
			} finally {
					..................
			}

			return list;
		}//selectList()
		
		//_insert템플릿
		public int insert(UserVO vo) {
			
			int res = 0;

			Connection conn = null;
			PreparedStatement pstmt = null;

			String sql = 
			   "insert into myshop values(seq_myUser_idx.nextVal,?, ?, ?)";

			try {
				//1.Connection획득
				conn = DBService.getInstance().getConnection();
				//2.명령처리객체 획득
				pstmt = conn.prepareStatement(sql);

				//3.pstmt parameter 채우기
				pstmt.setString(1, vo.getName());
				pstmt.setString(2, vo.getId());
				pstmt.setString(3, vo.getPwd());

				//4.DB로 전송(res:처리된행수)
				res = pstmt.executeUpdate();

			} catch (Exception e) {
				// TODO: handle exception
				e.printStackTrace();
			} finally {
				..............
			}
			return res;
		}//insert()
}

```

## action패키지에 MemberListAction서블릿 만들기

```java
@WebServlet("/member_list.do") //url맵핑 변경!!!!!!!

public class MemberListAction extends HttpServlet {
	private static final long serialVersionUID = 1L;

	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

		//회원목록 얻어오기
		List<UserVO> list = UserDAO.getInstance().selectList();

		//리퀘스트 영역에 list바인딩
		request.setAttribute("list", list);

		//member_list.jsp에서 el기법을 사용할수 있도록 하기 위해
		//위에서 바인딩해준 list정보를 넘겨준다.
		RequestDispatcher disp = 
				request.getRequestDispatcher("member_list.jsp");

		disp.forward(request, response);
	}
}
```

## member_list.jsp 생성하기
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
    
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script src="js/httpRequest.js"></script>
<style>
	table{border-collapse:collapse;} --> 테이블 테두리 한겹으로 바꿈
	th{ width:200px;
		height:60px;}
</style>
</head>
<body>
	<table border="1">
		<tr>
			<td colspan="5" align="center">
				<input type="button" value="회원가입" onclick="location.href='member_insert_form.jsp'">
			</td>
		</tr>
		
		<tr>
			<th>회원번호</th>
			<th>이름</th>
			<th>아이디</th>
			<th>비밀번호</th>
			<th>비고</th>
		</tr>
		
		<c:forEach var="vo" items="${list}">
			<td>${vo.idx}</td>
			<td>${vo.name}</td>
			<td>${vo.id}</td>
			<td>${vo.pwd}</td>
			
			<td>
				<input type="button" value="삭제" onclick="del('${vo.idx}');">
			</td>
		</c:forEach>
	</table>

</body>
</html>
```

![image](https://user-images.githubusercontent.com/54658614/233264349-a5fc4853-0d8a-4ec2-b1bf-9276ac737dbe.png)

## 회원가입을 위한 member_insert_form.jsp 생성하기

![image](https://user-images.githubusercontent.com/54658614/233264878-074b3f58-c00f-43ae-b93a-109ea3e85131.png)


```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>

	<script src="js/httpRequest.js"></script>
	
	<script>
		var b_idCheck = false;
		
		function send(f){
			
			var id = f.id.value.trim();
			var pwd = f.pwd.value.trim();
			var name = f.name.value.trim();
			
			//유효성체크
			if(id=='') {
				alert("아이디를 입려하세요"); 
			//db에서 notnull로 만들었어도 비워버리면 그냥 500에러가 나버림
				return;
			}
			
			//했다쳐
			
			f.method = "POST";
			f.action = "insert.do";
			f.submit();
		}
	
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
				<td colspan="2" align="center">
					<input type="button" value="가입" onclick="send(this.form);">
					<input type="button" value="취소" onclick="location.href='member_list.do'">
				</td>
			</tr>
		</table>
	</form>
</body>
</html>

```
## action패키지에 MemberInsertAction서블릿 생성
```
package action;

import java.io.IOException;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import dao.UserDAO;
import vo.UserVO;

/**
 * Servlet implementation class MemberInsertAction
 */
@WebServlet("/insert.do")
public class MemberInsertAction extends HttpServlet {
	private static final long serialVersionUID = 1L;

	/**
	 * @see HttpServlet#service(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void service(HttpServletRequest request, HttpServletResponse response) 
						throws ServletException, IOException {
		// insert.do?id=aaa&pwd=1111&name=홍길동
		request.setCharacterEncoding("utf-8");
		
		String id = request.getParameter("id");
		String pwd = request.getParameter("pwd");
		String name = request.getParameter("name");
		
		UserVO vo = new UserVO();
		vo.setId(id);
		vo.setPwd(pwd);
		vo.setName(name);
		
		int res = UserDAO.getInstance().insert(vo);
		
		if(res >=1 ) {
			response.sendRedirect("member_list.do");
		}
	}
}
```
이제 회원가입을 클릭했을 때 아이디가 중복되면 가입할 수 없도록 처리해보자

## member_list_form.jsp에 코드 추가하기

```
<script type="text/javascript">
		
		//아이디 중복여부 체크
		var b_idCheck = false;
	
		function send(f){
			//입력내용 체크
			var id = f.id.value.trim();
			var pwd = f.pwd.value.trim();
				.............
			중복체크 버튼을 눌렀을 때 내가 입력한 id를 DB까지 전달해서 데이터가 없다면 b_idCheck를 true로 바꾸자.			
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
			
		}//resultFn()
		
		
	</script>
```

## action패키지에 MemberCheckAction서블릿 생성

![image](https://user-images.githubusercontent.com/54658614/233267529-c63377a8-9a6f-4f95-8f13-debf6c11e30f.png)


```
/**
 * Servlet implementation class MemberIdCheckAction
 */
@WebServlet("/check_id.do")
public class MemberIdCheckAction extends HttpServlet {
	private static final long serialVersionUID = 1L;

	/**
	 * @see HttpServlet#service(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// /check_id.do?id=aaaa
		String id = request.getParameter("id");
		UserVO vo = UserDAO.getInstance().selectOne(id); //DB에 접근
		
		String res = "no";
		if(vo == null) {//중복조회를 해서 회원가입이 가능한 경우
			res = "yes";
		}
		
		String result = String.format("[{'res':'%s'}]",res);
		//result를 가지고 콜백메서드 복귀
		response.getWriter().print(result);
		
	}

}
```

## UserDAO에 selectOne메서드 추가하기
- 입력받은 아이디를 DB까지 가지고 가서 있는지 검증을 해서 결과를 돌려주자

```
//아이디 중복체크
	public UserVO selectOne(String id) {

		UserVO vo = null;

		Connection conn = null;
		PreparedStatement pstmt = null;
		ResultSet rs = null;
		String sql = "select * from myuser where id=?";

		try {
			//1.Connection얻어온다
			conn = DBService.getInstance().getConnection();
			//2.명령처리객체정보를 얻어오기
			pstmt = conn.prepareStatement(sql);

			//3.pstmt parameter 설정
			pstmt.setString(1,id);
			//4.결과행 처리객체 얻어오기
			rs = pstmt.executeQuery();

			//해당 if문은 조회가 한건이라도 되었을 때만 수행
			if (rs.next()) {
				vo = new UserVO();
				//현재레코드값=>Vo저장
				//이미 가입된 사람의 정보를 얻을 수 있음
				//vo.setIdx(rs.getInt("idx"));
			}

		} catch (Exception e) {
			// TODO: handle exception
			e.printStackTrace();
		} finally {

			try {
				if (rs != null)
					rs.close();
				if (pstmt != null)
					pstmt.close();
				if (conn != null)
					conn.close();
			} catch (SQLException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
		}
		//vo -> null : 회원가입이 가능한 상태
		//vo -> null이 아님 : 회원가입이 불가능한 상태
		return vo;
	}
```
check_id.do에서 아이디 중복여부를 json타입으로 전달하였기 때문에<br>
member_insert_form.jsp에서 resultFn() 콜백메서드를 통해 값을 처리해줘야 한다.<br>

## member_insert_form.jsp의 콜백메서드 작성해주기
```
<script type="text/javascript">
		
	//아이디 중복여부 체크
	var b_idCheck = false;

	function send(f){
		.........
	}//send()			


	//아이디 중복체크를 위한 메서드
	function check_id(){
			...............
		sendRequest( url, param, resultFn, "POST" );

	}//check_id()

	function resultFn(){
		if(xhr.readyState == 4 && xhr.status == 200){
			//"[{'res':'no'}]"
			var data = xhr.responseText;
			//문자열 구조인 data를 실제 JSON형태로 변환
			var json = eval(data);

			if(json[0].res == 'no'){
				alert("이미 사용중인 아이디 입니다.");
				return;
			} else {
				//회원가입이 가능한 경우
				alert("사용 가능한 아이디 입니다");
				b_idCheck = true;
			}
		}
	}

</script>
```
여기에 커다란 오류가 한가지 있다.<br>
처음에 중복되지 않은 아이디로 중복체크를 하게 되면 b_idCheck 의 값이 true로 바뀌게 되는데<br>
문제는 그 다음 다시 중복되는 아이디로 적은다음 가입을 누르면 가입이 된다는 점이다.<br>

b_idCheck가 true로 바뀌게 되면서 이미 가입했던 아이디로 가입이 되버린다.<br>
하지만 테이블에서 만들 때 유니크로 만들었기 때문에 오류가 난다. 오류가 아닌 경고를 띄워주자.<br>

## member_insert_form.jsp에 코드 추가하기
```
<script>
//아이디를 입력받는 입력상자의 값이 변환되면 호출되는 메서드
function che(){
	b_idCheck = false;
}

</script>


 <tr>
	<th>아이디</th>
	<td>
	<input name="id" id="id" onchange="che()">
	<input type="button" value="중복체크" onclick="check_id()";>
	</td>
</tr>
```









