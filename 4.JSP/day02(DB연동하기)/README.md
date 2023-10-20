# Ex_날짜_DB 프로젝트 생성하기

## JDBC로 이클립스와 오라클DB 연동하기
- JDBC(Java DataBase Connectivity)는 자바/JSP 프로그램 내에서 데이터베이스와 관련된 작업을 처리할 수 있도록 도와주는 방법이다.
- 자바 표준 인터페이스로, 관계형 데이터베이스 시스템에 접근하여 SQL문을 실행하기 위한 자바 API 또는 자 JDBC API를 사용하면 DBMS 종류에 상관없이 데이터베이스 작업을 처리할 수 있습니다.
- JDBC API는 java.sql.\* 패키지에 의해 구현됩니다.
- 프로젝트를 생성한 후 WebContent의 META-INF에 context.xml(실행시 최초로 호출되는 설정파일)을, WEB-INF lib폴더에 준비된 4개의 라이브러리를 넣는다.

![image](https://user-images.githubusercontent.com/54658614/231060513-bec2d76e-3f09-4ccc-844c-5755e70b7e49.png)

- commons-collections-3.1.jar 
  - Java Collection Framework을 상속 확장

- commons-dbcp-1.2.1.jar 
  - Database connection pooling 서비스 제공  데이타소스를 org.apache.commons.dbcp.BasicDataSource를 사용

- commons-pool-1.4.jar
  - 데이터베이스와 연결된 커넥션을 미리 만들어 풀(pool)에 저장해 두었다가 필요할 때에 가져다 쓰고 반환하는 기법이다. <br>아파치에서 제공하는 DBCP (DataBase Connection Pool) API를 이용해서 사용할 수 있다.

### context.xml

![image](https://user-images.githubusercontent.com/54658614/231063002-1c32db00-19f9-4395-b233-e03dda8a5d43.png)

```xml
<?xml version="1.0" encoding="UTF-8"?>

리소스를 미리 준비해두고 DB에 연결해야 하는 상황에
찾아서 사용하는 구조를 JNDI(Java Naming Directory Interface)

<Context>
	<Resource DB로 접속하기 위한 설정들에 대한 정보
	        auth="Container"
      		name="jdbc/oracle_test"
      		type="javax.sql.DataSource"
      		driverClassName="oracle.jdbc.driver.OracleDriver"
      		factory="org.apache.commons.dbcp.BasicDataSourceFactory"
      		url="jdbc:oracle:thin:@localhost:1521:xe"
      		username="test" password="1111" -> 어떤 계정으로 로그인을 할건지 
      		maxActive="20" maxIdle="10" maxWait="1"/>
          
          maxActive(최대연결수) : 현재 프로젝트에서 오라클 DB에 연결하는 시간이 1초라고 가정한다면 10명이 동시에 접속을 시도할 경우 10번째 사람은 10초
          뒤에 DB로 접속할 수 있다. 따라서 미리 10명이 동시에 접속할 수 있도록 준비를 해 두면 대기시간 없이 DB에 효율적으로 접근할 수 있다.
          이를 DBCP(Database Connection Pool)이라고 한다.
          
          maxWait : 1초동안만 기다림, 1초 지나면 포기 -1은 무한대기          
</Context>
```
- auth="Container" : "Container" 값은 컨테이너(애플리케이션 서버나 서블릿 컨테이너)가 인증을 처리하도록 지시하는 것을 나타냅니다.
- name="jdbc/oracle_test" : 데이터 소스의 이름을 지정합니다. 애플리케이션에서 이 이름을 사용하여 데이터베이스에 연결할 수 있습니다.
- type="javax.sql.DataSource" : 이 데이터 소스가 javax.sql.DataSource 인터페이스를 구현하는 데이터 소스임을 나타냅니다.
- driverClassName="oracle.jdbc.driver.OracleDriver" : Oracle 데이터베이스와 Java 애플리케이션 간의 연결을 제공하는 JDBC 드라이버의 클래스<br>OracleDriver는 Oracle 데이터베이스와 상호작용하고 데이터베이스 연결 및 쿼리 실행을 처리하는 Java 애플리케이션에서 사용된다.<br>Oracle 데이터베이스와의 통신을 담당하며, Java 코드가 Oracle 데이터베이스와 상호작용할 수 있게 해줍니다.
- factory="org.apache.commons.dbcp.BasicDataSourceFactory" : 클래스는 데이터 소스 객체를 생성하는 역할을 한다.데이터베이스 연결 풀을 설정하고 관리할 수 있으며,<br>이를 통해 애플리케이션에서 데이터베이스 연결을 효율적으로 사용할 수 있다.<br>설정된 데이터베이스 연결 풀은 데이터베이스 연결 요청이 있을 때 연결을 제공하고, 연결을 사용한 후에 다시 풀에 반환한다.
- url="jdbc:oracle:thin:@localhost:1521:xe" : jdbc:oracle:드라이버타입:[유저이름/패스워드]@호스트이름[:포트번호][서비스네임]
	- JDBC드라이버는 THIN방식과 OCI방식이 있다.
	- THIN : 순수하게 자바 패키지(클래스)만으로 바로 DB와 연결, 범용성이 높다. 상대적으로 OCI보다 속도가 느리다.
	- OCI : Oracle Call Interface 특정 운영체제 내에서만 돌아가는 모듈을 통해 DB에 연결한다.

현재 계정에 접속해도 테이블이 존재하지 않는다.

### 테이블 만들기
1. Start Database접속하기

![image](https://user-images.githubusercontent.com/54658614/231349811-0c1f24e8-a129-432e-a06e-a3dbe4427378.png)

2. TEST_PM 로그인하기

![image](https://user-images.githubusercontent.com/54658614/231349937-d7b2ce2d-94b0-44c4-bb31-370cdf8d0c66.png)

3. 부서테이블 텍스트만들고 테이블만들기

![image](https://user-images.githubusercontent.com/54658614/231350378-b01e0240-f2b1-49ef-98c8-15bde0eecc3e.png)

4. 복사 붙혀넣기 (오타 확인하기! 테이블이 이미 존재하면 DROP TABLE로 테이블 삭제한 후 만들기)

![image](https://user-images.githubusercontent.com/54658614/231350540-bdd25ced-fb9d-41ed-94f9-1ea32d957ca4.png)

5. 데이터 추가해보기

![image](https://user-images.githubusercontent.com/54658614/231351074-c5a229e0-104d-451a-9cc2-f3464e96ce1d.png)

6. 고객 테이블과 사원테이블 만들기
#### 고객 테이블
```
CREATE TABLE GOGEK(
	GOBUN NUMBER(3) PRIMARY KEY, --고객번호
	GONAME VARCHAR2(50), --고객 이름
	GOADDR VARCHAR2(50), --고객 주소
	GOJUMIN VARCHAR2(20), -- 주민번호
	GODAM NUMBER(3) -- 담당자 번호
);

insert into gogek values(1, '류민', '서울 강남구', '660215-1234567', 3);
insert into gogek values(2, '강청', '대정 유성구', '760815-1234567', 4);
insert into gogek values(3, '영희', '부산 강서구', '791015-2345678', 10);
insert into gogek values(4, '순이', '인천 계양구', '911105-2234567', 10);
insert into gogek values(5, '마징가', '서울 동작구', '860212-1111111', 1);
insert into gogek values(6, '짱가', '서울 강북구', '801215-1223345', 10);
insert into gogek values(7, '아톰', '경기 안양시', '770225-1234567', 9);
insert into gogek values(8, '스머프', '서울 강남구', '850205-1234567', 8);
insert into gogek values(9, '투덜이', '서울 강서구', '880215-1567899', 7);
insert into gogek values(10, '슛돌이', '서울 강북구', '911115-2234567', 6);
COMMIT;
```
#### 사원테이블
```
CREATE TABLE SAWON(
	SABUN NUMBER(3) PRIMARY KEY,
	SANAME VARCHAR2(50),
	SAGEN VARCHAR2(10), -- 성별
	DEPTNO NUMBER(3), --부서번호
	SAJOB VARCHAR2(10), --직책
	SAHIRE DATE, --입사일
	SAMGR NUMBER(3), --상사번호
	SAPAY NUMBER(10) --연봉
);

insert into sawon values(1, '장동건', '남자', 40, '부장', '1993-07-25', null, 4000);
insert into sawon values(2, '안재욱', '남자', 20, '부장', '1988-02-25', null, 4000);
insert into sawon values(3, '이미자', '여자', 20, '대리', '1998-03-25', 2, 3500);
insert into sawon values(4, '김민종', '남자', 20, '사원', '2001-03-15', 3, 2400);
insert into sawon values(5, '최불암', '남자', 10, '부장', '1984-07-25', null, 4000);
insert into sawon values(6, '최민수', '남자', 10, '사원', '2001-04-30', 4, 2000);
insert into sawon values(7, '김혜수', '여자', 30, '과장', '2004-05-25', 10, 3900);
insert into sawon values(8, '김용만', '남자', 40, '과장', '1993-08-15', 1, 3000);
insert into sawon values(9, '배슬기', '여자', 20, '대리', '1997-11-25', 8, 3200);
insert into sawon values(10, '감우성', '남자', 10, '대리', '1998-05-25', 4, 3300);

COMMIT;
```
이제 데이터베이스에 테이블은 준비가 됐고 이클립스를 통해 브라우저에 조회를 해보자.

### wx01_jdbc_dept jsp파일 생성하기

- db에 연동을 하는 코드들이 자바코드 (java.sql 패키지)이기 때문에 스크립트릿을 안쓸 수 없다.
```jsp
<%@page import="java.sql.Connection"%>
<%@page import="javax.sql.DataSource"%>
<%@page import="javax.naming.Context"%>
<%@page import="javax.naming.InitialContext"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">

<% 
	//톰캣이 JNDI를 검색하기 위해 필요한 클래스(JNDI 기법:java naming directory interface )
	InitialContext ic = new InitialContext();
	//자바에 존재하는 클래스 JAVAX -> 라이브러리에서 왔다고 생각하면 됨
	//내가 만든적은 없는데 DB연동하려면 필요는 하고, 직접 만들자니 시간도 오래걸린다.
		
	//Resource위치 검색 -> CONTEXT.XML에 있는 RESOURCE만을 의미하는건 아니다. 	
	프로그램을 구현하기 위한 모든 참조파일을 다 리소스라고 할 수 있다.

	
	
	Context ctx = (Context)ic.lookup("java:comp/env");
	//javax.naming.Context 인터페이스 import	
	//lookup -> 조회, jsp에서 db에 대한 리소스가 저장되어 있는 위치는 지정되어 있다.
	//java:comp/env <- 자바에 내장되어 있는 리소스 자원을 검색하는 상수(고정)

	//java.sql 인터페이스 improt	
	//검색된 Resource를 통해 필요한 JNDI의 자원을 검색
	//context.xml파일의 Resource영역에 참조되어 있는 name속성값
	DataSource ds=(DataSource)ctx.lookup("jdbc/oracle_test");
	
	
	//java.sql.Connection import
	//위에서 준비해둔 경로로 로그인 시도(접속)
	Connection conn = ds.getConnection();
	
	System.out.println("---get connection!!---");
	
	//context.xml의 maxActive=10이라면 11번째 사용자는 connection이 없어 연결할 수 없다.
	//그러므로 컨넥션을 연결하여 사용한 후에는 (연결객체를)반드시 제자리에 돌려놓아야 한다.
	//연결 후 사용한 DB는 종료코드를 통해 마무미를 지어줘야 한다.
	conn.close();//DBCP에 다시 돌려놓는 개념. 주석달고도 확인해보자.
%>

<html>

<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>DB연동을 통한 부서 테이블 조회</title>
</head>

<body>
	
</body>

</html>
```
콘솔에 뜬다면 db와의 연결이 성공한 것이다.

![image](https://user-images.githubusercontent.com/54658614/231358544-efba9a78-0f15-4420-bfe1-8b1104436394.png)

이제 DEPT테이블의 내용을 읽어와서 테이블 형식으로 조회해보자.

DEPT의 세가지 컬럼을 변수로 만들어서 저장을 할 것이다. 변수는 자바 클래스이기 때문에 src영역에 만든다.

![image](https://user-images.githubusercontent.com/54658614/231358915-4a3c5456-25c4-46a5-a27e-ee0de04e8f45.png)

```java
package vo;

public class DeptVO {

	//테이블의 컬럼이름과 되도록이면 같은 이름으로 만들자.
	private int deptno;
	private String dname;
	private String loc;
	
	public int getDeptno() {
		return deptno;
	}
	public void setDeptno(int deptno) {
		this.deptno = deptno;
	}
	public String getDname() {
		return dname;
	}
	public void setDname(String dname) {
		this.dname = dname;
	}
	public String getLoc() {
		return loc;
	}
	public void setLoc(String loc) {
		this.loc = loc;
	}
}

```

### ex01_jdbc_dept에 코드 추가하기

![image](https://user-images.githubusercontent.com/54658614/231361772-5c6e984b-f293-4fb8-9c5c-b578719be408.png)


```
<%@page import="vo.DeptVO"%>
<%@page import="java.util.ArrayList"%>
<%@page import="java.sql.ResultSet"%>
<%@page import="java.sql.PreparedStatement"%>
<%@page import="java.sql.Connection"%>
<%@page import="javax.sql.DataSource"%>
<%@page import="javax.naming.Context"%>
<%@page import="javax.naming.InitialContext"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">

<% 
	...............
		
	//명령처리객체 얻어오기
	String sql = "select * from dept"; -> sql문을 인식하는 객체
	
	//java.sql.PreparedStatement -> 문자열을 쿼리문으로 알아먹을수 있게 변경해줌
	Connection conn = ds.getConnection(); 어느 db로 연결했는지 정보를 알고있는 객체
	PreparedStatement pstmt = conn.prepareStatement(sql);
	보이지 않는 커서가 테이블로 접근한다.
	
	한칸씩 내려가게 해주는 객체가 따로 있다.
	//데이터베이스에서 데이터를 가져와서 결과집합을 반환한다.
	ResultSet rs = pstmt.executeQuery();
	
	
	//부서목록을 저장할 ArrayList
	List.java.util
	List<DeptVO> dept_list = new ArrayList<DeptVO>();
	while( rs.next() ){//데이터가 있다면
		DeptVO vo = new DeptVO();
	
		//현제 레코드의 필드값을 vo에 담는다.
		vo.setDeptno( rs.getInt("deptno") );
		vo.setDname(rs.getString("dname"));
		vo.setLoc(rs.getString("loc"));
		
		dept_list.add(vo); 잊어버리지 말라고 arrayList에 추가해주기
	}
	
	//연결 후 사용한 DB는 종료코드를 통해 마무리를 지어줘야 한다.
	//DB접속과 관련된 모든 객체는 생성된 역순으로 반드시 닫아주자!!
	rs.close();
	pstmt.close();
	conn.close();
%>

<html>

<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>

</head>

<body>
	<!-- 문제 : 위의 dept_list가 가지고 있는 결과값을 테이블로 출력하시오. -->
	<table border="1">
		<caption>부서목록</caption>
		
		<tr>
			<th>부서번호</th>
			<th>부서명</th>
			<th>부서위치</th>
		</tr>
		
		<% for( int i = 0; i < dept_list.size(); i++ ){
			DeptVO dv = dept_list.get(i);%>
			
		<tr>
			<td> <%= dv.getDeptNo() %> </td>
			<td> <%= dv.getdName() %> </td>
			<td> <%= dv.getLoc() %> </td>
		</tr>
		
		<%} %>
		
	</table>
	
</body>

</html>
```
이런 형태를 모델1 이라고 한다.<br>
모델1은 데이터를 처리하는 로직과 화면을 출력하는 로직을 한 페이지에서 처리하고 해결한다.<br>

각 부서명을 클릭했을 때 부서에 속하는 사원들의 내용을 출력하기

![image](https://user-images.githubusercontent.com/54658614/231652565-24183e0e-365b-4c90-83b0-fc3b27b97eae.png)


```jsp
<%
	.................
%>

<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>

	<script type="text/javascript">
		
		function send(data){
			
			var f = document.m_form; this.form으로 전달을 못했으니 이름으로 검색하기
			var hid = f.deptno;
			hid.value = data; input태그에 데이터 넣고 확인하기
			
			f.action = "sawon_list.jsp";
			f.submit(); submit을 해도 값을 전달할 수 있는 input 태그가 없어서 난감해진다.
			
		}//send()    
	
	</script>
	
</head>

<body>
	<form name="m_form">
		<table border="1">
			<caption>부서목록</caption>

			<tr>
				<th>부서번호</th>
				<th>부서명</th>
				<th>부서위치</th>
			</tr>

			<%
				for (int i = 0; i < dept_list.size(); i++) {
					DeptVO dv = dept_list.get(i);
			%>

			<tr>
				<td><%=dv.getDeptNo()%></td>
				
				<td> <!-- a태그에 메서드 연결하기(잘안씀) 그냥 메서드를 호출하면 잘 안되기 때문에 앞에 javascript:를 써줘야 한다. -->
				<a href="javascript:send('<%=dv.getDeptNo()%>');"> 
					<%=dv.getdName()%>
					</a>
				</td>
				<td><%=dv.getLoc()%></td>
			</tr>

			<%
				}
			%>

		</table>
		
		굳이 보여줄 필요가 없기 때문에 type을 hidden으로 해주자.
		<input type="hidden" name="deptno">
				
	</form>
	
</body>

</html>
```

#### SawonVO.java 생성하기
```java
package vo;

public class SawonVO {
	private int sabun; //사원번호
	private String saname;//사원이름
	private int deptno;//부서번호
	private int sapay;//연봉
	
	public int getSabun() {
		return sabun;
	}
	public void setSabun(int sabun) {
		this.sabun = sabun;
	}
	public String getSaname() {
		return saname;
	}
	public void setSaname(String saname) {
		this.saname = saname;
	}
	public int getDeptno() {
		return deptno;
	}
	public void setDeptno(int deptno) {
		this.deptno = deptno;
	}
	public int getSapay() {
		return sapay;
	}
	public void setSapay(int sapay) {
		this.sapay = sapay;
	}
	
	
	
	
}

```

#### sawon_list.jsp 생성하기
```
<%@page import="java.sql.ResultSet"%>
<%@page import="java.sql.PreparedStatement"%>
<%@page import="java.util.ArrayList"%>
<%@page import="java.util.List"%>
<%@page import="vo.SawonVO"%>
<%@page import="javax.naming.Context"%>
<%@page import="java.sql.Connection"%>
<%@page import="javax.sql.DataSource"%>
<%@page import="javax.naming.InitialContext"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
    
    
<%
	//sawon_list.jsp?deptno=20
	//deptno라는 이름의 파라미터 값을 수신
	int no = Integer.parseInt(request.getParameter("deptno"));

	//톰캣이 JNDI를 검색하기 위해 필요한 클래스(JNDI 기법:java naming directory interface )
	InitialContext ic = new InitialContext();
	//자바에 존재하는 클래스 JAVAX -> 라이브러리에서 왔다고 생각하면 됨
	//내가 만든적은 없는데 DB연동하려면 필요는 하고, 직접 만들자니 시간도 오래걸린다.
		
	//Resource위치 검색 -> CONTEXT.XML에 있는 RESOURCE만을 의미하는건 아니다. 	
	//프로그램을 구현하기 위한 모든 참조파일을 다 리소스라고 할 수 있다.
	//java:comp/env <- 자바에 내장되어 있는 리소스 자원을 검색하는 상수(고정)
	Context ctx = (Context)ic.lookup("java:comp/env");
	//javax.naming.Context	
	//lookup -> 조회 jsp에서 db에 대한 리소스가 저장되어 있는 위치는 지정되어 있다.
	

	//java.sql 인터페이스 improt	
	//검색된 Resource를 통해 필요한 JNDI의 자원을 검색
	//context.xml파일의 Resource영역에 참조되어 있는 name속성값
	DataSource ds =(DataSource)ctx.lookup("jdbc/oracle_test");
	
	//java.sql.PreparedStatement -> 문자열을 쿼리문으로 알아먹을수 있게 변경해줌
	Connection conn = ds.getConnection(); //어느 db로 연결했는지 정보를 알고있는 객체
	
	
	//명령처리객체 얻어오기
	String sql = "select * from sawon where deptno = "+no; //-> sql문을 인식하는 객체
	
	PreparedStatement pstmt = conn.prepareStatement(sql);
	//보이지 않는 커서가 테이블로 접근한다.
	
	
	//데이터베이스에서 데이터를 가져와서 결과집합을 반환한다.
	ResultSet rs = pstmt.executeQuery(); //조회문(select,show)을 실행할 목적으로 서용한다.
	
	//부서목록을 저장할 ArrayList
	//List.java.util
	List<SawonVO> sawon_list = new ArrayList<SawonVO>();
	//한칸씩 내려가게 해주는 객체가 따로 있다.
	while( rs.next() ){//데이터가 있다면
		SawonVO vo = new SawonVO();
		vo.setDeptno(rs.getInt("deptno"));
		vo.setSabun(rs.getInt("sabun"));
		vo.setSaname(rs.getString("saname"));
		vo.setSapay(rs.getInt("sapay"));
		
		sawon_list.add(vo);
	}
	
	//연결 후 사용한 DB는 종료코드를 통해 마무리를 지어줘야 한다.
	//DB접속과 관련된 모든 객체는 생성된 역순으로 반드시 닫아주자!!
	rs.close();
	pstmt.close();
	conn.close();
%>   
<!DOCTYPE html>
<html>
	<head>
		<meta charset="EUC-KR">
		<title>Insert title here</title>
	</head>
	<body>
		<table border="1">
			<tr>
				<th>사번</th>
				<th>이름</th>
				<th>급여</th>
				<th>부서번호</th>
			</tr>
			<%for(int i = 0; i<sawon_list.size(); i++){
				SawonVO vo = sawon_list.get(i); %>
			<tr>
				<td><%= vo.getSabun()  %></td>
				<td><%= vo.getSaname()  %></td>
				<td><%= vo.getSapay()  %></td>
				<td><%= vo.getDeptno()  %></td>
			</tr>
			<%} %>
		</table>
	</body>
</html>
```

### 고객테이블 조회해보기

![image](https://user-images.githubusercontent.com/54658614/231663415-11971d1f-ac19-4bc1-b095-6e83c56d93f2.png)


#### Gogek.java 클래스 만들기
```
package vo;

public class GogekVO {
	private int gobun;
	private String goname;
	private String goaddr;
	private String gojumin;
	
	public int getGobun() {
		return gobun;
	}
	public void setGobun(int gobun) {
		this.gobun = gobun;
	}
	public String getGoname() {
		return goname;
	}
	public void setGoname(String goname) {
		this.goname = goname;
	}
	public String getGoaddr() {
		return goaddr;
	}
	public void setGoaddr(String goaddr) {
		this.goaddr = goaddr;
	}
	public String getGojumin() {
		return gojumin;
	}
	public void setGojumin(String gojumin) {
		this.gojumin = gojumin;
	}
}

```

#### 고객 테이블 출력해보기 gogek.jsp
```
<%@page import="java.sql.ResultSet"%>
<%@page import="java.sql.PreparedStatement"%>
<%@page import="java.util.ArrayList"%>
<%@page import="java.util.List"%>
<%@page import="vo.GogekVO"%>
<%@page import="javax.naming.Context"%>
<%@page import="java.sql.Connection"%>
<%@page import="javax.sql.DataSource"%>
<%@page import="javax.naming.InitialContext"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
    
<%
	InitialContext ic = new InitialContext();
	Context ctx = (Context)ic.lookup("java:comp/env");
	DataSource ds = (DataSource)ctx.lookup("jdbc/oracle_test");
	Connection conn = ds.getConnection();//db에 접속
	
	//고객 테이블 조회
	String sql = "select * from gogek";
	PreparedStatement pstmt = conn.prepareStatement(sql);
	
	//위에서 수행한 쿼리문을 기반으로 결과를 한줄씩 가져오기
	ResultSet rs = pstmt.executeQuery();
	
	//고객목록을 저장할 ArrayList
	List<GogekVO> gogek_list = new ArrayList<GogekVO>();
	
	while(rs.next()){
		GogekVO vo = new GogekVO();
		vo.setGobun(rs.getInt("gobun"));
		vo.setGoname(rs.getString("goname"));
		vo.setGoaddr(rs.getString("goaddr"));
		vo.setGojumin(rs.getString("gojumin"));
		gogek_list.add(vo);
	}
	
	//DB연결에 사용된 객체는 모두 닫아줘야 한다.
	rs.close();
	pstmt.close();
	conn.close();


%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<table border="1">
		<tr>
			<th>번호</th>
			<th>이름</th>
			<th>주소</th>
			<th>주민번호</th>
		</tr>
		<%for(int i =0; i<gogek_list.size(); i++){ 
				GogekVO vo = gogek_list.get(i);%>
			<tr>
				<td><%= vo.getGobun() %></td>
				<td><%= vo.getGoname() %></td>
				<td><%= vo.getGoaddr() %></td>
				<td><%= vo.getGojumin() %></td>
			</tr>			
		<%} %>		
	
	</table>
</body>
</html>

```
