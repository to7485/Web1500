# 성적관리 프로그램
- view를 만들어서 없는 컬럼을 사용하는법에 대해서도 알아보자.

## Ex_날짜_SungJuk 프로젝트 생성하기
- context.xml, 라이브러리 넣어주기
- service 패키지 넣어주기

### 성적테이블 만들기

```ORACLE
--시퀀스 생성
create sequence seq_sungtb_no;

--테이블 생성
create table sungtb
(
   no int primary key,    //no라는 이름의 기본키        
   name varchar2(100) not null,  //이름(빈값을 허용하지 않는다)
   kor  NUMBER(3) not null ,    //국어성적(기본값0) number 써도 됨 유효성체크할 때
   eng  NUMBER(3) not null,	빈칸이면 경고 띄울거기 때문에 not null 안해도 됨    
   mat  NUMBER(3) not null	//영어성적(기본값0)         
   //수학성적(기본값0)         
);

--샘플 데이터
insert into sungtb values(seq_sungtb_no.nextVal,'일길동', 77, 88, 99);

insert into sungtb values(seq_sungtb_no.nextVal,'이길동',97,98,99);

insert into sungtb values(seq_sungtb_no.nextVal,'삼길동',87,88,89);

commit;
```

(실습)다음과 같은 결과 만들어보기

