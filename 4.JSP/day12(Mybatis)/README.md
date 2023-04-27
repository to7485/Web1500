# Mybatis
- Mybatis는 자바 오브젝트와 SQL사이의 자동 매핑 기능을 지원하는 ORM(Object relational Mapping)프레임워크이다.
- SQL을 별도의 파일로 분리해서 관리하게 해준다.
- Mybatis를 사용하지 않고 직접 JDBC를 사용할 경우의 문제점
    - 개발자가 반복적으로 작성해야할 코드가 많고, 서비스 로직 코드와 쿼리를 분리하기 어렵다.
    - 또한 커넥션 풀의 설정 등 개발자가 신경써야할 부분이 많아 여러가지 어려움이 있다.

### Mybatis의 특징
1. 쉬운 접근성과 코드의 간결함 JDBC의 모든 기능을 Mybatis가 대부분 제공한다.<br>복잡한 JDBC코드를 걷어내며 깔끔한 소스코드를 유지할 수 있다.<br>수동적인 파라미터 설정과 쿼리 결과에 대한 맵핑 구문을 제거할 수 있다.

2. SQL문과 프로그래밍 코드의 분리<br>SQL에 변경이 있을 때마다 자바 코드를 수정하거나 컴파일하지 않아도 된다.

3. 다양한 프로그래밍 언어로 구현가능<br>Java, C#, .NET, Ruby

### mybatis프레임워크를 사용하기 위한 라이브러리 다운로드

https://mvnrepository.com/artifact/org.mybatis/mybatis

### Mybatis 주요 컴포넌트 역할
- Mybatis 설정파일(SqlMapConfig.xml) : 데이터베이스의 접속 주소정보나 Mapping파일의 경로등의 고정된 환경정보를 설정한다.
- SqlSessionFactoryBuilder : Mybatis설정파일을 바탕으로 SqlSessionFactory를 생성한다.
- SqlSessionFactory : SqlSession을 생성한다.
- SqlSession : 핵심적인 역할을 하는 클래스로서 SQL 실행이나 트랙잭션 관리를 실행한다.<br> SqlSession 오브젝트는 Thread-Safe 하지 않으므로 thread마다 필요에 따라 생성한다.
- Mapping 파일( Dept.xml) : SQL문과 OR Mapping을 설정한다.

## Ex_날짜_mybatis 프로젝트 생성하고 DB연결을 위한 파일들 옮겨넣기
- service패키지는 옮기지 않는다.

