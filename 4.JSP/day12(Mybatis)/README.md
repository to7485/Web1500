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