![image](https://user-images.githubusercontent.com/54658614/232206636-34580aab-74c8-4892-a903-1594e33e8224.png)


#### SjVO.java 클래스 만들기
```java
package vo;

public class SjVO {
	//학생성적을 관리하는 클래스
		String name;//이름
		int kor;//국어
		int eng;//영어
		int mat;//수학
		int no;//학번

		
		public String getName() {
			return name;
		}
		public void setName(String name) {
			this.name = name;
		}
		public int getKor() {
			return kor;
		}
		public void setKor(int kor) {
			this.kor = kor;
		}
		public int getEng() {
			return eng;
		}
		public void setEng(int eng) {
			this.eng = eng;
		}
		public int getMat() {
			return mat;
		}
		public void setMat(int mat) {
			this.mat = mat;
		}
		public int getNo() {
			return no;
		}
		public void setNo(int no) {
			this.no = no;
		}
		
		

}

```

#### SjDAO.java 클래스 생성

```java
package dao;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.ArrayList;
import java.util.List;

import service.DBService;
import vo.SjVO;

public class SjDAO {
	// single-ton pattern: 
	// 객체1개만생성해서 지속적으로 서비스하자
	static SjDAO single = null;
	//_single 템플릿
	public static SjDAO getInstance() {
		//생성되지 않았으면 생성
		if (single == null)
			single = new SjDAO();
		//생성된 객체정보를 반환
		return single;
	}

	
	//_select_list 템플릿
	public List<SjVO> selectList() {

		List<SjVO> list = new ArrayList<SjVO>();
		Connection conn = null;
		PreparedStatement pstmt = null;
		ResultSet rs = null;

		String sql = "select * from sungtb";

		try {
			//1.Connection얻어온다
			conn = DBService.getInstance().getConnection();
			//2.명령처리객체정보를 얻어오기
			pstmt = conn.prepareStatement(sql);

			//3.결과행 처리객체 얻어오기
			rs = pstmt.executeQuery();

			while (rs.next()) {
				SjVO vo = new SjVO();

				//현재레코드값=>Vo저장
				vo.setName(rs.getString("name"));
				vo.setKor(rs.getInt("kor"));
				vo.setEng(rs.getInt("eng"));
				vo.setMat(rs.getInt("mat"));
				vo.setNo(rs.getInt("no"));

				//ArrayList추가
				list.add(vo);
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

		return list;
	}
}
```

#### student.jsp 생성

```jsp
<%@page import="vo.SjVO"%>
<%@page import="java.util.List"%>
<%@page import="dao.SjDAO"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>

<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">

<% 
	//학생정보 가져오기
	SjDAO dao = SjDAO.getInstance();
	List<SjVO> sj_list = dao.selectList();
	
	
	//테이블에는 총점과 평균을 따로 저장하는 컬럼은 없다.
	int total;
	float avg;
%>

<html>
<head>
	<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
	<title>Insert title here</title>
	
	<style>
		table{border:1px solid black; 
		      border-collapse:collapse;
		      text-align:center;}
		      
		th{width:80px;}
	</style>
	
</head>

<body>
	<table border="1">
			<caption>학생정보</caption>
			<tr>
				<th>이름</th>
				<th>국어</th>
				<th>영어</th>
				<th>수학</th>
				<th>총점</th>
				<th>평균</th>
			</tr>

			<%
				for (int i = 0; i < sj_list.size(); i++) {
					SjVO vo = sj_list.get(i);
			%>

			<tr>
				<td><%= vo.getName() %></td>
				<td><%= vo.getKor()%></td>
				<td><%= vo.getEng()%></td>
				<td><%= vo.getMat()%></td>
				
            총점을 구해주는 컬럼은 따로 없다.
				<% total = vo.getMat() + vo.getKor() + vo.getEng(); %>
				<td><%= total %></td>
				
            평균도 마찬가지
				<% avg = total / 3;%>
				<td><%= String.format("%.1f", avg)%></td>
			</tr>

			<%
				}
			%>

		</table>
</body>

</html>
```

내가 언제나 총점과 평균을 같이 조회하고 싶다면 DB시간에 배운 VIEW를 활용해보자.

```
--기존 성적테이블에서 총점과 평균까지 컬럼을 조회할 수 있도록 하는 VIEW 생성
CREATE VIEW SUNGTB_VIEW AS
(
   SELECT S.*,(KOR+ENG+MAT) TOT, ROUND((KOR+ENG+MAT)/3,1) AVG, RANK() OVER( ORDER BY (KOR+ENG+MAT) DESC) RANK
   FROM SUNGTB S;
);

```

#### SjVO.java 코드 수정하기

```java
package vo;

public class SjVO {
	//학생성적을 관리하는 클래스
		String name;//이름
		int kor;//국어
		int eng;//영어
		int mat;//수학
		int no;//학번
		int tot,rank;
		float avg;

		
		public String getName() {
			return name;
		}
		public void setName(String name) {
			this.name = name;
		}
		public int getKor() {
			return kor;
		}
		public void setKor(int kor) {
			this.kor = kor;
		}
		public int getEng() {
			return eng;
		}
		public void setEng(int eng) {
			this.eng = eng;
		}
		public int getMat() {
			return mat;
		}
		public void setMat(int mat) {
			this.mat = mat;
		}
		public int getNo() {
			return no;
		}
		public void setNo(int no) {
			this.no = no;
		}
		public int getTot() {
			return tot;
		}
		public void setTot(int tot) {
			this.tot = tot;
		}
		public int getRank() {
			return rank;
		}
		public void setRank(int rank) {
			this.rank = rank;
		}
		public float getAvg() {
			return avg;
		}
		public void setAvg(float avg) {
			this.avg = avg;
		}
}

```

#### SjDAO.java 코드 수정하기

```java
package dao;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.ArrayList;
import java.util.List;

import service.DBService;
import vo.SjVO;

public class SjDAO {
	// single-ton pattern: 
	// 객체1개만생성해서 지속적으로 서비스하자
	static SjDAO single = null;
	//_single 템플릿
	public static SjDAO getInstance() {
		//생성되지 않았으면 생성
		if (single == null)
			single = new SjDAO();
		//생성된 객체정보를 반환
		return single;
	}

	
	//_select_list 템플릿
	public List<SjVO> selectList() {

		List<SjVO> list = new ArrayList<SjVO>();
		Connection conn = null;
		PreparedStatement pstmt = null;
		ResultSet rs = null;

		String sql = "select * from sungtb_view";

		try {
			//1.Connection얻어온다
			conn = DBService.getInstance().getConnection();
			//2.명령처리객체정보를 얻어오기
			pstmt = conn.prepareStatement(sql);

			//3.결과행 처리객체 얻어오기
			rs = pstmt.executeQuery();

			while (rs.next()) {
				SjVO vo = new SjVO();

				//현재레코드값=>Vo저장
				vo.setName(rs.getString("name"));
				vo.setKor(rs.getInt("kor"));
				vo.setEng(rs.getInt("eng"));
				vo.setMat(rs.getInt("mat"));
				vo.setNo(rs.getInt("no"));
				vo.setTot(rs.getInt("tot"));
				vo.setAvg(rs.getFloat("avg"));
				vo.setRank(rs.getInt("rank"));

				//ArrayList추가
				list.add(vo);
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

		return list;
	}
}
```

#### student.jsp 코드 수정하기

```jsp
<%@page import="vo.SjVO"%>
<%@page import="java.util.List"%>
<%@page import="dao.SjDAO"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>

<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">

<% 
	//학생정보 가져오기
	SjDAO dao = SjDAO.getInstance();
	List<SjVO> sj_list = dao.selectList();
	
	
	//테이블에는 총점과 평균을 따로 저장하는 컬럼은 없다.
	int total;
	float avg;
%>

<html>
<head>
	<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
	<title>Insert title here</title>
	
	<style>
		table{border:1px solid black; 
		      border-collapse:collapse;
		      text-align:center;}
		      
		th{width:80px;}
	</style>
	
</head>

<body>
	<table border="1">
			<caption>학생정보</caption>
			<tr>
				<th>번호</th>
				<th>이름</th>
				<th>국어</th>
				<th>영어</th>
				<th>수학</th>
				<th>총점</th>
				<th>평균</th>
				<th>순위</th>
				<th>비고</th>
			</tr>

			<%
				for (int i = 0; i < sj_list.size(); i++) {
					SjVO vo = sj_list.get(i);
			%>

			<tr>
				<td><%= vo.getNo() %></td>
				<td><%= vo.getName() %></td>
				<td><%= vo.getKor()%></td>
				<td><%= vo.getEng()%></td>
				<td><%= vo.getMat()%></td>
				<td><%= vo.getTot() %></td>
				<td><%= vo.getAvg()%></td>
				<td><%= vo.getRank() %></td>
				<td><input type="button" value="삭제"></td>
			</tr>

			<%
				}
			%>

		</table>
</body>

</html>
```

## 성적 테이블에 테이터 추가하기

### student.jsp에 코드 추가하기

```jsp
......
			<tr>
				<td><%= vo.getNo() %></td>
				<td><%= vo.getName() %></td>
				<td><%= vo.getKor()%></td>
				<td><%= vo.getEng()%></td>
				<td><%= vo.getMat()%></td>
				<td><%= vo.getTot() %></td>
				<td><%= vo.getAvg()%></td>
				<td><%= vo.getRank() %></td>
				<td><input type="button" value="삭제"></td>
			</tr>

			<%
				}
			%>
			
			<tr>
				<td colspan="9" align=center">            location.href : 페이지를 이동할 수 있도록 하는 자바스크립트 코드
					<input type="button" value="학생정보 추가하기" onclick="location.href='sung_register.jsp'">
				</td>
			</tr>

```

### sung_register.jsp 생성

![image](https://user-images.githubusercontent.com/54658614/232208890-c455b830-360c-456f-91fc-1be4a8f1b22e.png)


```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>Insert title here</title>
		<script type="text/javascript">
		function send(f) {
			var name = f.name.value.trim();
			var kor = f.kor.value.trim();
			var eng = f.eng.value.trim();
			var mat = f.mat.value.trim();
			
			//유효성체크
			if( name == '') {
				alert("이름을 입력하세요");
				return;
			}
			
			var num = /^[0-9]{1,3}$/;
			if(!num.test(kor) || kor < 0 || kor >100) {
				alert("3자리 이하의 정수만 입력하세요");
				return;
			}
			var num = /^[0-9]{1,3}$/;
			if(!num.test(eng) || eng < 0 || eng >100) {
				alert("3자리 이하의 정수만 입력하세요");
				return;
			}
			var num = /^[0-9]{1,3}$/;
			if(!num.test(mat) || mat < 0 || mat >100) {
				alert("3자리 이하의 정수만 입력하세요");
				return;
			}
			
			f.action = "register.jsp";
			f.submit();
		}
		
		
	</script>
	</head>
	<body>
		<form>
		<table broder = 1>
			<catption>학생정보를 입력하세요</catption>
			
			<tr>
				<th>이름</th>
				<td><input name="name"></td>
			</tr>
			<tr>
				<th>국어</th>
				<td><input name="kor"></td>
			</tr>
			<tr>
				<th>영어</th>
				<td><input name="eng"></td>
			</tr>
			<tr>
				<th>수학</th>
				<td><input name="mat"></td>
			</tr>
			
			<tr>
				<td colspan="2">
					<input type="button" value="등록" onclick="send(this.form)"/>
					<input type="button" value="취소" onclick="location.href='student.jsp'">
				
				</td>
			</tr>
		</table>
	</form>
	</body>
</html>
```
## 정규표현식
- 문자열에서 특정 내용을 찾거나 대체 또는 발췌하는데 사용된다..
- 대표적으로 입력칸에 전화번호나 이메일을 입력하라고 했을 때 옳지 않은 값을 입력하면 정규표현식에 의해
- 필터링되어 걸러져 경고창을 띄우는 화면을 본적이 있을 것이다.
- 이처럼 반복문과 조건문을 사용해야 할것같은 복작한 코드도 정규표현식을 이용하면 매우 간단하게 표현할 수 있다.
- 단점은 여러가지 기호를 혼합하여 사용하기 때문에 가독성이 좋지 않다.

### 정규표현식 사용 예
1. 전화번호 데이터를 추출해야 할 때
2. 이메일,휴대폰번호,IP주소가 올바르게 입력되었는지 검증하고 싶을 때
3. 특정 조건과 위치에 따라서 문자열에 포함된 공백이나 특수문자를 제거하고 싶을 때

### 정규식의 구성
- 슬래쉬 문자 두개 사이로 정규식 기호가 들어가는 형태이다.
- 대부분의 언어의 정규식 문법은 비슷하기 때문에 하나의 언어의 정규식을 잘 익혀두면 다른 언어의 정규식을 익히는데 아주 빠르게 학습이 가능하다.
```js
/pattern/
```

### 정규식 메서드
- 위의 정규식을 가지고 이메일이나 전화번호 매칭 필터링을 하기 위해서는 자바스크립트 정규식 메서드를 이용하여 패턴을 검사하고 매칭되는 문자열을 추출, 변환한다.

|메서드|의미|
|------|-----|
|("문자열").match(/정규표현식/)|"문자열"에서 "정규표현식"에 매칭되는 항목들을 배열로 반환|
|("문자열").replace(/정규표현식/,"대체문자열")|"정규표현식"에 매칭되는 항목을 "대체문자열"로 변환|
|("문자열").split(/정규표현식/)|"문자열"을 "정규표현식"에 매칭되는 항목으로 쪼개어 배열로 반환|
|정규표현식.test("문자열")|"문자열"이"정규표현식"과 매칭되면 true,아니면 false를 반환|
|정규표현식.exec("문자열")|match메서드와 유사(단, 무조건 첫번째 매칭 결과만 반환|

### 정규식 기호 모음

|패턴|의미|
|-----|-----|
|a-z A-z| 영어 알파벳(-로 범위 지정)|
|ㄱ-ㅎ 가 - 힣|한글 문자(-으로 범위 지정)|
|0-9|숫자(-으로 범위 지정|
|.|모든 문자열(숫자,한글,영어,특수기호,공백 모두) 단,줄바꿈x|
|\\d|숫자|
|\\D|숫자가 아닌것|
|\\w|밑줄 문자를 포함한 영숫자 문자에 대응 [A-Za-z0-9_]와 동일|
|\\W|\\w가 아닌것|
|\\s|space 공백|
|\\S|space 공백이 아닌 것|

### 정규식 검색 기준 패턴

|기호|의미|
|-----|-----|
| \| | Or(또는)|
| [] | 괄호안의 문자들 중 하나. Or처리 묶음<br> /abc/ :"abc"를 포함하는<br>/[abc]/ : "a"또는"b"또는"c"를 포함하는<br>[다-바] : 다 or 라 or 마 or 바|
|[^문자] | 괄호안의 문자를 제외한것<br>[^lgEn] : "l","g","E","n" 4개 문자를 제외|
|^문자열| 특정 문자열로 시작(시작점)|
|문자열$| 특정 문자열로 끝남(종착점)|

### 정규식 갯수 반복 패턴

|기호|의미|
|-----|-----|
|?| 없거나 or 최대 한개만<br>/apple?/|
|*|없거나 or 있거나 (여러개)<br>/apple*/|
|+|최소 한개 or 여러개<br>/apple+/|
|{n}| n개|
|{Min,}|최소 Min개 이상|
|{Min,Max}|최소 Min개 이상, 최대 Max개 이하|


![image](https://user-images.githubusercontent.com/54658614/232209005-99b9d3e5-864c-4ddb-bdf2-f6e54ec425ab.png)

- 정보를 입력하고 등록버튼을 누르면 목적지로 파라미터가 넘어가게 된다.

### register.jsp 만들기

```jsp
<%
    
    	//register.jsp?name=산타클로스&kor=99&eng=80&mat=66
    	String name = request.getParameter("name");
    	int kor = Integer.parseInt(request.getParameter("kor"));
    	int eng = Integer.parseInt(request.getParameter("eng"));
    	int mat = Integer.parseInt(request.getParameter("mat"));

%>

DB연결과 관련된 정보는 DBService에 있고 데이터를 조작하는 쿼리문은 dao에 있다.
추가를 하기 위한 쿼리문도 함수로 만들고 함수만 호출해주자.
```

### SjDAO.java 에 코드 추가하기

```
....

//성적 추가
//insert,update,delete는 같은 함수를 사용한다.
//select와는 다르게 반환형이 int이다.

public int insert(SjVO vo) {
   // TODO Auto-generated method stub
   int res = 0;

   Connection conn = null;
   PreparedStatement pstmt = null;

   String sql = "";

   try {
      //1.Connection획득
      conn = DBService.getInstance().getConnection();
      //2.명령처리객체 획득
      pstmt = conn.prepareStatement(sql);

      //3.pstmt parameter 채우기

      //4.DB로 전송(res:처리된행수)
      res = pstmt.executeUpdate();

   } catch (Exception e) {
      // TODO: handle exception
      e.printStackTrace();
   } finally {

      try {
         if (pstmt != null)
            pstmt.close();
         if (conn != null)
            conn.close();
      } catch (SQLException e) {
         // TODO Auto-generated catch block
         e.printStackTrace();
      }
   }
   return res;
}
```

#### register.jsp에서 insert메서드 호출하기

```
<%
    
   //register.jsp?name=산타클로스&kor=99&eng=80&mat=66
   String name = request.getParameter("name");
   int kor = Integer.parseInt(request.getParameter("kor"));
   int eng = Integer.parseInt(request.getParameter("eng"));
   int mat = Integer.parseInt(request.getParameter("mat"));

	//SjDAO.getInstance().insert(name,kor,eng,mat);
   지금은 보낼 파라미터가 4개밖에 없지만 실제로 회원가입을 한다고 생각하면
   입력받은 데이터가 거의 10개는 될것 차라리 객체에 담아서 보내는것이 효율적이다.

	//db에 추가할 정보를 vo에 묶어둔다.
	SjVO vo = new SjVO();
	vo.setName(name);
	vo.setKor(kor);
	vo.setEng(eng);
	vo.setMat(mat);

	SjDAO.getInstance().insert(vo);

	if(res == 1) {
		//location.href=’student.jsp’;
		response.sendRedirect("student.jsp");
	}

    
    %>
```

#### register.jsp에서 호출한 insert함수로 보낸 파라미터 받아서 쿼리문에 넣어주기

```
package dao;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.ArrayList;
import java.util.List;

import service.DBService;
import vo.SjVO;

public class SjDAO {
	// single-ton pattern: 
	// 객체1개만생성해서 지속적으로 서비스하자
	static SjDAO single = null;
	//_single 템플릿
	public static SjDAO getInstance() {
		//생성되지 않았으면 생성
		if (single == null)
			single = new SjDAO();
		//생성된 객체정보를 반환
		return single;
	}


	//성적 추가
	//insert,update,delete는 같은 함수를 사용한다.
	public int insert(SjVO vo) {
		// TODO Auto-generated method stub
		int res = 0;

		Connection conn = null;
		PreparedStatement pstmt = null;

      우리가 데이터를 넣어야 할 자리를 ?로 채워넣는다.
		String sql = "insert into sungtb values(seq_sungtb_no.nextVal,?,?,?,?)";

		try {
			//1.Connection획득
			conn = DBService.getInstance().getConnection();
			//2.명령처리객체 획득
			pstmt = conn.prepareStatement(sql);

			//3.pstmt parameter 채우기
         //물을표 자리에 데이터로 채워주겠다.
			pstmt.setString(1, vo.getName());
			pstmt.setInt(2, vo.getKor());
			pstmt.setInt(3, vo.getEng());
			pstmt.setInt(4, vo.getMat());

			//4.DB로 전송(res:처리된행수)
			res = pstmt.executeUpdate();

		} catch (Exception e) {
			// TODO: handle exception
			e.printStackTrace();
		} finally {

			try {
				if (pstmt != null)
					pstmt.close();
				if (conn != null)
					conn.close();
			} catch (SQLException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
		}
      데이터가 잘 들어갔으면 1 잘 안들어가면 0을 반환한다.
		return res;
	}
}
```

![image](https://user-images.githubusercontent.com/54658614/232209990-f13d917a-2859-4b35-8bac-2b6d28b8da7d.png)


#### 데이터를 성적테이블에 추가하고 student.jsp로 돌아가기

```
<%@page import="vo.SjVO"%>
<%@page import="dao.SjDAO"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
    
<%
    
    	//register.jsp?name=산타클로스&kor=99&eng=80&mat=66
    	String name = request.getParameter("name");
    	int kor = Integer.parseInt(request.getParameter("kor"));
    	int eng = Integer.parseInt(request.getParameter("eng"));
    	int mat = Integer.parseInt(request.getParameter("mat"));

	//SjDAO.getInstance().insert(name,kor,eng,mat);

	//db에 추가할 정보를 vo에 묶어둔다.
	SjVO vo = new SjVO();
	vo.setName(name);
	vo.setKor(kor);
	vo.setEng(eng);
	vo.setMat(mat);

	int res = SjDAO.getInstance().insert(vo);

	if(res == 1) {
		//location.href=’student.jsp’; -> 자바스크립트코드이기 때문에 사용하지 못한다.
		
		//기능은 똑같지만 자바코드로 작성한다면...
		response.sendRedirect("student.jsp");
	}

    
    %>
```

## 데이터 삭제하기
- 비고란에 만들어놨던 삭제버튼을 누르면 테이블에서 특정 학생에 대한 정보만 삭제가 되게 기능을 만들어보자.
- 삭제는 DELETE문을 쓴다.(의도한 데이터가 아닌 다른 데이터를 지우지 않도록 주의하자)
	- DELETE FROM 테이블명 WHERE 컬럼명 = 데이터;

#### student.jsp 코드 추가하기

```jsp
	<script type="text/javascript">
		//삭제버튼 클릭에 대한 이벤트처리
		function del(no){			
			if(confirm("정말 삭제하시겠습니까?") == false){
				return; //아니오 버튼 클릭시
			}
			
			//예 버튼 클릭시(no를 GET방식으로 파라미터를 전달)
			location.href = 'sung_del.jsp?no=' + no;
		}

	</script>

...
<%
		for (int i = 0; i < sj_list.size(); i++) {
			SjVO vo = sj_list.get(i);
	%>

	<tr>
		<td><%= vo.getName() %></td>
		<td><%= vo.getKor()%></td>
		<td><%= vo.getEng()%></td>
		<td><%= vo.getMat()%></td>

		<td><%= vo.getTotal() %></td>
		<td><%= vo.getAvg() %></td>
		<td><%= vo.getRank() %></td>

		<td><input type=button value="삭제" 
		onclick="del('<%= vo.getNo() %>')"/></td>
		삭제 버튼 클릭시 학생의 번호를 파라미터로 전달해준다.
		
	</tr>

	<%}%>

```

삭제버튼 클릭시 목적지로 파라미터를 갖고 간다.

![image](https://user-images.githubusercontent.com/54658614/232223257-01e1e94f-25d2-4be4-b152-fdf093db73ae.png)


#### sung_del.jsp 작성하기

```jsp
<%@page import="dao.SjDAO"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
	//수신시 인코딩(GET일경우에는 상관없지만 POST일때는 한글이 깨진다.
	//그러나 GET일때도 습관처럼 써주는 것이 좋다.) 처리
	request.setCharacterEncoding("utf-8");
	
	//sung_del.jsp?no=1
	int no = Integer.parseInt(request.getParameter("no"));
	
	//db에서 값 제거
	//res가 0이면 실패했다는 뜻.
	int res = SjDAO.getInstance().delete(no); //<-- delete메서드를 만들지 않았으므로 오류
	
	//갱신된 데이터를 다시 로드한다. student.jsp
	if(res >=1) { //ex) 김씨를 홍씨로 바꾸는데 10명이 바뀐다면 10이 넘어옴
	response.sendRedirect("student.jsp");
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

#### SjDAO.java에 delete 메서드 추가하기

```java
//삭제하기
	public int delete(int no) {
		// TODO Auto-generated method stub
		int res = 0;

		Connection conn = null;
		PreparedStatement pstmt = null;

		String sql = "delete from sungtb where=?";

		try {
			//1.Connection획득
			conn = DBService.getInstance().getConnection();
			//2.명령처리객체 획득
			pstmt = conn.prepareStatement(sql);

			//3.pstmt parameter 채우기
			pstmt.setInt(1, no);
			//4.DB로 전송(res:처리된행수)
			res = pstmt.executeUpdate();

		} catch (Exception e) {
			// TODO: handle exception
			e.printStackTrace();
		} finally {

			try {
				if (pstmt != null)
					pstmt.close();
				if (conn != null)
					conn.close();
			} catch (SQLException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
		}
		return res;
	}

```
student.jsp에서 실행하여 삭제가 잘되는지 확인하기

## 수정하기
- 만약 하나의 점수만 수정하고 싶은데 모든 내용을 다시 입력해야 하면 매우 불편할 것이다.

![image](https://user-images.githubusercontent.com/54658614/232227516-cf1129ad-6e0c-4548-a8b1-c35e3983e342.png)

- 수정을 할 때는 정보를 미리 넣어두고 수정하고 싶은 부분만 바꿔서 UPDATE가 될 수 있도록 하자.

#### student.jsp에 수정버튼 추가하기

```jsp
<%@page import="vo.SjVO"%>
<%@page import="java.util.List"%>
<%@page import="dao.SjDAO"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">

<% 
	..............
%>

<html>
<head>
	<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
	<title>Insert title here</title>
	
	
	<script type="text/javascript">
		function del(no){			
			...........................
		}
		
		//수정버튼시 입력을 위한 팝업창을 보인다
		function modify(no, name, kor, eng, mat){
			location.href=
				'sung_update.jsp?no='+no+"&name="+name+"&kor="+kor+"&eng="+"&mat="+mat;
			
			}//update()	
	</script>
</head>

<body>
	<table border="1">
		....................

		<td><input type=button value="수정" 
 			   onclick="modify('<%= vo.getNo() %>', 
				            '<%= vo.getName() %>',
				            '<%= vo.getKor()%>', 
				            '<%= vo.getEng() %>', 
				            '<%= vo.getMat() %>')"/></td>
					    
		파라미터를 5개나 던저야 한다 오타가 나지 않게 주의하자.
		<td><input type=button value="삭제" 
                       onclick="del('<%= vo.getNo() %>')"/></td>
				
	</table>					
</body>

</html>
```

#### sung_update.jsp 생성하기

```jsp
<%@page import="dao.SjDAO"%>
<%@page import="vo.SjVO"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">

<%
	//수신시 인코딩(GET일경우에는 상관없지만 POST일때는 한글이 깨진다.
	//그러나 GET일때도 습관처럼 써주는 것이 좋다.) 처리
	request.setCharacterEncoding("utf-8");

//sung_update.jsp?no=+no+&name=+name+&kor=+kor+&eng=+&mat=+mat;

	//전송 버튼 클릭시 넘어온 파라미터를 받는다.
	String name = request.getParameter("name");
	int no = Integer.parseInt(request.getParameter("no"));
	int kor = Integer.parseInt(request.getParameter("kor"));
	int eng = Integer.parseInt(request.getParameter("eng"));
	int mat = Integer.parseInt(request.getParameter("mat"));

%>
<script>
	function update(f){
		//var no = f.no.value
		//유효성체크 했다쳐
		f.action="sung_modi.jsp";
		f.submit();
		
	}

</script>

<body>
	<form>
		<input type="hidden" name="no" value="<%=no%>">
	
	<table border="1">
		<caption>학생정보 수정</caption>
		<tr>
			<th>이름</th>
			<td><input name="name" value="<%= name %>"></td>
		</tr>
		<tr>
			<th>국어</th>
			<td><input name="kor" value="<%= kor %>"></td>
		</tr>
		<tr>
			<th>영어</th>
			<td><input name="eng" value="<%= eng %>"></td>
		</tr>
		<tr>
			<th>수학</th>
			<td><input name="mat" value="<%= mat %>"></td>
		</tr>
		<tr>
			<td colspan="2">
				<input type="button" value="수정"
						onclick="update(this.form);">
				<input type="button" value="취소" 											onclick="location.href='student.jsp'">
			</td>
		</tr>
	</table>		
	</form>		
</body>

```

#### 변경한 내용을 받아서 update 해줄 sung_modi.jsp 만들기

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
    
    <%
    	request.setCharacterEncoding("utf-8");
    
    	String name= request.getParameter("name");
	int no = Integer.parseInt(request.getParameter("no"));
	int kor = Integer.parseInt(request.getParameter("kor"));
	int eng = Integer.parseInt(request.getParameter("eng"));
	int mat = Integer.parseInt(request.getParameter("mat"));

	SjVO vo = new SjVO();
    		vo.setNo(no);
    		vo.setName(name);
    		vo.setKor(kor);
    		vo.setEng(eng);
    		vo.setMat(mat);
    	
    	//DB에 값 추가하기.
	int res = SjDAO.getInstance().update(vo); <-- 업데이트 메서드 없으므로 생성하자
	System.out.println(res);

	//갱신된 데이터를 다시 로드한다. student.jsp
	if(res >= 1) {
	  response.sendRedirect(“student.jsp”);
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
#### SjDAO.java에 update메서드 추가하기
```java
//성적수정 메서드
	//_insert_update_delete 템플릿
	public int update(SjVO vo) {
		
		int res = 0;
		Connection conn = null;
		PreparedStatement pstmt = null;

		String sql = "update sungtb set name=?, kor=?, eng=?, mat=? where no=?";

		try {
			//1.Connection획득
			conn = DBService.getInstance().getConnection();
			//2.명령처리객체 획득
			pstmt = conn.prepareStatement(sql);

			//3.pstmt parameter 채우기
			pstmt.setString(1, vo.getName());
			pstmt.setInt(2, vo.getKor());
			pstmt.setInt(3, vo.getEng());
			pstmt.setInt(4, vo.getMat());
			pstmt.setInt(5, vo.getNo());

			//4.DB로 전송(res:처리된행수)
			res = pstmt.executeUpdate();

		} catch (Exception e) {
			..............
		} finally {
			..............
		}
		return res;
	}

```