![image](https://user-images.githubusercontent.com/54658614/234179498-cfd05c05-f5ca-405a-a7ae-ce36d9f70748.png)

### config.mybatis패키지 생성하고 sqlMapConfig.xml 복사해 넣기
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<environments default="">
		<environment id="">
			<transactionManager type="JDBC" />
			
			Mybatis 사용을 위해 JNDI를 찾아주는 코드
			<dataSource type="JNDI">
				<property name="data_source" 
				value="java:comp/env/jdbc/oracle_test" />
			</dataSource>
		</environment>
	</environments>
	<mappers>
		
		<mapper resource="config/mybatis/mapper/sawon.xml" />
	</mappers>
</configuration>
```
### confing.mybatis.mapper 패키지를 생성하고 sawon.xml 복사해서 넣기

### service 패키지 생성하고 MybatisConnector.java 클래스 생성
1. SqlSessionFactoryBuilder가 Mybatis 설정파일을 참고하여 SqlSessionFactory를 생성합니다.
2. 웹단에서 데이터 접근 작업시 SqlSessionFactory는 매 요청마다 SqlSession객체를 생성한다.
3. SqlSession객체를 통해 DB작업을 진행하는데, 작업시 수행하는 쿼리는 mapper파일에 담겨있다.

```java
package service;

import java.io.IOException;
import java.io.Reader;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

public class MyBatisConnector {
	//Mybaatis는 데이터베이스 프로그래밍을 좀 더 쉽고 간단하게 할 수 있도록 도와주는
	//프레임워크
	
	SqlSessionFactory factory = null;

	// single-ton pattern: 
	// 객체1개만생성해서 지속적으로 서비스하자
	static MyBatisConnector single = null;

	public static MyBatisConnector getInstance() {
		//생성되지 않았으면 생성
		if (single == null)
			single = new MyBatisConnector();
		//생성된 객체정보를 반환
		return single;
	}
	
	public MyBatisConnector(){  //--> 기본생성자 생성
		//생성자로 하고싶은 내용 sqlMaConfig.xml을 읽어온다.
    		//sqlMapConfig : mybatis에 대한 정보를 담고있는 설정파일
		
		try {//reader로 읽어왔을 때 경로가 없을 수 있으니 try-catch로 묶어준다.
	       //xml이 뭔지는 잘 모르지만context.xml을 봤듯이 뭔가를 설정하는 파일

		//reader라고 하면 캐릭터 기반의 스트림으로 읽어온다고 생각하면 된다. (""로 쌓인 문자열로 읽어온다)
		//이미지 파일이 아니기 때문에 2byte씩 읽어와도 문제가 없다.
		//import org.apache.ibatis.io.Resources;
		Reader reader = Resources.getResourceAsReader("config/mybatis/sqlMapConfig.xml");

		//config/mybatis/sqlMapConfig.xml로 들어가보자!!!

		//위에서 읽어온 sqlMapConfig.xml에서 지정해둔 DB접근 경로를실제로 읽어온다.
		factory = new SqlSessionFactoryBuilder().build(reader);
			//이제 팩토리는 sqlMapConfig.xml에 있는 내용을 분석해서 갖고 있는다
		
				reader.close();
			} catch (IOException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
		}
	
	//sqlMapConfig.xml의 정보를 담고있는 factory객체를 반환 (getter임)
	public SqlSessionFactory  getSqlSessionFactory() { 
		return factory;
	}
}

```
### 부서테이블을 조회할 것이기 때문에 sawon.xml 파일이름을 dept.xml로 바꾸자.
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="">
<!--mapper : DB에 쿼리문을 요청하고 결과를 돌려받는 속성파일--> ,


</mapper>
```

#### sqlMapConfig.xml의 <mapper>의 개수와 mapper파일의 개수는 일치해야 한다.

### vo패키지에 DeptVO 클래스 만들기

```java
package vo;

public class DeptVO {
	private int deptno;
	private String dname,loc;
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

### dao패키지에 DeptDAO 클래스 만들기

```java
package dao;

import org.apache.ibatis.session.SqlSessionFactory;

import service.MyBatisConnector;

public class DeptDAO {

	SqlSessionFactory factory;
	// single-ton pattern: 
	// 객체1개만생성해서 지속적으로 서비스하자
	static DeptDAO single = null;

	public static DeptDAO getInstance() {
		//생성되지 않았으면 생성
		if (single == null)
			single = new DeptDAO(); //최초 1회 객체가 생성될 때 factory가 비어있으면 안된다.
		//생성된 객체정보를 반환
		return single;
	}
	
	public DeptDAO() {//기본생성자에서 MyBatisConnetor 호출하여 factory를 채워주자.
		factory = MyBatisConnector.getInstance().getFactory();
	}
	
	//부서테이블 조회
	//이제는 메서드로 작성을 한다.(메서드의 이름은 마음대로 쓸 수 있다.)
	public List<DeptVO> select(){
		//factory는 어떤 db로 연결하고 어떤 mapper로 접근해야 하는것 까지만 알고 있다.
		//그 정보를 가지고 실제로 쿼리문을 요청하는 것은 SqlSession객체가 한다. 
		SqlSession sqlSession = factory.openSession();
		
		//sqlSession에 있는 메서드는 이름을 막 지으면 안된다.
		//dept. -> 식별자 mapper의 namespace
		//dept_list -> select 태그의 id값
		List<DeptVO> list = sqlSession.selectList("dept.dept_list");
		
		sqlSession.close(); //DB접근을 위해 사용한 sqlSession은 마지막에 꼭 닫아줄것!
		
		return list;
	}
}

```
### dept.xml에 내용추가하기

이전에 우리가 db에서 select를 해서 값을 가져올때 vo 객체에 담아서 list에 add를 한 후 return을 한다.<br>
Mybatis에서는 mapper에서 쿼리문이 실행이 되고 얻는 한건의 데이터를 vo에 담아야 한다.<br>

그래서 select 태그에는 resultType이라는 속성이 반드시 필요하다.<br>
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="dept">

패키지명까지 반드시 명시해줘야 한다.
<select id="dept_list" resultType="vo.DeptVO">
	select * from dept (절대로 ;는 찍지말것)
</select>

</mapper>
```

### DeptListAction 서블릿 생성하기

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

import dao.DeptDAO;
import vo.DeptVO;

/**
 * Servlet implementation class DeptListAction
 */
@WebServlet("/dept_list.do")
public class DeptListAction extends HttpServlet {
	private static final long serialVersionUID = 1L;

    /**
     * Default constructor. 
     */
    public DeptListAction() {
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see HttpServlet#service(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		
		//부서목록 가져오기
		List<DeptVO> list = DeptDAO.getInstance().select();
		
		//바인딩
		request.setAttribute("list", list);
		
		//포워딩
		RequestDispatcher disp = request.getRequestDispatcher("dept_list.jsp");
		disp.forward(request, response);
	}

}
```

### dept_list.jsp 생성하기
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<table border="1">
		<caption>:::부서목록</caption>
		<tr>
			<th>부서번호</th>
			<th>부서이름</th>
			<th>부서위치</th>
		</tr>
		<c:forEach var="vo" items="${list}">
		<tr>
			<td>${vo.deptno}</td>
			<td>${vo.dname}</td>
			<td>${vo.loc}</td>
		</tr>
		</c:forEach>
	</table>

</body>
</html>
```
## 사원 테이블도 Mybatis를 이용하여 조회해보자.

### SawonVO 생성하기
```java
package vo;

public class SawonVO {
	private int sabun, sapay;
	private String saname, sajob, sahire;
	
	public int getSabun() {
		return sabun;
	}
	public void setSabun(int sabun) {
		this.sabun = sabun;
	}
	public int getSapay() {
		return sapay;
	}
	public void setSapay(int sapay) {
		this.sapay = sapay;
	}
	public String getSaname() {
		return saname;
	}
	public void setSaname(String saname) {
		this.saname = saname;
	}
	public String getSajob() {
		return sajob;
	}
	public void setSajob(String sajob) {
		this.sajob = sajob;
	}
	public String getSahire() {
		return sahire;
	}
	public void setSahire(String sahire) {
		this.sahire = sahire;
	}
}
```

### SawonDAO 만들기

```java
package dao;

import java.util.List;

import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;

import service.MyBatisConnector;
import vo.SawonVO;

public class SawonDAO {
	SqlSessionFactory factory;

	public SawonDAO() {
		factory = MyBatisConnector.getInstance().getFactory();
	}

	// single-ton pattern: 
	// 객체1개만생성해서 지속적으로 서비스하자
	static SawonDAO single = null;

	public static SawonDAO getInstance() {
		//생성되지 않았으면 생성
		if (single == null)
			single = new SawonDAO();
		//생성된 객체정보를 반환
		return single;
	}
	
	//사원목록 조회
	public List<SawonVO> select(){
		//SQL을 요청할 SqlSession객체 생성
		SqlSession sqlSession = factory.openSession();
		
		List<SawonVO> list = sqlSession.selectList("sawon.sawon_list");
		
		//conn, pstmt, rs를 close()하는 내용이 포함되어 있다.
		//그러므로 사용후에는 반드시 마지막에 닫아줘야 한다.
		sqlSession.close();
		
		return list;
		
	}
}
```

### mapper sawon.xml파일 만들기

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="sawon">

<!-- select tag 에는 reultType을 무조건 설정해줘야 한다. -->
<select id="sawon_list" resultType="vo.SawonVO">
	select * from sawon
</select>
</mapper>
```
### sqlMapConfig.xml에 매퍼 추가하기
```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
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
		<mapper resource="config/mybatis/mapper/dept.xml" />
		<mapper resource="config/mybatis/mapper/sawon.xml" />
	</mappers>
</configuration>
```

### SawonListAction 만들기

```java

import dao.SawonDAO;
import vo.SawonVO;

/**
 * Servlet implementation class SawonLIstAction
 */
@WebServlet("/sawon_list.do")
public class SawonLIstAction extends HttpServlet {
	private static final long serialVersionUID = 1L;

	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

		//사원 목록 조회
		List<SawonVO> list = SawonDAO.getInstance().select();

		//바인딩		
		request.setAttribute("list", list);
		
		//포워딩
		RequestDispatcher disp = request.getRequestDispatcher("sawon_list.jsp");
		disp.forward(request, response);
	}

}
```

### sawon_list.jsp 작성

```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<%@ taglib prefix="fn" uri="http://java.sun.com/jsp/jstl/functions"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<table border="1">
		<catption>:::사원목록</catption>
		<tr>
			<th>사번</th>
			<th>이름</th>
			<th>직책</th>
			<th>급여</th>
			<th>입사일</th>
		</tr>
		<c:forEach var="vo" items="${list}">
			<tr>
				<td>${vo.sabun }</td>
				<td>${vo.saname }</td>
				<td>${vo.sajob }</td>
				<td>${vo.sapay }</td>
				<td>${vo.sahire }</td>
				<c:set var="sahire" value="${vo.sahire }"/>
				<td>${fn:split(sahire,' ')[0]}</td>
			</tr>
		</c:forEach>
	</table>

</body>
</html>
```

## 문제 : 고객 테이블을 출력해보기

![image](https://user-images.githubusercontent.com/54658614/234480154-00f7f0bc-492e-4ee0-914d-b0a728d20d8f.png)


### GogekVO 클래스 생성하기
```java
public class GogekVo {
	
	int gobun;//고객번호
	int godam;//담당자번호
	String goname;//고객이름
	String goaddr;//고객주소
	String gojumin;//고객 주민번호	
	
	public int getGobun() {
		return gobun;
	}
	public void setGobun(int gobun) {
		this.gobun = gobun;
	}
	public int getGodam() {
		return godam;
	}
	public void setGodam(int godam) {
		this.godam = godam;
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

### GogekDAO 클래스 생성

```java
public class GogekDao {

	// single-ton템플릿: 
	static GogekDao single = null;

	public static GogekDao getInstance() {
		//생성되지 않았으면 생성
		if (single == null)
			single = new GogekDao();
		//생성된 객체정보를 반환
		return single;
	}

	//SessionFactory생성하는 객체
	SqlSessionFactory factory;

	public GogekDao() {
		super();
		//MyBatisConnector.java에서 설정해준 factory(어떤DB를 쓸것인가, mapper로 누구를 쓸것인가 등)를 DAO로 가져온다.
		factory = MyBatisConnector.getInstance().getSqlSessionFactory();
	}

	//고객목록 가져오기
	public List<GogekVo> select(){

		List<GogekVo> list = null;

		//sql을 요청할 SqlSession객체 생성
		//1.처리객체 얻어오기
		SqlSession sqlSession = factory.openSession();

		//2.처리객체를 사용하여 작업을 수행
		//mapper에 있는 해당 id의 sql명령을 실행 후 결과를 list로 반환
		list = sqlSession.selectList("gogek.gogek_list");

		//3.사용 후에는 반환(connection, pstmt, resultMap등을 반환하는 작업이 내장되어 있음)
		sqlSession.close();

		return list;
	}//selectList()
}
```

### GogekListAction 서블릿 생성하기
```
@WebServlet("/gogek/gogeklist.do")  <-- 반드시 변경!!!

public class GogekListAction extends HttpServlet {
	private static final long serialVersionUID = 1L;
	
	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		
		//목록 가져오기
		List<GogekVo> list = GogekDao.getInstance().select();
		
		//System.out.println(list.size());	
		
		//requestScope영역에 list 바인딩
		request.setAttribute("list", list);
		
		//member_list.jsp에서 el기법을 사용할수 있도록 하기 위해
		//위에서 바인딩해준 list정보를 넘겨준다.
		RequestDispatcher disp = 
				request.getRequestDispatcher("gogek_list.jsp");

		disp.forward(request, response);
	}

}
```

### gogek_list.jsp 생성하기

```
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>

<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>


<body>
	
	<table border="1" align="center">
		<caption>고객목록</caption>
		<tr>
			<th>고객번호</th>
			<th>담당자</th>
			<th>이름</th>
			<th>주소</th>
			<th>주민번호</th>
		</tr>
	
	<!-- 검색결과가 있다면 내용 출력 -->
	<c:if test="${!empty requestScope.list}" >
	
		<c:forEach var="vo" items="${list}">
			<tr>
				<td>${vo.gobun}</td>
				<td>${vo.godam}</td>
				<td>${vo.goname}</td>
				<td>${vo.goaddr}</td>
				<td>${vo.gojumin}</td>
			</tr>
		</c:forEach>
	
	</c:if>
	
	</table>
</body>
</html>
```
# Mybatis 파라미터 처리하기

## 전체 사원 목록을 조회하던 페이지에서 원하는 부서의 사람만 볼수 있도록 해보기

![image](https://user-images.githubusercontent.com/54658614/234757788-6460e8ed-da52-4d55-951f-d7b5818f107c.png)

### sawon_list.jsp에 태그 추가하기
```
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>

<html>

<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>

<script>
	function find(){
		//select태그의 value값(총무부가 선택됐다면 value=10)을 가져온다.
		//where절이 있는 쿼리문을 작성해야 한다!
		//선택된 값을 가져와서 sawon_list.do로 넘긴다. 0 ~ 50
		var deptno = document.getElementById("deptno").value;	
		
		//form태그를 사용하고 있지 않기 때문에 location.href를 통해서 보낸다.
		location.href = "sawon_list.do?deptno=" + deptno;
		
	}//find()
	
</script>
</head>
<body>
	
	자바에서 gui 할 때 배웠던 초이스 박스와 비슷한 모양
	<div align="center">
		부서 번호:
		<select id="deptno"> 검색해서현재 셀렉트에서 선택된 값을 전달하기위해 id를 설정한다.
			<option value="0">:::부서를 선택하세요:::</option>
			<option value="10">총무부</option>
			<option value="20">영업부</option>
			<option value="30">전산실</option>
			<option value="40">관리부</option>
			<option value="50">경리부</option>			
		</select>
		select가 form 태그에 들어갈 수 있는 정식적인 자식요소가 아니여서 사용하지 않았음
		<input type="button" value="검색" onclick="find();">
	</div>
	
	<hr>
	
	<table border="1" align="center">
		<caption>사원목록</caption>
		..............
	</table>
	
</body>

</html>
```

### SawonListAction 코드 추가하기

```java
@WebServlet("/sawon_list.do")

public class SawonListAction extends HttpServlet {
	private static final long serialVersionUID = 1L;

	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
	
		//sawon_list.do?deptno=10
		int deptno = 0;
		String str_deptno = request.getParameter( "deptno" );

		//콘솔에 null이 찍힘
		System.out.println(str_deptno); 
		
		//파라미터를 String으로 받은 이유
		//int deptno = Integer.parseInt(request.getParameter("deptno"); 에서 null은 정수로 바꿀수 없기 때문에  오류가 난다.
		
		//sawon.do? <--- null 
		//sawon.do?deptno= <-- empty
		if( str_deptno != null && !str_deptno.isEmpty() ){
			
			//파라미터가 없거나 비어있는 상태가 아닌 정상적으로 값이 전달되었다면
			//실제 정수로 바꿔준다.
			deptno = Integer.parseInt(str_deptno);
			
		}
		
		//목록 가져오기
		List<SawonVO> list = null;
			
		
		if(deptno == 0){//전체조회
			list = SawonDAO.getInstance().select();
		}else{
			//파라미터에 해당하는 부서별 조회
			list = SawonDAO.getInstance().select(deptno); <-- 없다. 만들자!! 메서드 오버로드
		}
		
		//System.out.println(list.size());
		
		request.setAttribute("list", list);
		
		//member_list.jsp에서 el기법을 사용할수 있도록 하기 위해
		//위에서 바인딩해준 list정보를 넘겨준다.
		RequestDispatcher disp = 
				request.getRequestDispatcher("sawon_list.jsp");

		disp.forward(request, response);
	}

}
```
									  
### SawonDAO에 메서드 오버로딩하기

```
public class SawonDao {

	static SawonDao single = null;
	SqlSessionFactory factory;

	public SawonDao() {
		super();
		............
	}

	public static SawonDao getInstance() {
		...........
		return single;
	}

	//사원목록 가져오기
	public List<SawonVo> select(){
		.............
		return list;
	}//select()
	
	//부서별 사원 목록
	public List<SawonVO> select( int deptno ) {

		//mybatis framwork이 처리해준다.
		//1.처리객체 얻어오기
		SqlSession sqlSession = factory.openSession();

		//2.처리객체를 사용하여 작업을 수행
		//mapper에 있는 해당 id의 sql명령을 실행 후 결과를 list로 반환
		//sqlSession이 관리하는 CRUD관련 메서드는 파라미터를 추가할 수 있다.
		//파라미터는 단 한 개만 추가할 수 있다.
		List<SawonVO> list = sqlSession.selectList("sawon.sawon_list_no", deptno); 
		//같은 쿼리문을 사용할것이 아니기 때문에 id값을 다르게 쓴다.

		//3.사용 후에는 반환(connection, pstmt, resultMap등을 반환하는 작업이 내장되어 있음)
		sqlSession.close();

		return list;

	} //select()

}
```

### sawon.xml에 쿼리문 추가하기

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="sawon">

	<select id="sawon_list" resultType="vo.SawonVo">
		select * from sawon
	</select>
	
	<select id="sawon_list_deptno" resultType="vo.SawonVo" parameterType="int">
	<!-- parameterType이 (Map이나 List가 아니라) int나 String등 단일자료형일때는 deptno=#{ aaa }처럼 아무 이름이나 넣어줘도 된다.-->
	pstmt처럼 채워주는게 없어서 ?를 쓸 수없다. 왠만하면 컬럼명이랑 이름 맞춰주자
		select * from sawon where deptno=#{ deptno }
	</select>

	<delete id="sawon_delete" parameterType="int">
		delete from sawon where sabun=#{ sabun } 
	</delete>
</mapper>
```
