## 회원정보 관리 프로그램

### 테이블 만들기

```
--일련번호 관리객체
create sequence seq_member_idx;

--회원테이블
create table member(
	idx int primary key,               --일련번호
	name varchar2(100) not null,       --이름
	id varchar2(100) not null unique,  --아이디(중복방지 unique)
	pwd varchar2(100) not null,        --패스워드
	email varchar2(100) unique,        --이메일
);

--샘플 데이터 추가
insert into member values( seq_member_idx.nextVal,
				 '홍길동', 
				 'one',
				 '1234',
				 'one@korea.com'
				 );

--커밋
commit;
```

### 프로젝트 생성하기
- Service 패키지, context.xml, 라이브러리를 추가해주자.

![image](https://user-images.githubusercontent.com/54658614/232308984-37f9e12c-fc66-4231-9cdd-138df74f3719.png)

### MemberVO.java 생성하기

![image](https://user-images.githubusercontent.com/54658614/232309186-c40fea21-a9be-46d0-8bfd-e38f23244221.png)

```java
package vo;

public class MemberVO {

	//회원관리객체
	int idx;//일련번호
	String name;//이름
	String id;//아이디
	String pwd;//패스워드
	String email;//이메일
	
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
	public String getEmail() {
		return email;
	}
	public void setEmail(String email) {
		this.email = email;
	}
}
```
### MemberDAO.jav 클래스 생성하기

![image](https://user-images.githubusercontent.com/54658614/232309392-bfebaa90-c82a-4717-9410-d52658ee0157.png)


```java
package dao;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.ArrayList;
import java.util.List;

import service.DBService;
import vo.MemberVO;

public class MemberDAO {
	// single-ton pattern: 
	// 객체1개만생성해서 지속적으로 서비스하자
	static MemberDAO single = null;

	//_singleton 템플릿
	public static MemberDAO getInstance() {
		//생성되지 않았으면 생성
		if (single == null)
			single = new MemberDAO();
		//생성된 객체정보를 반환
		return single;
	}
	
	//관리자단을 위한 회원정보 조회
	//_select_list 템플릿
	public List< MemberVO > selectList() {

		List<MemberVO> list = new ArrayList<MemberVO>();
		Connection conn = null;
		PreparedStatement pstmt = null;
		ResultSet rs = null;
		
		//idx내림차순으로 검색
		String sql = "select * from member order by idx desc";

		try {
			//1.Connection얻어온다
			conn = DBService.getInstance().getConnection();
			//2.명령처리객체정보를 얻어오기
			pstmt = conn.prepareStatement(sql);

			//3.결과행 처리객체 얻어오기
			rs = pstmt.executeQuery();

			while (rs.next()) {
				MemberVO vo = new MemberVO();
				
				//현재레코드값=>Vo저장
				vo.setIdx(rs.getInt("idx"));
				vo.setName(rs.getString("name"));
				vo.setId(rs.getString("id"));
				vo.setPwd(rs.getString("pwd"));
				vo.setEmail(rs.getString("email"));
				
				//ArrayList추가
				list.add(vo);
			}

		} catch (Exception e) {
			..............
		} finally {
			..............
		}

		return list;
	}//selectList()
		
}

```
### 데이터베이스에서 조회하기 위한 member_list.jsp 생성하기

![image](https://user-images.githubusercontent.com/54658614/232309377-0acec3ad-e3ea-4909-93e6-afdbf8762048.png)

다음과 같은 모양 만들어보기

![image](https://user-images.githubusercontent.com/54658614/232309577-db71b5c8-180b-43ae-ba22-3ce2bd26b2d3.png)


```jsp
<% 
	//회원목록 가져오기
	List<MemberVO> member_list =MemberDAO.getInstance().dao.selectList();
%>

<html>
<head>

<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>

	<style>
		table{border:1px solid black;
		      border-collapse:collapse;}
	</style>

</head>

<body>
	<table border="1">		
		<caption>::회원목록::</caption>
		
		<tr>
			<th>회원번호</th>
			<th>이름</th>
			<th>아이디</th>
			<th>비밀번호</th>
			<th>이메일</th>
			<th>비고</th>
		</tr>
		
		<% 
		for(int i = 0; i < member_list.size(); i++){ 
			MemberVO vo = member_list.get(i);
		%>
		
		<tr>
			<td><%= vo.getIdx()%></td>
			<td><%= vo.getName()%></td>
			<td><%= vo.getId()%></td>
			<td><%= vo.getPwd()%></td>
			<td><%= vo.getEmail()%></td>
			
			
			<td>
				<input type=button value="삭제" onclick="del('<%= vo.getIdx() %>')"/>
			</td>
		</tr>
		
		<% }%>
	</table>
	
		<input type="button" value="추가" onclick="location.href='member_register_form.jsp';">	
	
</body>

</html>
```

### member테이블에 추가하기 위한 데이터를 입력받는 member_register_form.jsp 생성

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>

	<script>
		function send(f) {
			var name = f.name.value.trim();
			var id = f.id.value.trim();
			var pwd = f.pwd.value.trim();
			var email = f.email.value.trim();
			
			
			//유효성 체크
//이메일 정규식/[0-9a-zA-Z]([-_.]?[0-9a-zA-Z])*@[0-9a-zA-Z]([-_.]?[0-9a-zA-Z])*.[a-zA-Z]$/i
			if(name=='') {
				alert("이름을 입력하세요");
				return;
			}
			
			var email = f.email.value.trim();
			var e_pattern = /[0-9a-zA-Z]([-_.]?[0-9a-zA-Z])*@[0-9a-zA-Z]([-_.]?[0-9a-zA-Z])*.[a-zA-Z]$/i;
			if(!e_pattern.test(email)) {
				alert("이메일 형식이 올바르지 않습니다.");
				return;
			}
			
			f.method="post";
			f.action="member_reg.jsp"
			f.submit();
		}
	</script>
</head>
<body>
	<!-- 회원정보 입력 폼 -->
	<form>
		<table border = 1>
			<tr>
				<th>이름</th>
				<td><input name="name"></td>
			</tr>
			<tr>
				<th>아이디</th>
				<td><input name="id"></td>
			</tr>
			<tr>
				<th>비밀번호</th>
				<td><input name="pwd" type="password"></td>
			</tr>
			<tr>
				<th>이메일</th>
				<td><input name="email"></td>
			</tr>
			<tr>
				<td colspan="2" align="center">
				<inputtype="button"value="등록" onclick="send(this.form);">
				<inputtype="button"value="취소" onclick="location.href='member_list.jsp';">				
				</td>
			</tr>		
		</table>
	</form>

</body>
</html>

```

### input태그에 작성한 내용을 받아서 실제로 테이블에 저장하기 위한 member_reg.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
    
    <%
    //post형태로 전송된 한글 데이터가 깨지지 않도록 처리하는 메서드
    request.setCharacterEncoding("utf-8");
    
    String name = request.getParameter("name");
    String id = request.getParameter("id");
    String pwd = request.getParameter("pwd");
    String email = request.getParameter("email");
    
    MemberVO vo = new MemberVO();
    vo.setName(name);
    vo.setId(id);
    vo.setPwd(pwd);
    vo.setEmail(email);
    
    int res = MemberDAO.getInstance().insert(vo); -> 오류나니까 만들어주기
    
    if(res >=1) {
    	response.sendRedirect("member_list.jsp");
    }
    
    
    %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
</body>
</html>
```

### MemberDAO.java에 insert 메서드 만들기
```java

_insert(MemberVO vo)

sql = "insert into member values(seq_member_idx,?,?,?,?)";

pstmt.setString(1,vo,getName());
pstmt.setString(2,vo,getId());
pstmt.setString(3,vo,getPwd());
pstmt.setString(4,vo,getEmail());

```

추가가 잘 되는지 확인하기

### 삭제 기능 구현해보기

```jsp
<% 
	//회원목록 가져오기
	List<MemberVO> member_list =MemberDAO.getInstance().dao.selectList();
%>

<html>
<head>

<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
<script>
	function del(idx) {
		alert(idx);
		if(!confirm("삭제하시겠습니까?"){
			return;
		}
		
		location.href="member_del.jsp?idx="+idx;
	}
</script>

</head>

<body>
	<table border="1">		
		<caption>::회원목록::</caption>
		
		<tr>
			<th>회원번호</th>
			<th>이름</th>
			<th>아이디</th>
			<th>비밀번호</th>
			<th>이메일</th>
			<th>비고</th>
		</tr>
		
		<% 
		for(int i = 0; i < member_list.size(); i++){ 
			MemberVO vo = member_list.get(i);
		%>
		
		<tr>
			<td><%= vo.getIdx()%></td>
			<td><%= vo.getName()%></td>
			<td><%= vo.getId()%></td>
			<td><%= vo.getPwd()%></td>
			<td><%= vo.getEmail()%></td>
			
			
			<td>
				<input type=button value="삭제" onclick="del('<%= vo.getIdx() %>')"/>
			</td>
		</tr>
		
		<% }%>
	</table>
	
	</body>

</html>

```

### 테이블에서 실제로 삭제해 주기 위한 member_del.jsp 생성하기

```jsp
<% 
	request.setCharacterEncoding(“utf-8”);
	int idx = Integer.parseInt( request.getParameter(“idx”));

	//idx에 해당하는 유저를 DB에서 삭제
	int res = MemberDAO.getInstance().delete(idx);

	if(res >= 1) {
		response.sendRedirect(“member_list.jsp”);
	}
%>

<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>		
</body>
</html>
```

### MemberDAO.java에 delete메서드 추가하기

```java
MemberDAO.java에서 _delete 생성.


_delete(int idx)

sql = "delete from member where idx=?";

pstmt.setInt(1,vo,getIdx());


실행 해서 삭제가 잘 되는지 확인
```
